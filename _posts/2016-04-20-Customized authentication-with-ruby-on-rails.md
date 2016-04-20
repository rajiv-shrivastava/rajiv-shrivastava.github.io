---
layout: post
title:  "Customized authentication with Ruby on Rails"

date:   2016-03-10 22:21:53 +0530

categories: jekyll update
---

<h2> Hi developers </h2>



<br>

<h3>  Why read this??   </h3>
<hr>
<br>
<blockquote>
 <p>
Authentication is the basic need for any website
Ruby On Rails has so many flexible and cool gems and librabries for performing authentication but many times we want a customized authentication. The need arises when we want to add an admin role with the gem we are using for our app.
For eg. We are using <a href="https://github.com/plataformatec/devise" target="blank"> Devise </a> as authentication gem and we also want to add an admin role in it without any signup/registration form/logic. For that purpose this tutorial is helpful 
</p>
</blockquote>


<h2> Let's Start </h2>

<h3 > STEP 1</h3>

First open your terminal and make a new app 

  {% highlight ruby %}
rails new custom_login
{% endhighlight %}

Type the command <kbd> cd custom_login </kbd>

<hr> <br>
<h3> STEP 2</h3>


After this open up your gemfile `custom_login/Gemfile` and add this line to it.This is 
the password encryptor.
  {% highlight ruby %}
gem 'bcrypt' ,'3.1.7'
{% endhighlight %}


Now run the bundler
{% highlight ruby %}
bundle install
{% endhighlight %}

<hr> <br>
<h3> STEP 3</h3>


First we make a model in which we put user/admin details.I am naming this model as Admin. You can name it whatever you want.

Now run the bundler
{% highlight ruby %}
rails g model Admin name:string email:string password_digest:string
{% endhighlight %}
 
 If you want to name the model User replace `Admin` with  `User`
  

Migrate the database

{% highlight html%} rake db:migrate {% endhighlight %}


<hr> <br>
<h3> STEP 4 </h3>

We have earlier use gem 'bycrypt' it is used for encrypting the password.Now we have to specify to encrypt Admin password. For this add this line to your model `custom_login/app/models/admin.rb`


```html
class Admin < ActiveRecord::Base
    has_secure_password
end
```

Adding ``has_secure_password`` will encrypt the password before saving.


<hr> <br>

<h3> STEP 5 </h3>

Make our `admins` Controller with `index` and `welcome` actions

{% highlight html %} rails generate controller admins index welcome {% endhighlight %} 


<hr> <br>
<h3> STEP 6 </h3>

Open up your database seeds file `custom_login/db/seeds.rb` <br>
Add this code to the seeds file.

{% highlight html %}Hover.create(name: 'demo',email: 'demo@gmail.com', password_digest: 'mypassword'){% endhighlight %} 

Replace `demo@gmail.com` and `mypassword` with the credentials you want

Then run {% highlight html %} rake db:seed {% endhighlight %} 

'rake db:seed' will add this entry to database


<hr> <br>
<h3> STEP 7 </h3>

<h4> Encrypt the password </h4>

Open a new terminal go to app location `path_to_app/custom_login`
and open rails console in it usng command <kbd>rails console</kbd>

Then run 
{% highlight html %} Admin.first{% endhighlight %} 

We have use Admin.first beacuse we have only one entry. This commnad will display the details.It looks like this

<p class="well"> 2.3.0 :001 > Admin.first Admin Load (0.2ms)  SELECT  "admins".* FROM "admins"  ORDER BY "admins"."id" ASC LIMIT 1=> #<Admin id: 1, name: "demo", email: "demo@gmail.com", password_digest: "password", created_at: "2016-04-19 17:45:08", updated_at: "2016-04-19 19:12:35"> 
2.3.0 :002 > </p>

In the console run this

{% highlight html %} admin = Admin.first{% endhighlight %}

This above command make instance of first Admin.Now run this in rails console

