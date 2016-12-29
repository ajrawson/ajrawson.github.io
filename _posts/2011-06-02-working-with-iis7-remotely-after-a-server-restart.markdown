---
title: Working with IIS 7 Remotely After a Server Restart
layout: post
type: posts
tags: [IIS]
categories: [tech]
---

Since this has bit me in the butt a couple times, I wanted to post a little reminder on a configuration you shouldn’t forget about when setting up your IIS to be managed remotely. 

If your working with IIS7 and you’ve configured IIS on your webserver to use Management Service don’t forget to go into the Services section of your admin tools and set the Web Management Service to start Automatically.  The Web Management Service is defaulted to start Manual, which is fine unless you ever restart your web server.  Since the service is set to Manual the Web Manangment Service won’t auto start after a restart and you’ll have to log into your web server and start yout Management Service manuallly, which is a pain.  To save yourself a step and frustration of not being able to work with IIS remotely after a restart, I’d recommend making the setting change so you never have to worry about not being able to connect again.

If you’ve never heard of [Management Service](https://www.iis.net/learn/manage/remote-administration/remote-administration-for-iis-manager), it basically allows you to remotely manage an IIS instance on one machine from another.  Which can be very handy and save you some time during the day. 