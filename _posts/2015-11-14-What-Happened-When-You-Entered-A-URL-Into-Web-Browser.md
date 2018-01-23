---
layout: post
title: What Happened When You Entered A URL Into Web Browser - Both Browser and Server Side Explanation
category: Server
tags: Apache 
date: 2015-11-14
---

Understanding the processes behind helps you make a better configuration.
<!--more-->

## Procedures After URL Entered in Browser
- The client browser will at first send the url to the *Local Domain Name Server*. I assume *Local Domain Name Server* is representated/configured by `C:\Windows\System32\drivers\etc\host` file. Contents in `host` will determine the next destination of this *URL Request*. If the *domain* of the *URL* I entered in browser matches one record/line in the `host`  file, then the *Local Domain Name Server* use the *IP Address* in this matched record/line as the *Will-Requested Server IP*.
    
    {% highlight bash %}
    Example:
    After I entered `home.local` into browser(Chrome Url Feild), the browser then send this *URL* to *Local Domain Name Server*. If there is one line `127.0.0.1 home.local` in the `C:\Windows\System32\drivers\etc\host` file, then the *URL Request*(I use *URL Request* instead of *HTTP Request* to make it easier to understand) to the *Server with ip address 127.0.0.1*.
    {% endhighlight %}

- B-S description for using one machine/PC to development(as both Browser Client and Server): in the first step, my machine acts as the *B* of *B/S Structure*, i.e., the Client; in the following steps, my machine acts as the *C* of *B/S Structure*, i.e., the Server.

- After the *Server with ip address 127.0.0.1* recieved the *URL Request*, the *Machine/Hardware Server* run the *Apache/Software Server* to serve the *URL Request*.

- When *Apache Server* recieved the *URL Request* which contains the *Requested URL*(if any) and *Source IP* information, *Apache Server* check it confuguration file `httpd.conf` for using which files to serve the *URL Request*.

- If vhost is enable in `httpd.conf` file, i.e., `Include conf/extra/httpd-vhosts.conf` is uncommened, *Apache Server* would aslo check `httpd-vhosts.conf` for using which files to serve the *URL Request*.

- Information in `httpd.conf` and `httpd-vhosts.conf` deciede both where the *Apache Server* to find the *Web Files* and also whether *Apache Server* is allowed to access the *Web Files* to serve the *URL Request*. (And also need the permission from Server OS User Right Control, check other configuration/settings in this process, but that is not the topic here). The information used from the *URL Request* here includes the *Requested URL* which contains *Domian Name*, *Path*, *GET Paremeter* and *POST Parameter*(if any).

    Example:
    If the *Requested URL* - `home.local` is in the *URL Request* without *Domian Name*, *Path*, *GET Paremeter* and *POST Parameter*. And there is the following configuration in `httpd-vhost.conf`:

    ```
    NameVirtualHost *:80

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

    With `Include conf/extra/httpd-vhosts.conf` uncommened in `httpd.conf`. Then *Apache Server*  can get file from `D:/Sites/home`.

- If *Apache* get the *Web Files*, then it emploies *Web File Preprocessor*(such as PHP for .php file, Python for .py file and so on) to process the *Web Files* to product *HTML FIles*(of course also css, blablabal...), and then send the *HTML Files* to the request *Client*.

- After the *Client* recieved *HTML Files*, it use the browser to display it! And that is what you see in the web browser.

### Sorry
Sorry about the wrong unnumbered list, I still don't understand markdown numbering list throughout. Especially with jekyll `highlight`  and `endhighlight` block since pure markdown doesn't rendered well!
