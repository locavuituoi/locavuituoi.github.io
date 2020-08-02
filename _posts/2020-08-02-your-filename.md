---
published: true
layout: post
title: 7 Nguyên tắc cơ bản cần nắm vững khi thiết kế Button
author: john
categories:
  - Thiết kế
tags: UX/UI
image: assets/images/nguyen-hue-dieu-khien-hai-tac.jpg
---
Button là thành phần thiết yếu trong các kiểu thiết kế tương tác ngày nay và chúng cũng chính là cầu nối chính giữa người dùng và toàn bộ hệ thống sản phẩm. Nên để hiểu rõ hơn tầm quan trọng của button, tôi sẽ liệt kê bảy nguyên tắc cơ bản để tạo nên được những button đạt hiệu quả.

#### 1. Dễ nhận ra

Khi người dùng cần tương tác, họ cần được tự xác định được ngay cái gì bấm vào được và cái gì thì không. Thông thường, người dùng sẽ mất một khoảng thời gian để làm được điều đó cho nên để UI có tính hữu dụng cao thì cái khoảng thời gian nói trên càng phải được rút ngắn lại.

Thế bằng cách nào mà người dùng lại hiểu được chính xác cái nào có tương tác được hay không? Chính bằng những trải nghiệm trước đây với những ký hiệu thị giác (size, shape, màu sắc, shadow,…) để lọc ra được ý nghĩa của một đối tượng trong UI. Bởi vậy việc dùng ký hiệu thị giác sát nghĩa nhất có thể vô cùng quan trọng để button dễ được nhận ra giữa hàng loạt các đối tượng khác. Chúng ta cần luôn ghi nhớ được mức độ quan trọng của ký hiệu thị giác đối với giá trị thông tin mà sản phẩm cung cấp – cũng sẽ khiến cho giao diện tăng thêm tính quen thuộc cho người dùng.

Thật không may rằng là dấu hiệu nhận biết tương tác cơ bản vẫn còn yếu trong một số giao diện và đòi hỏi nhiều thao tác hơn thông thường. Dẫn đến kết quả là mức độ hiệu quả giảm đi rõ rệt.

Nếu sự tương tác không có tính quen thuộc trong đó và người dùng cứ phải vất vả tự xác nhận xem chỗ nào bấm vào được, chỗ nào không thì thiết kế có lung linh mấy cũng chả giúp được gì bởi người dùng lúc này sẽ tự động đánh giá sản phẩm đó kém khả dụng khi chúng cứ gây khó dễ cho họ.

Với những dấu hiệu nhận biết đã kém thì sẽ càng kém hơn trên phiên bản mobile. Trong vài trường hợp, để nhận biết được một đối tượng có bấm được hay không thì người dùng trên PC có thể rê chuột lên đối tượng đó xem nó có phản ứng gì hay không còn trên mobile thì lại không có khả năng như thế. Thành ra để xác định được khả năng tương tác của một đối tượng, người dùng buộc phải chạm vào nó – ngoài ra không còn cách nào khác cả.

**Đừng tự cho rằng user cũng có thể tự hiểu được UI của bạn.**

Trong nhiều trường hợp, designer cố tính làm giảm đi dấu hiệu tương tác của button bởi họ cứ giả bộ rằng người dùng sẽ luôn tự hiểu điều đó. Nên khi thiết kế một giao diện bất kỳ, bạn nên luôn lưu tâm quy tắc sau đây:

> Khả năng tự phán đoán được dấu hiệu tương tác của bạn khác với người dùng bởi chỉ có bạn mới biết rõ chức năng nào sẽ đảm nhận vai trò gì trong thiết kế của mình.

**Sử dụng thiết kế quen thuộc**

Dưới đây là một số thiết kế button quen thuộc với hầu hết mọi người:

- Filled button with square borders
- Filled button with rounded corners
- Filled button with shadows
- Ghost button

Trong tất cả những ví dụ trên thì dạng “Filled button with shadows” là quen thuộc với hầu hết người dùng nhất do lớp shadow bên dưới tạo cảm giác ba chiều và người dùng thì lại thường có phản ứng khá tốt với các dạng hình ảnh ba chiều này.

![]({{site.baseurl}}/https://miro.medium.com/max/224/1*UZTsPglcLschWx2IUMt7dg.png)

**Đừng quên những khoảng trắng**

Không chỉ những yếu tố về thị giác trong button mới quan trọng. Mà ngay cả khoảng không gian xung quanh button cũng đóng vai trò giúp cho việc nhận ra được button dễ hay khó nữa. Như cái hình ví dụ dưới đây, nhiều người sẽ dễ lầm cái ghost button đấy thành một hình chữ nhật bỗ nghĩa cho cả đoạn văn bản.

Là một người dùng, bạn hẳn sẽ cảm thấy phân vân rằng cái nút đó có bấm được không? Hay chỉ đơn giản là một hình chữ nhật nào đấy..

#### 2. Đặt button ở những vị trí quen thuộc

Không ai ép bạn phải đặt button ở đâu cả nhưng tốt hơn hết, như tiêu đề, bạn nên đặt button ở những vị trí mà người dùng có thể đoán được bởi vì như vậy thì khi cần tương tác với giao diện, họ không nhất thiết phải đi tìm chúng nữa nếu không là họ sẽ tự cho rằng button không hiện hữu và đánh giá không tốt.

**Layout cơ bản và UI chuẩn**

Button được đặt ở những vị trí được quy ước lúc nào cũng dễ dàng tìm thấy cả. Đi kèm với những dạng layout cơ bản, quen thuộc thì người dùng sẽ luôn hiểu được mục đích của từng thành phần – cho dù chúng có khó phân biệt cách mấy đi chăng nữa. Kết hợp layout dạng vừa rồi với lối thiết kế xach-sạch-đẹp, sử dụng khoảng trắng hợp lý thì tổng thể sẽ vô cùng thân thiện cho người dùng.

> Đừng lấy button ra để chơi trốn tìm với người dùng

#### 4. Gắn nhãn (Label) button đúng với chức năng của nó

Khi nhiều button bị trùng nhau hoặc label sai sẽ rất dễ dàng gây hoang mang cho người dùng và để tránh vấn đề này thì bạn nên label button sao cho thật dễ hiểu và còn phải khớp với chức năng của chúng nữa.

Một ví dụ dễ hiểu, hãy tự đặt mình vào vị trí người dùng và bạn vô tình bấm vô cái nút Delete, dưới đây là cái bảng báo lỗi hiện ra.



Cái label “OK” không diễn đạt đủ hết chức năng chính của button.

“OK” và “Cancel” còn khá lá mông lung trong tình huống này, phần lớn người dùng sẽ thường tự hỏi là “Nếu mình bấm nút Cancel thì sao nhỉ?”

Đừng bao giờ thiết kế hộp thoại (dialog box) nào mà chỉ chứng hai button OK và Cancel không thôi.

Thay vì đặt label là “OK” thì nên đặt là “Remove” sẽ rõ nghĩa hơn nhiều. Ngoài ra, nếu việc Delete là một thao tác có khả năng gây hại thì có thể tô đỏ button lên để biểu thị tốt hơn.



Label “Remove” giúp làm rõ nghĩa cho chức năng của button hơn hẳn.



Button “Disable Card” có khả năng gây hại nên được bôi đỏ. Credit



