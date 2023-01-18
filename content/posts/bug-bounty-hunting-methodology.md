---
title: "Bug Bounty Hunting Methodology"
author: "theo0k"
description: "theo0k's Bug Bounty Hunting Methodology"
date: 2022-02-10
---

  

In this blog we will talk about:

  

** 1) Google Dorking **

** 2) Shodan Dorking **

** 3) Subdomain finders and Wordlists **

  
  

# Google Dorking

Google Dorking is a hacking technique that someone can use to locate valuable data or hard-to-find content.

  

*Some of the most interesting and important dorks for me are:*

  

**`site:*-*.example.com`** (In this case we can find different subdomains except of *.example.com)

  

**`site:*.example inurl:admin`**(Using inurl:admin, we can find interesting information related to admin)

  

**`site:example.com inurl:item.html?id={your XSS payload}`** (In my case I had found reflected XSS in one ArcGIS endpoint like that, so you can always try your XSS payload there.)

  

**`site:example.com filetype:log`** (In case you find exposed log files. Instead of the log, you can try different other filetypes like doc, docx, xls, pdf, etc.)

  

**`site:example.com inurl:main.html`** (In my case I had found a subdomain which contained a parameter that was vulnerable to Reflected XSS)

  

**`site:*.*.*.example.com`** (In this way you can find subdomains which is difficult to find by adding *.example.com. Generally the trick here is to add many * in order to find exclusive subdomains)

  

# Shodan Dorking

  

Shodan is a search engine that lets the user find much different information(IPs, servers, clouds, etc.).I prefer Shodan because you can find more valuable information compared with Google Dorking.

  

*Some of the most interesting and important dorks for me are:*

  

**`ssl:"example.com" 200`**(In this way, we can find all working IPS, subdomains of our domain. In most cases, you will find many interesting subdomains, but be sure that these subdomains belong to the company that we are searching for.)

  

**`hostname:"example.com"`** (In this case I think that you will get less results compared with ssl,but you can give it a try).

  

**`ssl:"example.com" 200 http.html:”admin”`**

  

**`ssl:"example.com" 200 http.html:"system"`** (You can try different other keywords which are seems dangerous for the public.)

  

**`ssl:"example.com" 200 http.html:"administrator"`**

  

**`ssl:"example.com" 200 http.html:"login"`** (If you ever find a login panel which belongs to an administrator, always search on google for default credentials or try the most common like admin:admin, root:root etc.)

  

**`http.html:"HP \&raquo; Device Status"`** (We can find unauthorized access to HP printer and we can try in login panel default credentials(admin:admin).

  

**Also in Shodan when you are searching something, always check the *Title* and *common-name* to be sure that we found something important.**

  

![](https://files.bitwarriors.net/blog_posts/bug_bounty_hunting_methodology/shodan_results.png)

  
  

# Subdomain Finders

  

If you are doing manual recon like me, you can use the following websites in order to find subdomains,DNS info, etc.

  

** 1) ** https://subdomainfinder.c99.nl/

** 2) ** https://securitytrails.com/

** 3) ** https://spyse.com/

  

These 3 subdomain finders are the best for me because they are free and they provide much information based on what you want.

  

# Wordlists

Some of the wordlists that i am using during my recon are:

**1.** [BIG Wordlist](https://www.mediafire.com/file/faesasd8tk5chra/BIG+Wordlist.txt/file) *(It containts 1M endpoints)*

**2.** [403 Wordlist](https://www.mediafire.com/file/dgt2d3h580ko487/403+Wordlist.txt/file ) *(403 Wordlist Bypass)*

**3.** [HTTP Headers](https://www.mediafire.com/file/1mwd5vy25q0h55a/HTTP+Header+X-.txt/file)
  
**4.** [Windows IIS Wordlist](https://www.mediafire.com/file/6wc89upgy44so5o/IIS+Wordlist.txt/file)

  
**Keep Hunting and I am sure that you will succeed!**


###### theo0k's social
- [GitHub](https://github.com/theo0k)
