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

The mind-warping film opened with a hospital patient Simon Cable (Ryan Phillippe) awakening in a <span class="spoiler"> hospital with little knowledge (amnesia perhaps?) of what had happened, and why he was there, etc. He was told by attending Dr. Jeremy Newman (Stephen Rea) that it was July 29, 2002 (Simon thought it was the year 2000 - he was confused - he heard a doctor say 20:00 hours!) and that he had died for two minutes from cardiac arrest following the near-fatal accident -- but he had been revived ("You're as good as new").</span> Dr. Newman: "Simon, this is the 29th of July. The year is 2002. And your wife, whose name is Anna, is waiting outside."

> It was decorated in an 18th-century rococo style, redesigned by Sybille de Margérie with furnishings by Sonia Rykiel.

#### Hãy copy đoạn code này và dán vào function.php

```html
//Ẩn menu trong trang quản trị cho từng người dùng
add_action( 'admin_init', 'my_remove_menu_pages' );
function my_remove_menu_pages() {
  global $user_ID;
  if(is_admin() && $user_ID == '5;6'){ //Thay ID người dùng ở đây
        remove_menu_page('edit.php?post_type=blocks');
        remove_menu_page('wpcf7');
    }
}
```
