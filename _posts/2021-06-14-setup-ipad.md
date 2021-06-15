---
toc: true
layout: post
description: My iPad computing setup
categories: [setup]
title: My iPad Setup
---

# Caveats and Story
My laptop is broken. I am away from office. I have an iPad Pro 2020. I got my office desktop's magic keyboard and trackpad. In this post I am discussing if iPad can help given that I do not have (physical) access to my main computers. Would I recommend this over a main computer - No! But, can you do some things on the iPad reasonably well enough given keyboard and trackpad - Yes! 


# The Hardware

The magic keyboard and the magic trackpad greatly increase the iPad experience and make it programmer friendly.

![]({{ site.baseurl }}/images/IPAD-Photo.jpeg "Magic Keyboard and Trackpad")


# Setting up an iPad for programming

## Setting up the terminal app (a-Shell)

First, after installing a-Shell, I like to set the font size, terminal background and foreground color. Here is how the a-shell app looks like


![]({{ site.baseurl }}/images/A-shell-initial.png "a-shell on the iPad is my favourite app!")

`config -b black -f white -s 20`

Sometimes I like using vim for editing documents and interfacing with WorkingCopy. a-Shell provides vim!

I like to setup a `bookmark` to the WorkingCopy folder so that I can direcly edit files in that location.

I do so by:
writing `pickFolder` in a-Shell and setting it to WorkingCopy folder. Now I can set a bookmark to this location. I do so by: 
`bookmark git` in the current location (set by pickFolder)

![]({{ site.baseurl }}/images/Bookmark-Git.png "Setting Bookmark location to WorkingApp -> git")

Interestingly, the latest testflight version of a-shell also provides a "Git-like" interface called `libgit2`. Configuring it requires specific steps that I'm writing below. Some of these steps are borrowed from this [nice tutorial](https://devmarketer.io/learn/set-ssh-key-github/) and some are specific to a-shell that I was able to get working courtesy a Twitter discussion with the creator of a-shell. 

![]({{ site.baseurl }}/images/Twitter.png "Twitter discussion with the creator of a-shell")

Now, the steps.

First, we need to create a new ssh key.

We do so by 
```bash
ssh-keygen -t rsa -b 4096 -C "email@domain.com"

```

While configuring I did not setup the passphrase.

The private and public keys are stored in `.ssh` with the name id_rsa 

```bash
$ ls -lah .ssh|grep "id"
-rw-------   1 mobile  mobile   3.3K Jun 14 15:43 id_rsa
-rw-------   1 mobile  mobile   747B Jun 14 15:43 id_rsa.pub
```

Next, I copied the public key in a newly generated ssh key in Github and gave it a name.

Next, I modified `.gitconfig` as follows

```bash
$ cat .gitconfig 
[user]
        email = MY EMAIL ID
        name = MY NAME
        identityFile =  id_rsa 
```

Now, I was almost done!

I was able to push and pull from some old Github repositories but the same did not work with the newer repositories. Again, after a discussion with the creator of a-shell, I figured, this was due to the fact that Github changed the name of the default branch as "main" instead of the earlier "master" whereas libgit2 implementation was expecting "master".

As a quickfix I renamed the branch on my Github repo as "master" and for now set the default branch to be named "master".

Finally, I am able to pull and push to the repositories. The next image infact is showing commits and pushes made to the repository generating the blog post you are reading. 


![]({{ site.baseurl }}/images/libgit-2.png "libgit2 push and commiting")


## Using the WorkingCopy App

WorkingCopy is a very nicely made Git app on the iPad. It is one of the best made apps I have used!

I'll let the pictures do the talking.

![]({{ site.baseurl }}/images/Working-Copy-1.png "WorkingCopy App interface")

![]({{ site.baseurl }}/images/Working-Copy-2.png "WorkingCopy App interfaces nicely with the Files app!")

![]({{ site.baseurl }}/images/Working-Copy-3.png "WorkingCopy App can nicely show the diffs")




## Editors

I use one of the following for editing:

* Koder App
* WorkingCopy App
* vim (in a-Shell)

# Setting Linux/Mac machine and iPad for Python programming (via Jupyter notebooks)

On the Linux/Mac (remote server) machines I set up Jupyter notebooks as follows:

First, I create the configuration file via: `jupyter notebook --generate-config`

Next, In `~/.jupyter/jupyter_notebook_config.py` change

```
python
c.NotebookApp.ip = '0.0.0.0'
c.NotebookApp.allow_origin = '*'
c.NotebookApp.port = SET YOUR PORT
c.NotebookApp.open_browser = False
```

Set password using:
```
jupyter notebook password 
```

Run notebook as:
```bash
nohup jupyter notebook& 
```

On the client end (iPad in this case), I use one of the following two ways to access remote Jupyter instances:

* Using the Juno Connect app
Juno connectly nicely saves all the configuration and can do the necessary port forwarding

* Using Safari and opening the remote Jupyter URL
Just open the remote URL with the set port number. I use my workplace recommended VPN client to get into my workplace network and access non-public IP based servers.

# VNC connection

I tried a few VNC software and found AnyDesk to work fairly well. I did a bunch of small settings tweak.
* I setup a password on my remote machine for uninterruped access 
* I put AnyDesk in the login items on my remote machine so that it starts when the system boots

# Email client

I like the Spark app. 

- The interface is pretty clean. 
- It has a bunch of keyboard shortcuts making it nice to use with an external keyboard. 
- It is trivial to interface with various cloud providers.
- It is trivial to save an email as a PDF.
- It is trivial to create a "link" to an email for sharing. 

# PDF management

The iPad Files app is a fairly well made app. Perhaps not as functional as the Mac Preview app, but, does some things well.

For instance, merging multiple PDFs is easier (for me) than doing the same on Mac. One just needs to select multiple files and then "right" click to "Create PDF"

![]({{ site.baseurl }}/images/CombinePDF.gif "Combining PDFs is trivial on the iPad using Files app")

I use the following ffmpeg command (inside a-Shell) to create this GIF courtesy this excellent tutorial from [GIPHY folks](https://engineering.giphy.com/how-to-make-gifs-with-ffmpeg/)

```bash
ffmpeg -i Video.MP4  -filter_complex "[0:v]  fps=12,scale=720:-1,split [a][b];[a] palettegen [p];[b][p] palette
use" CombinePDF.gif
```

Another alternative is to use the [following shortcut](https://www.icloud.com/shortcuts/f9a9c2cc6e61476abf2e35f654869553&xcust=1-1-232739-0-0-0&
) to convert the screen recorded video into a GIF 


# Some Safari shortcuts
- Command + W for closing a tab
- Command + L for going to the address bar (sometimes on tabs I am not automatically on the address bar)


# Conclusions (and rants)

For the long run, ofcourse, I would not recommend this kind of a setup. There is some learning and fun in getting a "resticted" device to function well, but as a main machine, probably no, at least not for the next few months. A comparably priced laptop (say M1 Macbook Air) would offer a lot more.

There are many things I'm missing

- Projection to a projector and screen recording do not work at the same time. 
- No proper dev environment like VSCode
- A lot of screen size is "wasted" due to the OS
- A lot of the apps are not as good as I'd like. For instance, perhaps a minor thing, but the Youtube app doesn't have a keyboard shortcut for full screen. One can ofcourse do the same on Safari.
- Safari though much improved over the years is not as functional as Safari on the Mac
- While quicklook/Files is good, the functionality of something like Preview is hard to beat
- The files app can not show hidden files and folders


Some of the above may be resolved over the next updates and the major iPadOS15 updates. Though, I do not expect most of them to be resolved anytime soon. 




 
 
