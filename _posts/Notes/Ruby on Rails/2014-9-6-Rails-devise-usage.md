---
layout: post
title: devise 的使用
date: 2014-9-6 11:51
categories: 笔记
tags: [Ruby_on_Rails,devise]
---

测试环境如下:

Ruby: 2.0    
Rails: 4.1     
OS: Ubuntu 10.04.4    

Authentication 神器: devise 基本用法如下

1.在 Gemfile 中添加:

`gem 'devise'`

2.bundle install
3.rails g devise:install 
4.使用 devise 生成Model,这里假设 Model 是 User,运行:
`rails generate devise User`

5.运行`rake db:migrate`

6.在`config/environments/development.rb`和`config/environments/production.rb` 中加入发送邮件时的地址

```ruby
config.action_mailer.default_url_options = { :host => 'localhost:3000' }
```

7.测试 flash 信息的显示:

在`app/views/layouts/application.html.erb`添加

```ruby
<% flash.each do |key,value| %>
    <div class="alert alert-<%= key %> alert-dismissible fade in">
      <button type="button" class="close" data-dismiss="alert">
	       <span aria-hidden="true">&times;</span>
	       <span class="sr-only">Close</span>
      </button>
           <%= value %>
    </div>
<% end %>
```

8.在你的路由里面设置`root`,即`root to: "home#index"`.

9.若想使用邮件认证功能,需进行以下操作:    
	a.在`app/models/user.rb`的 devise 加上`:confirmable`.    
	b.在`db/migrate/XXXXXX_devise_create_user.rb`中,去掉与`confirmable`相关的注释.    

10.运行`rails generate devise:views`自动生成登录,注册表单等视图.

11.rake db:migrate

12.在需要验证登录的 controller 中加入`before_filter :authenticate_user!`.

13.加入邮箱认证功能    
配置`config/environment/development.rb`和`config/environment/production.rb`,让这一功能能在开发和生产环境下使用,以 Gmail 邮箱为例:

`config/initializer/devise.rb`中加入

```ruby
config.mailer_sender = "XXX@gmail.com" #设置网站邮箱,我暂时使用网易邮箱测试
```
```ruby
config.action_mailer.default_url_options = { host: 'localhost:3000' }
  config.action_mailer.delivery_method = :smtp
  config.action_mailer.smtp_settings = {
    address:              'smtp.gmail.com',
    port:                 587,
    domain:               'gmail.com',
    user_name:            'XXX@gmail.com',			 //填入你的邮箱地址,用来发送邮件
    password:             'YOUR_EMAIL_PASSWORD',     //你的邮箱密码
    authentication:       'plain',
    enable_starttls_auto: true     
  }
```

到这里为止, devise 一些基本的功能已经可以正常使用了.

devise 自带的一些方法:

```ruby
	new_user_session_path 		#登录路径
	new_user_registration_path 	#注册路径
	destroy_user_session_path	#账户退出路径
	edit_registration_path		#修改密码
	user_signed_in? 			#用户是否登录
	current_user			    #当前用户
```

关于 devise 在 bootstrap 中一些样式无法显示出来的问题,网上给出了两个[解决方法](https://github.com/plataformatec/devise/wiki/How-To:-Integrate-I18n-Flash-Messages-with-Devise-and-Bootstrap):        
1.重写 devise 自带的 helper 方法

新建 `app/helpers/devise_helper.rb`, 写入

```ruby
module DeviseHelper
  def devise_error_messages!
    return '' if resource.errors.empty?

    messages = resource.errors.full_messages.map { |msg| content_tag(:li, msg) }.join
    html = <<-HTML
    <div class="alert alert-error alert-block"> <button type="button"
    class="close" data-dismiss="alert">x</button>
      #{messages}
    </div>
    HTML

    html.html_safe
  end
end
```
在你想要显示错误信息的地方嵌入`<%= devise_error_messages! %>`.

2.直接加入一段 css 代码,简单粗暴,我采用的就是该方法,如下.

```sass
/*flash*/
.alert-error {
    background-color: #f2dede;
    border-color: #eed3d7;
    color: #b94a48;
    text-align: left;
 }

.alert-alert {
    background-color: #f2dede;
    border-color: #eed3d7;
    color: #b94a48;
    text-align: left;
 }

.alert-success {
    background-color: #dff0d8;
    border-color: #d6e9c6;
    color: #468847;
    text-align: left;
 }

.alert-notice {
    background-color: #dff0d8;
    border-color: #d6e9c6;
    color: #468847;
    text-align: left;
 }
```



