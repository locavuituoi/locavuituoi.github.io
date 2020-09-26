---
published: false
publish: true
layout: post
author: sal
categories:
  - Wordpress
tags:
  - Code
image: assets/images/code.jpg
title: 9 Thủ thuật với function.php trong Wordpress
---

## 1. Xoá Widget tại màn hình dashboard

Chúng ta cần chú ý **normal** và **side**

```
//Dashboard Widgets
function remove_dashboard_widgets() {
    global $wp_meta_boxes;
	remove_meta_box( 'dashboard_primary', 'dashboard', 'side');
	remove_meta_box( 'dashboard_site_health', 'dashboard', 'normal');
	remove_meta_box( 'rank_math_dashboard_widget', 'dashboard', 'normal');
	remove_meta_box( 'dashboard_right_now', 'dashboard', 'normal');
	remove_meta_box( 'dashboard_recent_comments', 'dashboard', 'normal');
	remove_meta_box( 'e-dashboard-overview', 'dashboard', 'normal');
	remove_meta_box( 'jupiterx-dashboard-overview', 'dashboard', 'side');
}
add_action('wp_dashboard_setup', 'remove_dashboard_widgets' );
```

## 2. Tắt thông báo của các plugin

```
//Turn Off Notifications
function remove_core_updates(){
        global $wp_version;return(object) array('last_checked'=> time(),'version_checked'=> $wp_version,);
    }
    add_filter('pre_site_transient_update_core','remove_core_updates');
    add_filter('pre_site_transient_update_plugins','remove_core_updates');
    add_filter('pre_site_transient_update_themes','remove_core_updates');
```

## 3. Thêm CSS tuỳ chỉnh trong trang quản trị

```
//Fix CSS
add_action('admin_head', 'lackim_fix_font_gtbg');
function lackim_fix_font_gtbg() {
  echo '<style>
    .editor-styles-wrapper .wp-block {
    font-family: "Noto Serif", serif !important;}
	.notice-info, .jupiterx-cp-sidebar-list li:nth-child(n) {display:none;}
  </style>';
}
```

## 4. Thay đổi đường dẫn mặc định của Portfolio

```
//Change Slug
add_filter( 'register_post_type_args', 'wpse247328_register_post_type_args', 10, 2 );
function wpse247328_register_post_type_args( $args, $post_type ) {
	if ( 'portfolio' === $post_type ) {
		$args['rewrite']['slug'] = 'du-an-bds';
	}
	return $args;
```

## 5. Chuyển hướng

```
// Redirect
function redirect_page() {
     if (isset($_SERVER['HTTPS']) &&
        ($_SERVER['HTTPS'] == 'on' || $_SERVER['HTTPS'] == 1) ||
        isset($_SERVER['HTTP_X_FORWARDED_PROTO']) &&
        $_SERVER['HTTP_X_FORWARDED_PROTO'] == 'https') {
        $protocol = 'https://';
        }
        else {
        $protocol = 'http://';
    }
    $currenturl = $protocol . $_SERVER['HTTP_HOST'] . $_SERVER['REQUEST_URI'];
    $currenturl_relative = wp_make_link_relative($currenturl);
    switch ($currenturl_relative) {
        case '/du-an-bds/':
            $urlto = home_url('/du-an/');
            break;
		case '/tin-tuc/video-cong-ty/':
            $urlto = home_url('/video-cong-ty/');
            break;
        default:
            return;
    }
    if ($currenturl != $urlto)
        exit( wp_redirect( $urlto ) );
}
add_action( 'template_redirect', 'redirect_page' );
}
```

## 6. Dịch từ ngữ trọn vẹn

```
//Translate Elementor
function my_text_strings( $translated_text, $text, $domain ) {
	switch ( $translated_text ) {
		case 'All' :
			$translated_text = __( 'Tất cả', 'elementor' );
			break;
	}
	return $translated_text;
}
add_filter( 'gettext', 'my_text_strings', 20, 3 );
```

## 7. Cài đặt SMTP Gmail

