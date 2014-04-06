---
layout: post
title: "Setup your new MacBook Pro"
description: "So you've just received a new laptop for X-mas? Time to set it up!"
category: blog
---

This post will serve as a memo of the basic actions I like to perform on a new MacBook Pro to feel at ease. This is a semi-automatic configuration, I know that all of this could be scripted, so if you are looking at a reproductible method to install a laptop at your company, look for GitHub or ThoughtBot scripts.

## Basic Security

You should turn on FileVault. This way, your hard drive is encrypted, and if your laptop is stolen, the thief won't be able to access your data. Go to System Preferences > Security & Privacy > FileVault and turn it on. Before booting you will be prompted for the encryption password.

You should enable an automatic screensaver with the password required to log in again. This way, while away, people won't be able to perform bad actions with your data or your browser sessions. Go to System Preferences > Security & Privacy > General. I like to set up the time after sleep or screen saver begins at **5 seconds**, because I use the "Hot Corners" (System Preferences > Mission Control > Look at the bottom button) bottom left corner to enable the screensaver. When I leave my seat, I just put my mouse there and the screen locks. If I put my mouse in this corner by mistake, I have 5 seconds to move it elsewhere without entering my password again.

Turn on the firewall, in System Preferences > Security & Privacy > Firewall.

## Sublime Text

I use Sublime Text 2 as my text editor. Grab it, set up your license, and [install the package manager](https://sublime.wbond.net/installation#st2). Then install the following packages:

- Theme - Soda
- Emmet
- GitGutter
- SideBarEnhancements
- TrailingSpaces

## Basic hacking configuration

I assume you run Mavericks.

```bash
$ xcode-select --install
```

Install [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) for a fancy shell.

```bash
$ curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```

Then open your terminal preferences and set the "Pro" theme as default. Quit and relaunch it.

Install HomeBrew to `brew install` all the things.

```bash
$ ruby <(curl -fsS https://raw.github.com/Homebrew/homebrew/go/install)
$ brew doctor
```

Some basic programs to have on your computer.

```bash
$ brew install vim wget git hub
```

I then copy paste my old `~/.ssh` folder to this new computer.


## Ruby!

I use [`rbenv`](https://github.com/sstephenson/rbenv) in combination with
[`ruby-build`](https://github.com/sstephenson/ruby-build).

```bash
$ brew install rbenv rbenv-gem-rehash ruby-build
$ rbenv install 2.1.0
$ rbenv global 2.1.0
$ gem update --system
$ gem install bundler foreman rails
$ bundle config --global jobs `expr $(sysctl -n hw.ncpu) - 1`
$ brew install heroku-toolbelt
$ gem install jekyll  # Static Blogging FTW!
```

## Dotfiles

I then reinstall my [dotfiles](http://github.com/ssaunier/dotfiles). You can fork
my repo to configure it to you needs.

```bash
$ mkdir -p ~/code/ssaunier && cd $_
$ git clone git@github.com:ssaunier/dotfiles.git && cd dotfiles
$ ./install.sh
```

## MacOSX

You want your system to be as responsive as possible. For instance, blazing
fast keyboard repeat rate. You get that with:

```bash
$ defaults write NSGlobalDomain KeyRepeat -int 2
$ defaults write NSGlobalDomain InitialKeyRepeat -int 15
```

You should explore [@mathiasbynens's `.osx`](https://github.com/mathiasbynens/dotfiles/blob/master/.osx)
file for the perfect Hacking configuration.

## Misc

I like to switch between terminal tabs with **⌘ →** and **⌘ ←**, there is a
[trick](http://superuser.com/a/54004) to configure this.

My 2014 year is to use 1password and to change all my old passwords. I plan not to
use the iCloud Keychain and store as little as possible in there. I also plan to
investigate Docker to isolate my coding environments switching from one client to
the other.
