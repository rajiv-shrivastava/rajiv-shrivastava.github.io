---
layout: post
title:  "How to integrate a hindi textarea in the website which convert english typed words to hindi"

date:   2016-03-10 22:21:53 +0530

categories: jekyll update
---

<h2> Hi developers </h2>

 We often get some clients who want to us to add a form field in our website with hindi typing fuctionality.

 But there may be a possibilty that the user also know which engish aplpabet key is there for a particular hindi word.

A sample screenshot

<br> `right click on image and open it new tab to view actual image`
<img src="/images/hindi-textbox.png" style="height:350px;width:700" />

 Here i am using a simple api provided by <a hre="http://www.hinkhoj.com/"> Hindkhoj </a>  which will dynamically add a text field to your app.

<br>
Add this stylesheet to the page where you have to your html file.
      {% highlight ruby %}

<link rel="stylesheet" type="text/css"href="http://www.hinkhoj.com/common/css/keyboard.css" />

{% endhighlight %}

<br>

Add this  javascript file in head section.

      {% highlight ruby %}
 <script src="http://www.hinkhoj.com/common/js/keyboard.js"></script>
{% endhighlight %}

<br><br>  
In you html body add this code.
      {% highlight ruby %}

<script language="javascript">

CreateCustomHindiTextArea("id 1","अपऩा मेसेज यहा लिखे",80,5,true);
</script>

{% endhighlight %}

That's all run the code a custom textarea is there with easy hindi typing.

Type in some words inside textarea it will convert it to hindi.`Eg. If you type a == अ` 

`Similarly if you type p == प and with some practice you are able to write whole sentences in hindi`

