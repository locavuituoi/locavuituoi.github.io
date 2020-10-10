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
title: Tạo mục lục Wordpress không cần Plugin
---
Bạn hãy ném đoạn code này vào file function.php

```PHP
// filter function to generate the table of content (from webdeasy.de)
function add_table_of_content($content) {
    ob_start();
    preg_match_all("/<h[2,3](?:\sid=\"(.*)\")?(?:.*)?>(.*)<\/h[2,3]>/", $content, $matches);
    $tags = $matches[0];
    $ids = $matches[1];
    $names = $matches[2];
    ?>
    <ul class="table-of-contents">
        <li><strong>Inhaltsverzeichnis</strong></li>
        <!-- Table of contents by webdeasy.de (LH) -->
        <?php for($i = 0; $i < count($names); $i++) { ?>
            <?php if(strpos($tags[$i], "h2") === false || strpos($tags[$i], "class=\"nitoc\"") !== false) continue; ?>
            
                <li>
                    <?php if(!empty($ids[$i])) { ?>
                        <a href="#<?php echo $ids[$i]; ?>"><?php echo $names[$i]; ?></a>
                    <?php } else { ?>
                        <?php echo $names[$i]; ?>  
                    <?php } ?>
        
                    <?php if($i !== count($names) && strpos($tags[$i+1], "h3") !== false) { ?>
                        <ul>
                            <?php for($j = 0; $j < count($names) - 1; $j++) { ?>
                                <?php $sub_index = $i + $j; ?>
                                <?php if($j != 0 && strpos($tags[$sub_index], "h2") !== false) break; ?>
                                <?php if(strpos($tags[$sub_index], "h3") === false || strpos($tags[$sub_index], "class=\"nitoc\"") !== false) continue; ?>
                                <li>
                                    <?php if(!empty($ids[$sub_index])) { ?>
                                        <a href="#<?php echo $ids[$sub_index]; ?>"><?php echo $names[$sub_index]; ?></a>
                                    <?php } else { ?>
                                        <?php echo $names[$sub_index]; ?>  
                                    <?php } ?>
                                </li>
                            <?php } ?>
                        </ul>
                    <?php } ?>
                </li>
        <?php } ?>
    </ul>
    <?php
    return str_replace("<p>[[MUC_LUC]]</p>", ob_get_clean(), $content);
}
// add our table of contents filter (from webdeasy.de)
add_filter('the_content', 'add_table_of_content');
```

## 2.Thêm một chút CSS cho đẹp

Bạn có thể thêm đoạn CSS này vào CSS Bổ sung (Advanced CSS) trong Tuỳ biến, hoặc style.css của Child theme...

```CSS
ul.table-of-contents {
  padding: 1rem;
  border-left: 3px solid #cecece;
  background-color: #e6e6e6;
}
ul.table-of-contents > li:first-child {
    margin-bottom: 0.5rem;
    font-size: 110%;
}
table-of-contents li {
  list-style: none;
}
```

## 3. Hiển thị Mục lục trên bài viết
Bạn hãy thêm đoạn code này **[[MUC_LUC]]** lên trên đầu bài viết. Để tạo link cho từng tiêu đề của mục lục, hãy trỏ chuột vào Tiêu đề. Sau đó chọn Nâng cao ở sidebar bên phải > Điểm neo HTML > Nhập ký tự làm điểm neo của bạn. Ví dụ: tieu-de-1.

## Bổ sung
Để loại bỏ Tiêu đề khỏi mục lục, trên trình soạn thảo Wordpress, bạn hãy thêm class **nitoc** cho tiêu đề. Các bước như sau: Chọn tiêu đề > Chuột phải > Sửa như là HTML > Thêm class="nitoc" trong thẻ H2 hoặc H3. Ví dụ:

```HTML
<h2 id="tieu-de-1" class="nitoc">Tiêu đề 1</h2>
```