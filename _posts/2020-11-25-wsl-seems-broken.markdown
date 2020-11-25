---
layout: post
title:  "WSL seems broken"
date:   2020-11-25 18:05:55 +0300
image:  01.jpg
tags:   [WSL]
---
I started my ubuntu installation in Windows Subsystem for Linux today for the first time in a while. I am running focal aka Ubuntu 20.04 so I got the standard notification about you should update it. I did the standard command of 
{% highlight terminal %}
sudo apt update
{% endhighlight %}
And got this response
{% highlight terminal %}
Hit:1 http://archive.ubuntu.com/ubuntu focal InRelease
Get:2 http://security.ubuntu.com/ubuntu focal-security InRelease [109 kB]
Get:3 http://archive.ubuntu.com/ubuntu focal-updates InRelease [114 kB]
Get:4 http://archive.ubuntu.com/ubuntu focal-backports InRelease [101 kB]
Reading package lists... Done
E: Release file for http://security.ubuntu.com/ubuntu/dists/focal-security/InRelease is not valid yet (invalid for another 1d 8h 48min 41s). Updates for this repository will not be applied.
E: Release file for http://archive.ubuntu.com/ubuntu/dists/focal-updates/InRelease is not valid yet (invalid for another 1d 9h 17min 55s). Updates for this repository will not be applied.
E: Release file for http://archive.ubuntu.com/ubuntu/dists/focal-backports/InRelease is not valid yet (invalid for another 1d 6h 36min 44s). Updates for this repository will not be applied.
{% endhighlight %}

This got me wondering, but I discarded this as generic error and tought to fix that later. But going over to other projects and trying npm update and gem update all came back with SSL/TLS errors whenever I tried to run a command against it.
{% highlight terminal %}
Fetching source index from https://rubygems.org/

Retrying fetcher due to error (2/4): Bundler::Fetcher::CertificateFailureError Could not verify the SSL certificate for https://rubygems.org/.
There is a chance you are experiencing a man-in-the-middle attack, but most likely your system doesn't have the CA certificates needed for verification. For information about OpenSSL certificates, see http://bit.ly/ruby-ssl. To connect without using SSL, edit your Gemfile sources and change 'https' to 'http'.
{% endhighlight %}
 
This got me wondering and digging through the internet. Found random posts about makeing sure I have the correct CA-list and make sure my toolchain was up to date, but finally tried to go back to the original problem of apt update failing and googled that. That ended up in a [ask ubuntu][ask-ubuntu] post that had this magic command listed as a resolution
{% highlight terminal %}
sudo hwclock --hctosys
{% endhighlight %}

The problem was that my clock in WSL was drifting and since both the apt feeds and rubygems was running new certificates they where still not valid and rejected by openssl

[ask-ubuntu]: https://askubuntu.com/a/1169203