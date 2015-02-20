---
layout: post
title:  "OpenSSL - p12 certificate from scratch"
date:   2015-01-22 23:15:45
categories: posts
---

#### **create a private key**

{% highlight bash %}
openssl genrsa -des3 -out server.key 2048
{% endhighlight %}


#### **create a certificate signing request**

{% highlight bash %}
openssl req -new -key server.key -out server.csr
{% endhighlight %}

#### **create a DER certificate**

{% highlight bash %}
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
{% endhighlight %}

#### **create a PKCS#12 certificate**

{% highlight bash %}
openssl pkcs12 -export -out server.p12 -inkey server.key -in server.crt
{% endhighlight %}

It's also possible to provide CA's certificate by adding to the latter command

{% highlight bash %}
-certfile CACert.crt
{% endhighlight %}
