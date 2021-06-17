---
toc: true
layout: post
description: Running Python scripts on server over ssh and getting back content
categories: [setup]
title: Running Python scripts on server over ssh and getting back content
---


Context: I upload GIFs to my blog from the screencasts I create on my iPad. a-shell provides ffmpeg, but, it runs out of memory often.

Solution: Send video to server and run a Python script to convert to GIF and get the content back

I use two scripts:

First called convert-video-gif.py that uses moviepy (FFMPEG under the hood) to convert an MP4 to a GIF

```python
from moviepy.editor import *
import sys

vid = sys.argv[1]
vid_name = vid.split(".")[0]
clip = (VideoFileClip(vid).speedx(10).resize(0.3))
clip.write_gif(vid_name + ".gif")
```

Second called convert_video.py that accepts:
- a video file as a command line argument
- transfers the video files to the server using SFTP
- transfers the convert-video-gif.py file to the server using SFTP
- runs the python file convert-video-gif.py on the server and create the GIF
- retreive the GIF back to my local machine


```python

import sys
import paramiko

# The file you wish to convert to GIF
f = sys.argv[1]
f_name_without_ext = f.split(".")[0]
f_name_gif = f_name_without_ext + ".gif"

ssh_client =paramiko.SSHClient()
ssh_client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
ssh_client.connect(hostname='MY-HOSTNAME', username='MY-USERNAME', password='MY-PASSWORD')
ftp_client=ssh_client.open_sftp()

# Transferring convert-video-gif.py to server
ftp_client.put('convert-video-gif.py', 'convert-video-gif.py')

# Transferring video to server
ftp_client.put(f, f)

# Running Python script
stdin, stdout, stderr = ssh_client.exec_command(f"python convert-video-gif.py {f}")
e = stderr.readlines()

# Retreiving the video back from server
ftp_client.get(f_name_gif, f_name_gif)

```





 
 
