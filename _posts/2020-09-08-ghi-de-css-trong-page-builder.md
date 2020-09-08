---
published: true
layout: post
author: sal
categories:
  - Wordpress
tags:
  - Code
image: assets/images/code.jpg
title: Ghi đè CSS trong Page Builder trên Wordpress
---
## Làm thế nào để sửa element.style

Khi làm việc với WP Bakery tôi phát hiện ra 1 đoạn code phát sinh và 'hơi' khó sửa như sau:

```html
<div class="col-md-6 col-sm-6 col-xs-12 masonry-item biet-thu-nha-lien-ke nghi-duong shophouse-officetel" style="position: absolute; left: 0px; top: 279px;">
```

Dùng F12, đây là những gì tôi thấy:

```css
element.style {
    position: absolute;
    left: 0px;
    top: 279px;
}
````

Tôi không biết top: 279px chui ở đâu ra. Nhưng thực sự tôi muốn sửa top: 279px thành top: 228px để đảm bảo tính thẩm mỹ. Vì vậy tôi dùng bộ chọn _selector [style^="property:value"]_:

```css
div [style^="position: absolute; left: 0px; top: 279px;"]{top:228px !important;}
```

# Ghi đè thuộc tính ưu tiên !important

Không biết thằng dở người nào làm cái theme khó xơi vl, bỏ cả cái thuộc tính !important vào, bảo sao mãi không sửa được. Vì vậy tôi đành phải sửa trên file function.php. Đoạn code tôi đã sửa như sau:

```php
// Fix Piority CSS.
function fix_bad_theme_styles() { ?>
    <style data-type="vc_shortcodes-custom-css">.vc_custom_1419240516480{box-shadow: none;background-color:unset!important;margin-bottom:-60px;}</style>
<?php }

add_action('wp_footer', 'fix_bad_theme_styles');
```