{% highlight html %}admin.password = "password"{% endhighlight %} 

<blockquote>You can change the password here or keep it same i am using 'password' here.</blockquote>

Now  save the updated admin
{% highlight html %} admin.save{% endhighlight %}


Now again run 

{% highlight ruby %} admin = Admin.first{% endhighlight %}


<p class="well">
2.3.0 :002 > Admin.first
  Admin Load (0.4ms)  SELECT  "admins".* FROM "admins"  ORDER BY "admins"."id" ASC LIMIT 1=> #<Admin id: 1, name: "demo", email: "demo@gmail.com", password_digest: "$2a$10$LL4PPjnTrHcpxuQmYfCkjeEq3fsS88Xvxt8oGkPkbU7...", created_at: "2016-04-19 17:45:08", updated_at: "2016-04-19 19:12:35"> 
2.3.0 :003 > 
</p>

 If you look at `password_digest:` it is now encrypted. 

 Type <kbd>exit</kbd> to quit the console. 



<hr> <br>
<h3> STEP 8 </h3>


<h4>Specify the routes for application </h4> Open your `custom_login/config/routes.rb`.Add the code <br>
{% highlight ruby %}

Rails.application.routes.draw do
  
  get 'sessions/new'
  get 'admins/index'
  get 'admins/welcome'
  get 'login', to: 'sessions#new'
  post 'login', to: 'sessions#create'
  get 'logout', to: 'sessions#destroy'
  root 'admins#index'
end
{% endhighlight %}



<hr> <br>

<h3> STEP 9 </h3>

Now we make a `sessions` controller to manage our seesions.In terminal run this commnad

{% highlight ruby %} rails generate controller sessions{% endhighlight %}

Add this code to your `custom_login/app/controllers/sessions_controller.rb`
{% highlight ruby %} 
class SessionsController < ApplicationController
  
	def new
	end

	def create
	     @admin = Admin.first
	    if @admin && @admin.authenticate(params[:password])
	  		session[:admin_id] = @admin.id
	  		flash.notice = "welcome admin"
	        redirect_to admins_welcome_path
	  	else
	  	    
	        flash.notice = "incorrect id passswrd"
	        render 'new'
	  	end	
	end

	def destroy
	  	session[:admin_id] = nil
	  	flash.notice = "bye"
	  	redirect_to root_path
	end

end
{% endhighlight %}

<hr> <br>

<h3> STEP 10 - Final Step </h3>

<h3> Views for our app </h3>

<h4> (I) Default View </h4>

Add this code to your `custom_login/app/views/layouts/application.html.erb` 

{% highlight html %} 
<!DOCTYPE html>
<html>
<head>
  <title>CustomLogin</title>
  <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true %>
  <%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
  <%= csrf_meta_tags %>
</head>
<body>

<%= link_to "Log in" , login_path%>

<%= link_to "Log out" , logout_path %>

<h4>
<% flash.each do |type ,message | %>

      <%= message %>

      <% end %>
</h4>

<%= yield %>

</body>
</html>

{% endhighlight %}


<h4> (II) Login View </h4>
Now the login form. Add these lines in your `custom_login/app/views/sessions/new.html.erb`


{% highlight html %} 
<h1>Login To App</h1>

<%= form_tag '/login' do %>
	email <%= text_field_tag :email %>
	Password <%= password_field_tag :password %>
<%= submit_tag  "submit" %>
<% end %> 
{% endhighlight %}

<h4> (III)Welcome page after successfull login </h4> 

Add these lines in your `custom_login/app/views/admins/welocome.html.erb`

{% highlight html %} 
<h1>Welcome Admin</h1>
{% endhighlight %}

<hr> <br>

<h4> Now you have a customized authentication application.Lets check </h4>

Visit <a href="http://localhost:3000/" target="blank" class="btn btn-primary" style="color: #fff"> localhost:3000 </a> 

