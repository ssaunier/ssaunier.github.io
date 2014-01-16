---
layout: post
title: "Stay safe on unencrypted public Wi-Fi"
description: "Set up a poor's man VPN, fowarding all your network traffic to a server of yours through SSH"
category: blog
---

<img class="inline pull-right" src="/images/posts/garden.jpg" alt="Parisian Park" />

When working from a café or a parisian park, the tentation to connect to this
unprotected public Wi-Fi is great. And you may know that all the web traffic not
going through `HTTPS` is transmitted in clear for everybody connected to the
same network to see! Basically, anybody sniffing the HTTP packets on the network can
harvest your cookies and impersonate you. Remember
[Firesheep](http://codebutler.github.io/firesheep/tc12/). Here is a solution to
keep your Linux or Mac laptop safe.

## Tunnelling to the rescue

We want to encrypt the HTTP traffic passing through this public wi-fi. We can use a
transparent proxy which will forward all your network traffic through a SSH
connection to a server of yours, using [sshuttle](https://github.com/apenwarr/sshuttle).

### Pre-requisites: somewhere to SSH

If you don't already own a server with SSH access, you can use AWS EC2 and create
a free micro-instance through your [AWS Console](https://console.aws.amazon.com/ec2/v2/home).
It's really easy. When creating the instance, you will be provided with a `.pem` file.
Put it in your `~/.ssh` folder and `chmod 600` it.

### (Optional) Use your domain

You can setup a CNAME with your domain name if you own one. I named my micro-instance
`chaton`, kitten in French.

```bash
$ dig +short chaton.saunier.me
ec2-54-194-5-226.eu-west-1.compute.amazonaws.com.
```

You can skip this step and use the public DNS name of your instance instead. Just
replace each mention of `chaton.saunier.me` in the following snippets.

### Setup an SSH config

Open your `~/.ssh/config` file and add a new configuration for this host.

```bash
Host chaton
  HostName chaton.saunier.me
  User ubuntu
  IdentityFile ~/.ssh/chaton.pem
```

That way, you'll be directly connected to your micro-instance with:

```bash
$ ssh chaton
```

(If it does not work, make sure to `chmod 600 ~/.ssh/config`)

### Get sshuttle

You need to clone the github repository. For instance:

```bash
$ cd ~/code/python
$ git clone git://github.com/apenwarr/sshuttle
$ cd sshuttle
$ ./sshuttle --dns --remote=chaton.saunier.me 0/0
```

The last line is here to manually check that `sshuttle` behaves correctly.
On Mac OSX, you may need to reboot.

### Create handy aliases

Open your `.aliases` file or your `.zshrc` / `.bashrc`, and put the following
two lines:

```bash
alias ip="curl ipinfo.io/ip"
alias tunnel='~/code/python/sshuttle/sshuttle --dns \
              --daemon --pidfile=/tmp/sshuttle.pid --remote=chaton 0/0'
alias tunnelx='[[ -f /tmp/sshuttle.pid ]] && kill $(cat /tmp/sshuttle.pid) && echo "Disconnected."'
```

Don't forget to put your own server in place of `chaton`

### Encrypt your HTTP traffic!

That's it! After sourcing your configuration file, you should be able to run

```bash
$ ip  # Your IP without the tunnel
$ tunnel
$ ip  # Your IP with the tunnel
```

## Ready to get things done!

That's it, next time you want to connect to an unencrypted Wi-Fi:

1. Close your Browser and Mail client
1. Connect to the evil unencrypted Wi-Fi
1. Launch a Terminal and run `tunnel`
1. Enjoy the Internet!