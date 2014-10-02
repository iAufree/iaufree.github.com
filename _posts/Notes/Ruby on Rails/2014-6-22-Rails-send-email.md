---
layout: post
title: 使用Rails发送邮件
categories: 笔记
tags: [Ruby_on_Rails,Notes]
---

新建测试项目:

`Rails new mailit`    
`rails g scaffold user name:string email:string`    
`rake db:migrate`    

假设使用Gmail来发送邮件    
在`config/initializers/setup_mail.rb`中加入

```ruby
ActionMailer::Base.smtp_settings = {
  address:              'smtp.gmail.com',
  port:                 587,
  domain:               'gmail.com',
  user_name:            'your_email',
  password:             'your_email_password',
  authentication:       'plain',
  enable_starttls_auto: true
}
```

创建mailer

`rails g mailer user_mailer`

在`user_mailer.rb`中写入

```ruby
class UserMailer < ActionMailer::Base
	default from: "your_email"
	def welcome(user)
		@user = user
		mail(to: user.email, subject: "Welcome")
	end
end
```

`users_controller.rb`中的`create`添加`UserMailer.welcome(@user).deliver`,其中的`deliver`可以放在其它地方,表示执行到某操作时,触发发送邮件的功能.

在`views/user_mailer`中新建一个`welcome.text.rrb`或`welcome.html.erb`文件,写入想要发送的内容.

新建用户之后,邮件将会发送到新用户注册时使用的那个邮箱上.

若在运行的命令行能见到邮件已成功发送的信息,而邮件却始终没被发送,解决方法是:

  在`development.rb`中设置`config.action_mailer.raise_delivery_errors`的值为`true`.


安装gem `mail`后,可执行更多操作.

例如,将用户注册的情况反馈回来.

先把前面`subject`的信息改为"成功注册".    
创建`lib/development_mail_interceptor.rb`,写入:

```ruby
class DevelopmentMailInterceptor
  def self.delivering_email(message)
    message.subject = "#{message.to} #{message.subject}"
    message.to = "your_email"
  end
end
```

`config/initializers/setup_mail.rb` 添加:

```ruby
require 'development_mail_interceptor' 
ActionMailer::Base.default_url_options[:host] = "localhost:3000"
Mail.register_interceptor(DevelopmentMailInterceptor) if Rails.env.development?
```

See also: 

[Action Mailer in Rails 3](http://railscasts.com/episodes/206-action-mailer-in-rails-3)   
[ActionMailer](http://api.rubyonrails.org/classes/ActionMailer/Base.html)



