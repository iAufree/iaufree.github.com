---
layout: post
title: Rails 中使用 pjax 和 nprogress
date: 2014-9-7 14:47
categories: 笔记
tags: [pjax,nprogress]
---

[pjax](https://github.com/defunkt/jquery-pjax) (pushState + ajax) + [nprogress](https://github.com/caarlos0/nprogress-rails) (Show Progressbar).

双剑合璧,天下无敌.

安装,编辑 Gemfile,加入:

```ruby
gem 'pjax_rails'
gem 'nprogress-rails'
```

运行`bundle install`后,先配置 pjax,打开`layouts/application.html.erb`,修改代码

```ruby
<div data-pjax-container>
  <%= yield %>
</div>
```
若想添加标题,在页面中使用`<% provide(:title, 'TITLE_HERE')%>`给标题赋值    
打开`application.html.erb`,使用`<title><%= yield(:title) %></title>`嵌入标题    
或者使用另一个gem:`rack_pjax`,具体用法[点击这里](http://railscasts.com/episodes/294-playing-with-pjax)

配置 `nprogress`

`application.js`

```javascript
//= require nprogress
//= require nprogress-turbolinks
```

如果该文件里面是使用`pajx`而不是`turbolinks`的话,用`nprogress-ajax`代替.

`application.css.scss`

```sass
*= require nprogress
*= require nprogress-bootstrap
```

到这里各个 gem 都可以正常工作,点击[pjax_rails](https://github.com/rails/pjax_rails) 和 [nprogress-rails](https://github.com/caarlos0/nprogress-rails) 的 Github 地址了解更多配置.

感谢大神们制作了这么多好用的 gem,简单易用,赞一个!