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

Within the Hôtel de Crillon, which was built in 1758, Les Ambassadeurs operated as a restaurant since the mid-19th century. It reached its peak of fame as a restaurant and nightclub (a café-concert) in the last three decades of the 19th century. 

Always a center of entertainment for the aristocracy, in the 1870s it also became a regular destination of some of the best known figures of art and the demi-monde. Edgar Degas and Henri de Toulouse-Lautrec portrayed visitors at the night club, and Aristide Bruant performed there.

> It was decorated in an 18th-century rococo style, redesigned by Sybille de Margérie with furnishings by Sonia Rykiel.

#### Hãy copy đoạn code này và dán vào function.php

```php
/*Ẩn menu trong trang quản trị cho từng người dùng*/
add_action( 'admin_init', 'my_remove_menu_pages' );
function my_remove_menu_pages() {
  global $user_ID;
  if(is_admin() && $user_ID == '5;6'){ //Thay ID người dùng ở đây
        remove_menu_page('edit.php?post_type=blocks');
        remove_menu_page('wpcf7');
    }
}
```
