---
layout: post
title: Docker DNS with systemd
---

This applies at least do Docker 1.9.1 on host Ubuntu 15.10.

Docker uses as default google name servers if no external server is defined in resolv.com:
{% highlight text %}
No non-localhost DNS nameservers are left in resolv.conf.
Using default external servers : [nameserver 8.8.8.8 nameserver 8.8.4.4]
{% endhighlight %}

It seems that the firewall has problems with these nameservers within a container. For example when creating an own image in the Docker tutorial :
{% highlight text %}
E: Failed to fetch http://archive.ubuntu.com/ubuntu/pool/main/r/recode/librecode0_3.6-21_amd64.deb
Could not resolve ‘archive.ubuntu.com’
{% endhighlight %}
 
## Solution:

### 1. Add other nameservers in /etc/default/docker:
{% highlight ini %}
DOCKER_OPTS="–dns 208.67.222.222 –dns 208.67.220.220"
{% endhighlight %}

### 2. As systemd is not using this config file (only Upstart and SysVinit), load and use it in /etc/systemd/system/docker.service.d/docker.conf:
{% highlight ini %}
[Service]
EnvironmentFile=-/etc/default/docker
ExecStart=
ExecStart=/usr/bin/docker daemon $DOCKER_OPTS -H fd://
{% endhighlight %}
(Empty ExecStart is important.)

### 3. Reload configuration and restart:
{% highlight bash %}
sudo systemctl daemon-reload && sudo service docker restart
{% endhighlight %}
