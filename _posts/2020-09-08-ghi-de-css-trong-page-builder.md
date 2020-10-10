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

Khi làm việc với WP Bakery mình phát hiện ra 1 đoạn code phát sinh và 'hơi' khó sửa như sau:

```html
<div class="col-md-6 col-sm-6 col-xs-12 masonry-item biet-thu-nha-lien-ke nghi-duong shophouse-officetel" style="position: absolute; left: 0px; top: 279px;">
```

Dùng F12, đây là những gì mình thấy:

```css
element.style {
    position: absolute;
    left: 0px;
    top: 279px;
}
````

Không có id, không có class, chơi khó nhau vl. Vì vậy mình dùng bộ chọn thuộc tính CSS (Attribute CSS Selector). Dưới đây là các cấu trúc của bộ chọn này:
- Bộ chọn thuộc tính cơ bản: **A[B]**. Chọn tất cả các phần tử **A** có thuộc tính **B**.
- Bộ chọn thuộc tính cụ thể: **A[B="C"]**. Chọn các phần tử **A** có thuộc tính **B** với giá trị là **C**.
- Bộ chọn thuộc tính cụ thể (Bắt đầu với...): **A[B^="C"]**. Chọn tất cả các phần tử **A** có thuộc tính **B** với giá trị bắt đầu là **C**. Ký tự **^** là ký tự thể hiện chuỗi bắt đầu (trong Biểu thức chính quy (Regex)).
- Bộ chọn thuộc tính cụ thể (Kết thúc với...): **A[B$="C"]**. Chọn tất cả các phần tử **A** có thuộc tính **B** với giá trị kết thúc là **C**. Ký tự **$** là ký tự thể hiện chuỗi kết thúc trong (Biểu thức chính quy (Regex)).
- Bộ chọn thuộc tính cụ thể (Chứa ký tự...): **A[B*="C"]**. Chọn tất cả phần tử **A** với thuộc tính **B** chứa giá trị **C**.

Vì vậy để sửa đoạn code trên mình sửa thành:
```css
div [style$="top: 279px;"]{top:228px !important;}
```

# Ghi đè thuộc tính ưu tiên !important

Không biết thằng dở người nào làm cái theme khó xơi vl, bỏ cả cái thuộc tính !important vào, bảo sao mãi không sửa được. Vì vậy mình đành phải sửa trên file function.php. Đoạn code mình đã sửa như sau:

```php
// Fix Piority CSS.
function fix_bad_theme_styles() { ?>
    <style data-type="vc_shortcodes-custom-css">.vc_custom_1419240516480{box-shadow: none;background-color:unset!important;margin-bottom:-60px;}</style>
<?php }

add_action('wp_footer', 'fix_bad_theme_styles');
```
