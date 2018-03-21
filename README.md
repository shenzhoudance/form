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