```
//SMTP
add_action( 'phpmailer_init', 'setup_phpmailer_init' );
function setup_phpmailer_init( $phpmailer ) {
    $phpmailer->Host = 'smtp.gmail.com'; // for example, smtp.mailtrap.io
    $phpmailer->Port = 587; // set the appropriate port: 465, 2525, etc.
    $phpmailer->Username = 'abc@gmail.com'; // your SMTP username
    $phpmailer->Password = 'edzzazzmrozdsdstiar'; // your SMTP password
    $phpmailer->SMTPAuth = true; 
    $phpmailer->SMTPSecure = 'tls'; // preferable but optional
    $phpmailer->IsSMTP();
}

```

## 8. Nhân bản Post Type bất kỳ

Hãy thay đổi portfolio trong đoạn code dưới đây thành post type bất kỳ.

```
//Duplicate Portfolio
function rd_duplicate_post_as_draft(){
	global $wpdb;
	if (! ( isset( $_GET['post']) || isset( $_POST['post'])  || ( isset($_REQUEST['action']) && 'rd_duplicate_post_as_draft' == $_REQUEST['action'] ) ) ) {
		wp_die('No post to duplicate has been supplied!');
	}
 
	if ( !isset( $_GET['duplicate_nonce'] ) || !wp_verify_nonce( $_GET['duplicate_nonce'], basename( __FILE__ ) ) )
		return;
 
	$post_id = (isset($_GET['post']) ? absint( $_GET['post'] ) : absint( $_POST['post'] ) );
	$post = get_post( $post_id );
	$current_user = wp_get_current_user();
	$new_post_author = $current_user->ID;

	if (isset( $post ) && $post != null) {
 
		$args = array(
			'comment_status' => $post->comment_status,
			'ping_status'    => $post->ping_status,
			'post_author'    => $new_post_author,
			'post_content'   => $post->post_content,
			'post_excerpt'   => $post->post_excerpt,
			'post_name'      => $post->post_name,
			'post_parent'    => $post->post_parent,
			'post_password'  => $post->post_password,
			'post_status'    => 'draft',
			'post_title'     => $post->post_title,
			'post_type'      => $post->post_type,
			'to_ping'        => $post->to_ping,
			'menu_order'     => $post->menu_order
		);
 
		$new_post_id = wp_insert_post( $args );
		$taxonomies = get_object_taxonomies($post->post_type);
		foreach ($taxonomies as $taxonomy) {
			$post_terms = wp_get_object_terms($post_id, $taxonomy, array('fields' => 'slugs'));
			wp_set_object_terms($new_post_id, $post_terms, $taxonomy, false);
		}
 
		$post_meta_infos = $wpdb->get_results("SELECT meta_key, meta_value FROM $wpdb->postmeta WHERE post_id=$post_id");
		if (count($post_meta_infos)!=0) {
			$sql_query = "INSERT INTO $wpdb->postmeta (post_id, meta_key, meta_value) ";
			foreach ($post_meta_infos as $meta_info) {
				$meta_key = $meta_info->meta_key;
				if( $meta_key == '_wp_old_slug' ) continue;
				$meta_value = addslashes($meta_info->meta_value);
				$sql_query_sel[]= "SELECT $new_post_id, '$meta_key', '$meta_value'";
			}
			$sql_query.= implode(" UNION ALL ", $sql_query_sel);
			$wpdb->query($sql_query);
		}
 
		wp_redirect( admin_url( 'post.php?action=edit&post=' . $new_post_id ) );
		exit;
	} else {
		wp_die('Post creation failed, could not find original post: ' . $post_id);
	}
}
add_action( 'admin_action_rd_duplicate_post_as_draft', 'rd_duplicate_post_as_draft' );
 
function rd_duplicate_post_link( $actions, $post ) {
	if ($post->post_type=='portfolio' && current_user_can('edit_posts')) { //Post Type
		$actions['duplicate'] = '<a href="admin.php?action=rd_duplicate_post_as_draft&post=' . $post->ID . '" title="Nhân bản dự án này" rel="permalink">Nhân bản</a>';
	}
	return $actions;
}
add_filter( 'post_row_actions', 'rd_duplicate_post_link', 10, 2 );
```

## 9. Vô hiệu hoá Google Fonts trong Elementor

```
//Disable Elementor Google Fonts
add_filter( 'elementor/frontend/print_google_fonts', '__return_false' );
```