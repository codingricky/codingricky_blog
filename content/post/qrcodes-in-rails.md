+++
author = "Ricky Yim"
categories = ["rails", "ruby", "rqrcode", "rqrcode_png", "QRcode"]
date = 2014-03-28T11:08:37Z
description = ""
draft = false
slug = "qrcodes-in-rails"
tags = ["rails", "ruby", "rqrcode", "rqrcode_png", "QRcode"]
title = "QR Codes in Rails"
aliases = [
    "/qrcodes-in-rails/"
]
+++

[QR codes](http://en.wikipedia.org/wiki/QR_code) are becoming more and more commonplace nowadays, as they represent an extremely quick and convenient way to transmit any long piece of string like a URL. Recently during my [company's](http://www.dius.com.au) second Hack Day, my team had to go about dynamically generating QR codes that represented URLs of a user's profile page. As our application was written in Rails, it turned out to be extremely easy to incorporate this functionality. The rest of this article is devoted to showing the steps necessary to generate QR codes in a Rails 4 application.

## Using rqrcode_png

The first thing to do is add the [rqrcode_png](https://github.com/DCarper/rqrcode_png) dependency to your application. So edit the Gemfile and add the following line.

```txt
gem 'rqrcode_png'
```

Now run bundle install

If this all succeeds, we can begin using the library. So let's just take a controller and in its index method generate a QR Code. At the start of the controller, require the library.

```ruby
require 'rqrcode_png'
```

We'll just use the url to this blog as the input and generate a QR code based on that. The *to_data_url* method converts the image to a base64 encoded string that can be interpreted by Rail's *image_tag* helper method. 

```ruby
  def index
  	  	@qr = RQRCode::QRCode.new("http://codingricky.com").to_img.resize(200, 200).to_data_url
  end
```

In the corresponding view, it will only be a matter of passing the *@qr* variable to the image tag.

```ruby
<%= image_tag @qr %>
```

If everything was done correctly, a QR code like the following should be rendered.

![](/images/2014/Mar/qrcode.png)

Scanning this should take you to the front page of this blog.

##Summary

The sample code for this article can be found [here](https://github.com/codingricky/rails-qrcode). For more information see the following links below:

* https://github.com/whomwah/rqrcode
* https://github.com/DCarper/rqrcode_png
* http://en.wikipedia.org/wiki/QR_Code
* http://www.qrcode.com/en/


