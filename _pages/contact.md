---
layout: page
title: Liên hệ
permalink: /contact
comments: false
---

<form action="https://formspree.io/{{site.email}}" method="POST">    
<p class="mb-4">Vui lòng gửi lời nhăn tới {{site.name}}. Chúng mình sẽ trả lời bạn trong thời gian sớm nhất!</p>
<div class="form-group row">
<div class="col-md-6">
<input class="form-control" type="text" name="name" placeholder="Tên của bạn*" required>
</div>
<div class="col-md-6">
<input class="form-control" type="email" name="_replyto" placeholder="Địa chỉ Email*" required>
</div>
</div>
<textarea rows="8" class="form-control mb-3" name="message" placeholder="Lời nhắn*" required></textarea>    
<input class="btn btn-dark" type="submit" value="Gửi đi">
</form>
