---
title: Windows Authentication in ASP.NET
layout: post
type: posts
tags: [IIS]
categories: [tech]
---

Was doing some research on how Windows Authentication worked in an ASP.Net MVC application I was working on for work and wanted to get research and testing written down for future reference. Hopefully if you read this you’ll find it helpful as well, or if you have any additional info or advice please share them in the comments. 

It looks like in IIS6 & IIS7 when you’re using Windows Integrated Authentication it first tries to use Kerberos to authenticate, then if that fails it tries NTLM authentication, and after that it will stop unless you have basic authentication enabled. Kerberos tries to use your current network credentials to verify authentication, I believe this is ‘Negotiate’ in the HTTP response header. If it can log in using Kerberos the user will receive no username and password request popup and will be directly logged into the site. From my testing it looks as if this only works in IE, tried in Firefox and I was prompted. My testing was on a test app in IIS7 using IE 8, I did have some luck in IIS6 with this as well but was lacking a good testing environment. So if a user is on the network, and Kerberos is working, the user should never have to supply their username and password at the time of page authentication.

__Example HTML response header (Kerberos)__ 
HTTP/1.1 401 Unauthorized Content-Type: text/html Server: Microsoft-IIS/7.5 WWW-Authenticate: Negotiate WWW-Authenticate: NTLM X-Powered-By: ASP.NET Date: Fri, 18 Jun 2010 20:04:25 GMT Content-Length: 1293 Proxy-Support: Session-Based-Authentication 

NTLM will prompt the user for valid username and password credentials for the network login for the particular site, this is NTLM in the HTTP response header. This is normally encrypted, only times this is not encrypted is on a browser IE2 or older, or non major browser. Biggest browser that has been known to not support encrypting NTLM authentication was Chrome, but apparently they have or are going to fix that at some point soon. If NTLM is not supported the credentials get passed as plain text. This is the type of authentication that users receive when they receive a user log in dialog box.

__Example HTML Response Header (NTLM)__
Host: stewie User-Agent: Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US; rv:1.9.2.3) Gecko/20100401 Firefox/3.6.3 Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8 Accept-Language: en-us,en;q=0.5 Accept-Encoding: gzip,deflate Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7 Keep-Alive: 115 Connection: keep-alive Authorization:

NTLM Blah Blah Encrypted stuff who cares If basic authentication is enabled then the site will prompt the user for login credentials and it will pass those credentials to the server via plain text. This only happens if all other log in validation types fail and basic authentication is enabled. If you want to view what data is being passed across the network through you HTTP requests and response I recommend [Firebug](http://getfirebug.com/) if your using Firefox and [Fiddler](https://www.telerik.com/download/fiddler) for IE and Firefox.