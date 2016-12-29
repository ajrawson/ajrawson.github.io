---
title: MVC’s DropDownListFor Selected Value not being set
layout: post
type: posts
tags: [MVC]
categories: [tech]
---

I came across an issue the other day with the DropDownListFor Html Helper in ASP.Net MVC 2. For some reason the selected value of the drop down list was not being set by the HTML Helper, but only seemed to happen with one instance of the overloaded functions. My issue became apparent when I was validating data in my controller and sending the populated ViewModel back to the view if the data was invalid. I wanted all the previously selected values and text to retain their previous values but for some reason the DropDownListFor’s were not working. For Example in the code snippet below is the DropDownListFor that wasn’t working for me.

[image lost]

I’m passing in the Manufacture and ManufacturerList to my View through a view model, the ManufacturerList is a prepopulated list of manufacturer names stored as string values. The Manufacturer stores the selected value from the ManufacturerList. Here is what my controller looks like

```C#
Function Search(ByVal itemSrch As ItemSearchViewModel) As ActionResult

     If Not ModelState.IsValid Then Return View(itemSrch)
     
     'some mapping code and model stuff
     'return a new view
     
End Function
```

And this is what my ViewModel looks like:

```C#
Public Property Manufacturer As String

Public Property ManufacturerList() As List(Of String)
```

If I use an overloaded method by setting the data text and data value fields everything seems to work fine, for example:

[image lost]

The only problem is that in my case the ManufactuerList is not a Manufacturer object but a list of String values containing Manufacturer names. I am there for stuck using the first DropDownListFor function that I showed in this post. I did a bunch of testing and debugging to see if I could figure out if it was something that I was doing wrong or something in the DropDownListFor helper I was using. I came to the conclusion that it was the DropDownListFor helper code, when I tested the value of the Manufacturer property upon page refresh the property was populated. After a bunch of research on the issue and looking for a solution to the problem, and I couldn’t find a good one, I figured I’d dig in and see if I could come up with something.

My first thought was trying to move the populating of the SelectList outside of the View. So I moved the SelectList to my ViewModel and changed the ManufacturerList property to be a SelectList. I didn’t even get as far as testing to see if this would work before I decided this was a bad idea. All I could think of was, “What if I have to use this ViewModel in some other application and its not an MVC app”. If I had left the SelectList in the ViewModel I’d be stuck with using that or creating a new ManufacturerList in the ViewModel that was a List(Of String) and that seemed redundant. So I trashed the idea of putting the SelectList in the ViewModel. The next, and what ended up being the final, idea I had was to use jQuery to set the selected value on the drop down list once the page had loaded. I knew for certain from my earlier testing that the Model was holding the Manufacturer value, I just needed to get it to be the selected value of the drop down list. So with the help of jQuery’s ready handler I set the value of the drop down list to be that of the Manufacturer property contained in the Model. Here is the jQuery code I used:

```javascript
$(document).ready(function() {

    $("#Manufacturer").val("");

});
```

And ta-da my problem was fixed, the jQuery code was now setting the selected value of the drop down list to that of my Manufacturer value. For the sake of those who don’t want to use jQuery also tested this just using good old javascript and that worked as well. You just need to setup some javascript to run when the page loads and set the drop down list value using the code below:

```javascript
document.getElementByID("Manufacturer").Items.FindByValue("").Selected = true;
```

Hopefully this helps someone, and also if you have any other advice or see any problems with my solution please leave a comment. I’d appreciate it. Thanks.