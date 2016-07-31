---
layout: post
title: Configure Perfect Web Development Environment on Windows - Apache Dynamic Vhost
category: Server
tags: Apache 
date: 2015-11-14
---

Reference to [2015-11-14-What-Happened-When-You-Entered-A-URL-Into-Web-Browser](./server/2015/11/14/What-Happened-When-You-Entered-A-URL-Into-Web-Browser.html) for understanding what is done here if not be able to understand.
<!--more-->

## Apache Vhost Configuration
Take the web site in `D:/Sites/home` as an example.
1. add a new line `127.0.0.1 home.local` to `C:\Windows\System32\drivers\etc\host` file;
2. uncommen `Include conf/extra/httpd-vhosts.conf` in  Apache configuration `httpd.conf`(for XAMPP, located at `C:\xampp\apache\conf`) file;
3. uncommen `NameVirtualHost *:80` in Apache vhost configuration `httpd-vhost.conf` file;
4. add the following lines in to Apache vhost configuration `httpd-vhost.conf`(for XAMPP, located at `C:\xampp\apache\conf\extra`) file:

```
<Directory "D:/Sites/home">
	Options Indexes FollowSymLinks Includes ExecCGI
	AllowOverride All
	Order allow,deny
	Allow from all
	Require all granted
</Directory>

<Virtualhost *:80>
    VirtualDocumentRoot "D:/Sites/home"
    ServerName home.local
    UseCanonicalName Off
</Virtualhost>
```

So, done.

## localhost Problem Solving
After doing the above configuration, I cannot simply request `localhost` and get a working well responce. So what is the problem.  

Em...My guess is because of 'NameVirtualHost *:80', the VHost is listening at *prot 80*, and this is no information configured about the default responce for 'localhost' request, then something unpleasant happen.

To solve this, simply coping the following configuration from `httpd.conf` into `httpd-vhost.conf` does not works well:

```
<Directory "C:/xampp/htdocs">
    Options Indexes FollowSymLinks Includes ExecCGI
    AllowOverride All
    Require all granted
</Directory>
```

In fact, this does nothing, since `httpd-vhost.conf` is just a included part of `httpd.conf`, repeating the above line does nothing!

And the **real solution** should be adding the following block into `httpd-vhost.conf`:

```
<Virtualhost *:80>
    VirtualDocumentRoot "C:/xampp/htdocs"
    ServerName localhost
</Virtualhost>
```

I don't know exactly why this code block is neccessary, but without it, `localhost` request will just get "access forbidden" and the alike response. Maybe the Vhost configuration disturbed `localhost` mapping from orginally `C:/xampp/htdocs` to Apache server root `C:/xampp/apache` which is configured with `<Directory /> AllowOverride none Require all denied </Directory>` in the `httpd.conf` file. So I need using the above code block to led `localhost` back to `C:/xampp/htdocs`. Also notice that, using *DocumentRoot* instead of *VirtualDocumentRoot* works well.

##What is More?
Thanks to the wildcard supporting by apache configuration files, we can do even more.  

We can easily provide Apache access permission to all folders under a certain directory in configuration file `httpd-vhost.conf`. Take my configuration as an example,

```
<Directory "D:/Sites/*">
	Options Indexes FollowSymLinks Includes ExecCGI
	AllowOverride All
	Order allow,deny
	Allow from all
	Require all granted
</Directory>
```

This code block will enable Apache to get files from all folders under `D:/Sites`. *Notice*, the `Allow from all` and `Require all granted` lines are neccessary!

Now, Apache is able to get files from all folders under `D:/Sites`, the next question is when to get files from a certain subdirectory of `D:/Sites`? The answer is as following:

```
<Virtualhost *:80>
    VirtualDocumentRoot "D:/Sites/%1"
    ServerName sites.local
    ServerAlias *.local
    UseCanonicalName Off
</Virtualhost>
```

Yes, the above block is not as straightforword as the afore-mentioned part. Let me crack it down, notice the line `ServerAlias *.local`, there is a wildcard! To make "Alias" working, one ahead job is to uncommen 
`LoadModule vhost_alias_module modules/mod_vhost_alias.so` in `httpd.conf`.

And then this wildcard works! The "%1" then is the content of this wildcard(I am not going into details such as "%2" and so on)!  The last line `UseCanonicalName Off`, I guess just form its name, maybe something about TLD or gTLD.

##What is even more?!
You may notice that, for each website in the `D:/Site` for me, I need to add one line like `127.0.0.1 home.local` into `host` file. That's not pleasant although I do not need to restart my machine or Apache Server after modification just like after modifying Apache configuration file aquering restarting Apache Server to take effects.

You make come up with the idea, using wildcard! Yes! It is while it isn't!  
Bad news is windows host file seems not supporting wildcard.  
By the way, to check whether the *Local Domain Name Server* is functioning well, for Windows user, the *CMD* `ping` command is of help. Try `ping localhost`, `ping home.local` and so on by your self! Or `tracert` is also a good alternative to this!

So what is the solution for this newly problem?  [Acrylic DNS Proxy](http://mayakron.altervista.org) and another one [stackia/DNSAgent](https://github.com/stackia/DNSAgent) should help.(Be careful with the IPv6 setting and VPN connection, which disturbed me a lof!)

And one really good index page is [cmall/LocalHomePage](https://github.com/cmall/LocalHomePage).

So, By yourself!

##Something left?
Yes!  
For me, I still don't understand why when i entered `127.0.0.1` into browser, and I can get the same result as `localhost`! While, not just `localhost` is mapping to `127.0.0.1`, there are also `home.local`, `index.local` and so on...

Or, maybe all the unspecified `127.0.0.1` request is served as `localhost` as default?
