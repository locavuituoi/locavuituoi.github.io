---
published: false
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
## 3. Thêm CSS tuỳ chỉnh

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

