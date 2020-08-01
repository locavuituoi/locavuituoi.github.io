---
layout: post
title: Cách ẩn menu trong trang Quản trị Wordpress cho từng người dùng
author: sal
categories:
  - Wordpress
tags:
  - Thủ thuật
image: assets/images/2.jpg
rating: 4.5
published: true
---
Giả sử mình đang có một vài bạn làm cộng tác viên viết bài tuy nhiên khi các bạn này đăng nhập vào website sẽ nhìn thấy một số mục như UX Block, Contact Form... Để đảm bảo sự riêng tư, mình sẽ phải ẩn các mục này đi để các bạn cộng tác viên không nhìn thấy.

Hoặc bạn thiết kế website cho khách hàng và không muốn khách hàng truy cập một số mục. Bạn cần ẩn những mục này đi. Vậy làm thế nào?

#### Hãy chèn đoạn code này vào function.php
Bạn vào **Appearance -> Editor** và tìm file **functions.php**, sau đó chèn đoạn code này vào cuối file.
```php
//Ẩn menu trong trang quản trị cho từng người dùng
add_action( 'admin_init', 'my_remove_menu_pages' );
function my_remove_menu_pages() {
  global $user_ID;
  if(is_admin() && $user_ID == '5;6'){ //Thay ID người dùng ở đây
        remove_menu_page('edit.php?post_type=blocks'); // Ẩn UX Block của Flatsome
        remove_menu_page('wpcf7'); // Ẩn Contact Form 7
    }
}
```
#### Hướng dẫn sử dụng
Sau khi chèn đoạn code trên vào function.php, bạn tiến hành tuỳ chỉnh theo ý muốn của mình.
#####  1. Tìm ID người dùng

Để tìm ID người dùng, bạn vào **User -> All users**, sau đó nhấp vào người dùng mà bạn muốn ẩn menu. Bạn sẽ thấy ID người dùng trên thanh địa chỉ của trình duyệt.

_Ví dụ: domain/wp-admin/user-edit.php?**userid=5**&wphttpreferer=%2Fwp-admin%2Fusers.php. Trường hợp này ID người dùng là 5_

#####  2. Ẩn menu bạn muốn

Trước tiên bạn hãy nhấp vào menu bạn muốn ẩn, sau đó nhìn lên thanh địa chỉ của trình duyệt. Nếu URL chứa **admin.php** bạn chỉ cần copy đoạn đằng sau dấu bằng.

_Ví dụ: domain/wp-admin/**admin.php**?page=**wpcf7** -> **wpcf7**_

Trường hợp còn lại, nếu URL không chứa **admin.php**, các bạn copy đoạn đằng sau **wp-admin/**, ví dụ:

1. domain/wp-admin/themes.php -> **themes.php**
1. domain/wp-admin/edit.php?posttype=sidebar -> **edit.php?posttype=sidebar**
1. domain/wp-admin/edit.php?posttype=events -> **edit.php?posttype=events**_

Sau khi tuỳ chỉnh đoạn code sẽ trông như thế này

```php
//Ẩn menu trong trang quản trị cho từng người dùng
add_action( 'admin_init', 'my_remove_menu_pages' );
function my_remove_menu_pages() {
  global $user_ID;
  if(is_admin() && $user_ID == '5;6'){ //Thay ID người dùng ở đây
  		remove_menu_page('wpcf7'); // Ẩn Contact Form 7
        remove_menu_page('themes.php');
        remove_menu_page('edit.php?posttype=sidebar');
        remove_menu_page('edit.php?posttype=events');
    }
}
```
