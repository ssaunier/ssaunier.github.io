---
layout: post
title: "GitHub public key authentication"
description: "A diagram explaining how the Public/Private key system works in an authentication scnario."
category: blog
photo: computer.jpg
---

After creating a GitHub account, you need to setup your computer so that
you can `git push`. When cloning a project, you chose `SSH` or `HTTPS`. I
like to clone my projects in `SSH` as it will use my keys.

## SSH Authentication Scenario

Here's a diagram explaining how those keys are used when you want to
`git push` your code.

<div class="center">
  <img src="/images/posts/SSH Connection explained.png" alt="Public Key SSH Connection explained" />
</div>

In this SSH authentication scenario, we can view the **private key as a vault**. When
the client encrypts the message from the server, you can picture it as putting the
message into the vault, and closing it. Then GitHub opens the vault with the public key
and check the message in it is the same as the one he sent. If it's not, it means that your
did not use the vault (private key) matching the public key GitHub has associated to
your profile.

## What you need to know

- You generated, on your computer, **both** a public and private key. They are stored
in the `~/.ssh` folder, and are just **files stored on your hard drive**, usually named `id_rsa.pub` (public key) and `id_rsa` (private key). GitHub has a [nice guide](https://help.github.com/articles/generating-ssh-keys/#step-2-generate-a-new-ssh-key) to help you generate those keys.
- You had to **give GitHub your public key** in order for them to authenticate you. That's
perfectly normal, that's why it's called **public**. Go
to [github.com/settings/ssh](https://github.com/settings/ssh) to see them.
- Your private key is really important. **If someone steals it, they can impersonate
you**, and that's bad as they could have access to your private repos or `git push`
as you.
- That's why you should **always protect your private key with a strong passphrase**. The
private key is just a file on your disk, if left unencrypted, anybody with a USB key
and access to your laptop can steal it and **use** it.
