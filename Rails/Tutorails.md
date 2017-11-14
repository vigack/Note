#### 前言

每次看rails总是重头开始看，这样做效率实在太低了，决定每看一点，就记录下重点。
笔记还是很重要的，后期建立个人知识管理中心，那笔记简直就是我的第二生命了。

#### 安装rails

首先要确认已经是否已经安装了ruby，然后直接执行gem命令即可：
```shell
ruby -v
gem install rails
rails -v
```
这里要注意，不知道从那个版本开始，rails需要依赖js运行环境了，所以nodejs也需要预装一下。

#### 创建项目Scaffold

很简单的一个命令
```shell
rails new blog
```

启动rails服务器，只要在相应目录下执行：
```shell
rails server
```

#### hello,rails

至少已经做了一年多java了，基本的WEBMVC还是会的嘛。要想让一个页面显示，至少得有controller和view。
在rails中，创建文件的行为都是自动化的：
```shell
rails generate controller Welcome index
```

这里关注下控制台输出:
```shell
create  app/controllers/welcome_controller.rb
 route  get 'welcome/index'
invoke  erb
create    app/views/welcome
create    app/views/welcome/index.html.erb
invoke  test_unit
create    test/controllers/welcome_controller_test.rb
invoke  helper
create    app/helpers/welcome_helper.rb
invoke    test_unit
invoke  assets
invoke    coffee
create      app/assets/javascripts/welcome.coffee
invoke    scss
create      app/assets/stylesheets/welcome.scss
```
rails不仅仅帮我们创建了相应文件和资源，还给我们生成了Restful的路由。

#### 设置首页

也就是把/路径的路由交付给我们刚刚创建的Welcome中。rails中路由相关的设置在config/routes.rb文件中。

```ruby
Rails.application.routes.draw do
  get 'welcome/index'   # 这个是我们在执行generate命令时自动生成的
 
  root 'welcome#index'  # 设置/路径路由，这里我不太懂welcome/index和welcome#index的区别时什么
end
```

#### 创建资源

实际上，一个完整的资源访问借口，仅仅通过controller和view是不行的，还需要model或者说是resource。

在路由中申明一个资源:
```ruby
Rails.application.routes.draw do
  get 'welcome/index'
 
  resources :articles # 申明一项资源
 
  root 'welcome#index'
end
```

这样路由会自动关联如下路由：
```shell
$ bin/rails routes
      Prefix Verb   URI Pattern                  Controller#Action
    articles GET    /articles(.:format)          articles#index
             POST   /articles(.:format)          articles#create
 new_article GET    /articles/new(.:format)      articles#new
edit_article GET    /articles/:id/edit(.:format) articles#edit
     article GET    /articles/:id(.:format)      articles#show
             PATCH  /articles/:id(.:format)      articles#update
             PUT    /articles/:id(.:format)      articles#update
             DELETE /articles/:id(.:format)      articles#destroy
        root GET    /                            welcome#index
```

主要，上述申明实际上没有生成任何controller或是MVC中其他我们关心的东西，只是申明了一系列rest接口而已。

所以，我们先创建一个controller，安装命名规范，文件名应该是Articles
```shell
rails generate controller Articles
```

这样我们就能看到rails帮我们创建了一个空文件：
```ruby
class ArticlesController < ApplicationController
end
```

接下来分别创建增删改查等action

+ new
```ruby
class ArticlesController < ApplicationController
  def new
  end
end
```