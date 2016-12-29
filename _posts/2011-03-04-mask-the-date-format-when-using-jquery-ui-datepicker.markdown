---
title: Mask the date format when using jQuery UI datepicker
layout: post
type: posts
tags: [jQuery, javascript]
categories: [tech]
---

In this post I want to cover how we can mask the value of an input thats using the jQuery UI datepicker, so if a user enters a date into the input field it is formatted correctly.  Also by supplying a mask to the input field it will give a user an idea of the date format required if they enter the date manually, below is an example if what we will create (using MM/dd/yyyy).

![Figure 1](/assets/img/20110304/figure1.png)

The two things we’ll need to add to our code to get this working, is a reference to the [jQuery UI](http://jqueryui.com/) plugin and the [jQuery masked input](http://archive.plugins.jquery.com/project/maskedinput) plugin.  Also since we’re using jQuery, if your not already referencing the jQuery library you’ll need to reference that as well. 

![Figure 2](/assets/img/20110304/figure2.png)

Now that we have the correct jQuery libraries and plugins registered in the page we are ready to setup our html input and javascript code.   First add an input text element to the html code:

![Figure 3](/assets/img/20110304/figure3.png)

Next we’ll want to add the javascript code to hook up the datepicker and input masking functionality from the jQuery plugins to the input element we’ve just added to our page.  We’ll do this in the jQuery ready function.  In the code that follows you’ll first see that the dateValue input is wired up to use the jQuery mask plugin and that we want to use the date format of MM/dd/yyyy.  To declare that we want to use the format of MM/dd/yyyy, we say that we want only digits in the textbox and it should be in the format of “99/99/9999”.  This works great, the only problem is that it allows users to put any digit in the text box, but it must still follow the date format we are expecting (MM/dd/yyyy).  To fix this issue we’ll add some validation later.  The other part of the javascript you’ll see being wired up to the dateValue input in the jQuery ready function is the jQuery UI datepicker.  For the settings of the datepicker we need to also set the format to follow the masking format we set earlier.  If you want to learn more about the dateformat options on the jquery UI datepicker check out this link http://jqueryui.com/demos/datepicker/#option-dateFormat.

![Figure 4](/assets/img/20110304/figure4.png)

(Disclaimer: Just to make this easy to read, I broke up the jQuery selectors, but you could chain them together.)

We now have the jQuery datepicker and masking wired up to our dateValue input element.  The only other thing we might want to add to complete this feature is to add some validation to the input. The validtion will be important for users who manually enter a date value rather then using the datepicker.  For this we’ll just use jQuery validation plugin and a little regular expression.  The code that follows will also reside in the jQuery ready function and wires up the jQuery validation plugin to the dateValue input box.  The validtion will be verified before the form is posted back to the server, thus saving you hit on your server, and will notify the user if there is something wrong with their date value.

![Figure 5](/assets/img/20110304/figure5.png)

Hopefully this post helps you out if you are looking to add some masking to your jQuery UI datepickers.  If you have questions or comments please leave me a note in the comment section below.