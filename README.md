# 才华横溢 form 案例教学 10
```
cd workspace
rails new form
cd form
git init
git status
git add .
git commit -m "initial commit"
git remote add origin https://github.com/shenzhoudance/form.git
git push -u origin master
rails server
```
![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fpk8aumm6kj31ik0pcadx.jpg)
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fpkhoecbkgj31i41141kx.jpg)

```
git checkout -b posts
rails g model post title:string content:text
rake db:migrate
rails g controller posts
rails server
```
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fpkhsfstnkj31bk0gu0w4.jpg)
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fpkhzs2bjzj31a00ci40f.jpg)
```
config/routes.rb
---
Rails.application.routes.draw do
 resources :posts
 root 'posts#index'
end
---
app/controllers/posts_controller.rb
---
class PostsController < ApplicationController
 def index
 end
end
---
app/views/posts/index.html.erb
---
<h1>欢迎来到才华横溢的世界</h1>
---
```
```
rake routes
rails server
```
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fpki7x4wn1j30vi09mdhv.jpg)
![image](https://ws1.sinaimg.cn/large/006tKfTcgy1fpki3pn7mdj30qe08uwf7.jpg)
```
git status
git add .
git commit -m "add post model & controller & index"
git push origin posts
```
![image](https://ws1.sinaimg.cn/large/006tKfTcgy1fpkif8cwmjj31kw0ixq6y.jpg)
![image](https://ws1.sinaimg.cn/large/006tKfTcgy1fpkien0818j317s0n2n08.jpg)
![image](https://ws4.sinaimg.cn/large/006tKfTcgy1fpkie970v4j31920p4aey.jpg)
![image](https://ws4.sinaimg.cn/large/006tKfTcgy1fpkidszzokj31ak0pkq8n.jpg)
```
git checkout -b gem
https://rubygems.org/
gem 'haml', '~> 5.0', '>= 5.0.4'
gem 'bootstrap-sass', '~> 3.3', '>= 3.3.7'
gem 'simple_form', '~> 3.5', '>= 3.5.1'
gem 'devise', '~> 4.4', '>= 4.4.3'
bundle install
rails generate simple_form:install --bootstrap
```
```
app/views/posts/index.html.haml
---
%h1 欢迎来到才华横溢的世界
app/assets/stylesheets/application.css.scss
---
@import “ bootstrap-sprockets ”;
@import “ bootstrap ”;
---
app/assets/javascripts/application.js
---
//= require bootstrap-sprockets
//= require turbolinks
```
```
app/controllers/posts_controller.rb
---
class PostsController < ApplicationController
  before_action :find_post, only: [:show, :edit, :update, :destroy]

	def index
		@posts = Post.all.order("created_at DESC")
	end

	def show
	end

	def new
		@post = Post.new
	end

	def create
		@post = Post.new(post_params)

		if @post.save
			redirect_to @post
		else
			render 'new'
		end
	end

	def edit
	end

	def update
		if @post.update(post_params)
			redirect_to @post
		else
			render 'edit'
		end
	end

	def destroy
		@post.destroy
		redirect_to root_path
	end

	private

	def find_post
		@post = Post.find(params[:id])
	end

	def post_params
		params.require(:post).permit(:title, :content)
	end
end

```
```
app/views/posts/_form.html.haml
---
= simple_form_for @post do |f|
	= f.input :title
	= f.input :content
	= f.submit
---
app/views/posts/new.html.haml
---
%h1 New Post

= render 'form'
---
app/views/posts/edit.html.haml
---
%h1 Edit Post

= render 'form'

= link_to "Cancel", post_path
---
app/views/posts/show.html.haml
---

%h1= @post.title
%p= @post.content

= link_to "Edit", edit_post_path(@post), class: "button"
= link_to "Delete", post_path(@post), method: :delete, data: { confirm: "Are you sure you want to do this?" }, class: "button"
---
app/views/posts/index.html.haml
---
- @posts.each do |post|
		.post
		%h2.title= link_to post.title, post
		%p.date
				Published at
				= time_ago_in_words(post.created_at)

= link_to "New Post", new_post_path, class: "btn btn-default btn-lg"

---
```
![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fpkk0kxqusj30y40fidhy.jpg)
![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fpkk0ay5b2j31kw0ewtaj.jpg)
![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fpkk01jp5jj30q00a6q45.jpg)
![image](https://ws1.sinaimg.cn/large/006tKfTcgy1fpkl8aldavj313y0icmzg.jpg)

```
git checkout -b devise
rails generate devise:install
rails g devise:views
rails generate devise user
rake db:migrate
---
git status
git commit -m "add devise user"
git push origin devise
---
rails server
http://localhost:3000/users/sign_up
```
![image](https://ws4.sinaimg.cn/large/006tKfTcgy1fpklcxgynsj311e0z0aif.jpg)
![image](https://ws4.sinaimg.cn/large/006tKfTcgy1fpklbc044xj31kw0jcage.jpg)
![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fpklnfb3q3j31ew0qotar.jpg)

```
app/controllers/posts_controller.rb
---
class PostsController < ApplicationController
  before_action :find_post, only: [:show, :edit, :update, :destroy]
  before_action :authenticate_user!, except: [:index, :show]
	def index
		@posts = Post.all.order("created_at DESC")
	end

	def show
	end

	def new
		@post = current_user.posts.build
	end

	def create
		@post = current_user.posts.build(post_params)
  ---
```
```
app/models/post.rb
---
class Post < ApplicationRecord
  belongs_to :user
end
---
app/models/user.rb
---
class User < ApplicationRecord
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable
   has_many :posts
end

```
![image](https://ws4.sinaimg.cn/large/006tKfTcgy1fpkmg6uuhej31kw0i777a.jpg)

```
rails g migration add_user_id_to_posts user_id:integer
rake db:migrate
```
![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fpkmjn1tl6j31kw0hd40g.jpg)

```
rails c
2.3.1 :001 > @post = Post.first
2.3.1 :002 > @post = Post.last
2.3.1 :003 > @post.user_id = 1
2.3.1 :004 > @post.save
2.3.1 :005 > @post = Post.find(3)
2.3.1 :006 > @post.user_id = 1
2.3.1 :007 > @post.save
2.3.1 :007 > exit
---
```
![image](https://ws1.sinaimg.cn/large/006tKfTcgy1fpkmviha8ej31ji0q27ch.jpg)
![image](https://ws4.sinaimg.cn/large/006tKfTcgy1fpkmvqe3w1j30xo0dmabi.jpg)
![image](https://ws4.sinaimg.cn/large/006tKfTcgy1fpkmw4eco6j317w0mkjum.jpg)
![image](https://ws1.sinaimg.cn/large/006tKfTcgy1fpkmwj8wt5j30yq0c0t9s.jpg)

```
git status
git add .
git commit -m "add user find post"
git push origin devise
```
![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fpkmzp4jxvj31gi0qadm7.jpg)
```
git checkout -b nav-style
app/assets/stylesheets/application.css.scss
---
@import "bootstrap-sprockets";
@import "bootstrap";

body {
 font-family: 'Lato', sans-serif;
 background: #EDEFF0;
}

.wrapper {
 width: 60%;
 max-width: 1140px;
 margin: 0 auto;
}

.clearfix:before, .clearfix:after {
 content: " ";
 display: table;
}

.clearfix:after {
 clear: both;
}

.main_header {
 width: 100%;
 margin: 0 auto;
 background: white;
 #logo {
   float: left;
   a {
     line-height: 60px;
     text-transform: uppercase;
     font-weight: 700;
     letter-spacing: -1px;
     font-size: 1.8rem;
     text-decoration: none;
     color:#397CAC;
   }
 }
 #buttons {
   float: right;
   a {
     line-height: 60px;
     background: #397CAC;
     padding: .5em 1em;
     border-radius: 0.2em;
     color: white;
     text-decoration: none;
     font-weight: 100;
   }
 }
}

#posts {
 background: white;
 padding: 2em 5%;
 border-radius: .5em;
 .post {
   margin: 1em 0;
   padding: 1em 0;
   border-bottom: 1px solid #D1d1d1;
   .title {
     margin: 0;
     a {
       color: #397CAC;
       text-decoration: none;
       font-weight: 100;
       font-size: 1.25rem;
     }
   }
   .date {
     margin-top: .25rem;
     font-size: 0.9rem;
     color: #B2BAC2;
   }
 }
}

.button {
 color: #397CAC;
 border: 1px solid #397CAC;
 padding: .5em 1em;
 border-radius: 0.2em;
 text-decoration: none;
 margin-right: 2%;
}

#post_content {
 background: white;
 padding: 2em 5%;
 border-radius: .5em;
 h1 {
   font-weight: 100;
   font-size: 2em;
   color: #397CAC;
   margin-top: 0;
 }
 p {
   color: #B2BAC2;
   font-size: 0.9rem;
   font-weight: 100;
   line-height: 1.5;
 }
 }
---
app/views/layouts/application.html.haml
---
!!!
%html
%head
	%title XiaoWei Forum
	= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true
	%link{ rel: "stylesheet", href: "http://fonts.googleapis.com/css?family=Lato:300,400,700", type: "text/css"}
	%link{:rel => "stylesheet", :href => "http://cdnjs.cloudflare.com/ajax/libs/normalize/3.0.1/normalize.min.css"}/
	= javascript_include_tag 'application', 'data-turbolinks-track' => true
	= csrf_meta_tags
%body
	%header.main_header.clearfix
		.wrapper
			#logo
				%p= link_to "XiaoWei Forum", root_path
			#buttons
				- if user_signed_in?
					= link_to "New Post", new_post_path
				- else
					= link_to "Sign Up", new_user_registration_path
					= link_to "Sign In", new_user_session_path
	.wrapper
		%p.notice= notice
		%p.alert= alert
	.wrapper
		= yield
---
```
![image](https://ws3.sinaimg.cn/large/006tKfTcly1fplceccpcej31dm0jw415.jpg)
