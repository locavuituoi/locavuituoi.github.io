---
published: true
publish: true
layout: post
author: sal
categories:
  - Wordpress
tags:
  - Code
image: assets/images/code.jpg
title: 10 Thủ thuật cực hữu ích với function.php trong Wordpress
---

## 1. Xoá Widget không mong muốn trên màn hình dashboard

Chúng ta cần xác định **normal** và **side** bằng cách dùng F12.

```php
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

```php
//Turn Off Notifications
function remove_core_updates(){
        global $wp_version;return(object) array('last_checked'=> time(),'version_checked'=> $wp_version,);
    }
    add_filter('pre_site_transient_update_core','remove_core_updates');
    add_filter('pre_site_transient_update_plugins','remove_core_updates');
    add_filter('pre_site_transient_update_themes','remove_core_updates');
```

## 3. Thêm CSS tuỳ chỉnh trong trang quản trị

Bạn chỉ cần viết CSS trong thẻ style.

```php
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

Mình đã đổi /portfolio thành /du-an

```php
//Change Slug
add_filter( 'register_post_type_args', 'wpse247328_register_post_type_args', 10, 2 );
function wpse247328_register_post_type_args( $args, $post_type ) {
	if ( 'portfolio' === $post_type ) {
		$args['rewrite']['slug'] = 'du-an';
	}
	return $args;
```

## 5. Chuyển hướng

```php
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

Mình thấy có khá nhiều blog chia sẻ về cách dịch từ với function.php. Nhưng cách dịch này khiến thay đổi toàn bộ từ ngữ trên trang web. Ví dụ mình dịch từ All -> Tất cả thì từ Install -> InstTấtcả. Và đây là cách khắc phục:

```php
//Translate Elementor
function my_text_strings( $translated_text, $text, $domain ) {
	switch ( $translated_text ) {
		case 'All' :
			$translated_text = __( 'Tất cả', 'elementor' ); //Change text domain
			break;
	}
	return $translated_text;
}
add_filter( 'gettext', 'my_text_strings', 20, 3 );
```

Bạn hãy thay đổi **elementor** thành text domain của bạn. Vậy làm thế nào để tìm text domain?
Ví dụ: trong đường dẫn wp-content/plugins/woocommerce/ thì woocommerce chính là text domain của bạn.

## 7. Cài đặt SMTP Gmail

```php
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

## 8. Nhân bản bài viết trong Post Type bất kỳ

Hãy thay đổi **portfolio** trong đoạn **if ($post->post_type=='portfolio'** thành post type bất kỳ.

```php
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

```php
//Disable Elementor Google Fonts
add_filter( 'elementor/frontend/print_google_fonts', '__return_false' );
```

## 10. Ẩn trang đăng nhập WordPress không cần plugin

Hãy thay đổi 'newlogin' (đường dẫn đăng nhập example.com/wp-login.php?newlogin) với 'NonExistentPage' (trang chuyển hướng khi cố tình truy cập wp-admin).

```php
function redirect_to_nonexistent_page(){

     $new_login=  'newlogin';
    if(strpos($_SERVER['REQUEST_URI'], $new_login) === false){
                wp_safe_redirect( home_url( 'NonExistentPage' ), 302 );
      exit(); 
    }
 }
add_action( 'login_head', 'redirect_to_nonexistent_page');

function redirect_to_actual_login(){
 
  $new_login =  'newlogin';
  if(parse_url($_SERVER['REQUEST_URI'],PHP_URL_QUERY) == $new_login&& ($_GET['redirect'] !== false)){     
                 wp_safe_redirect(home_url("wp-login.php?$new_login&redirect=false"));
     exit();
 
  }
}
add_action( 'init', 'redirect_to_actual_login');
```
