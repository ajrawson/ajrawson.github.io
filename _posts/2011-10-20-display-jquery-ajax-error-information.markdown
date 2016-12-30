---
title: Display jQuery Ajax Error Information
layout: post
type: posts
tags: [jQuery, Javascript]
categories: [tech]
---

Was helping someone with an ajax issue the other day and to help diagnose what the error was I wrote a little jQuery code to display error information at the bottom of the page.  I’m sure there are about a thousand other plugins and posts out there for displaying error information about ajax requests, but thought I’d share mine anyways in hopes that it would help someone out.

```javascript
<script type="text/javascript">
    $('body').ajaxError(function (e, jqxhr, settings, exception) {
        $('div#ajaxLog').empty();
        $('div#ajaxLog').remove();
        $(this).append('<div ID="ajaxLog" style="background-color:Red;"></div>')
        var log = $('div#ajaxLog');
        $(log).append('Response Text: ' + jqxhr.responseText + '<br />');
        $(log).append('Status: ' + jqxhr.status + '<br />');
        $(log).append('Status Text: ' + jqxhr.statusText + '<br /><hr>');
        $(log).append('Data: ' + settings.data + '<br />');
        $(log).append('Data Types: ' + settings.dataTypes + '<br />');
        $(log).append('Type: ' + settings.type + '<br />');
        $(log).append('Url: ' + settings.url + '<br /><hr>');
        $(log).append('Exception: ' + exception + '<br />');
	});    
</script>
```

You’ll end up with something looking like this:

![Error Display Image](/assets/img/20111020/figure1.jpg)

Hopefully this helps you out if our run into some issues with your ajax requests.