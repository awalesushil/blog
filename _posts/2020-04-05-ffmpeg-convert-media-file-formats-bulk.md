---
layout: "post"
title: "How to Convert Media File Formats in Bulk using FFMPEG"
tags: ['ffmpeg','parallel','shell-script']
description: "Converting Media File Formats in Bulk"
comments: true
---

[FFMPEG](https://ffmpeg.org/){:target="_blank"} is a popular open-source tools used to handle media files. It can be used to change audio and video file formats, and other characteristics of the media files. 

Using FFMPEG, converting a single media file is an easy task. However, when you want to update a large number of media files at once, it maybe a challenge. Especially, the duration of the process being directly proportional to the number of files.

While working on a speech-related project at my internship, I faced a similar challenge where I had to convert 157,000 audio files in `.flac` format to `.wav` format. The whole process would have taken hours to complete. But, I decreased the time duration by 8 times using parallel processing.

In this post, I will discuss the steps to use FFMPEG encoding process in parallel.

## Installation

First, we need to install the required tools. We will be using FFMPEG and [GNU Parallel](https://www.gnu.org/software/parallel/){:target="_blank"}.

### Installing FFMPEG

```bash
# Install
$ sudo apt install ffmpeg

# Check if correctly installed
$ ffmpeg -version
```

### Installing GNU Parallel

We will download the tar file, unzip it, build the software and install it.

```bash
# Download
$ wget http://ftp.gnu.org/gnu/parallel/parallel-latest.tar.bz2

# Unzip
$ sudo tar xjf parallel-latest.tar.bz2

# Change directory
$ cd parallel-yyyymmdd 

# Build software
$ sudo ./configure && make

# Install
$ sudo make install

# Check if correctly installed 
$ parallel --version
```

## The One-Line Code

Now we are going to run the following one-line code on the terminal. But, first create a new directory (`new_dir`) where all your converted files are going to be stored.

```bash
$ cd /path/to/directory/with/files
$ ls
audio_dir
$ mkdir new_dir
```

Now the following one-line code.

```bash
find audio_dir -type f -name "*.flac" | parallel -j+0 --eta ffmpeg -i {} new_dir/{/.}.wav;
```

* Here, the first part `find audio_dir -type f -name "*.flac"` is listing the audio files with `.flac` extension and passing it one at a time to the later part.
  
* The latter part `parallel -j+0 --eta ffmpeg -i {} new_dir/{/.}.wav` uses `{}` as the placeholder for the file name which we later use for the new `.wav` file.

Here, I have used `-j+0` to specify **parallel** to use all the available CPUs (*8 in my machine*). The conversion process is running 8 parallel processes hence speeding up the process by 8 times.

You can can change it to specify only a fixed number of CPUs, like `-j 2` for 2 CPUs.

Moreover, you can remove the FFMPEG banner and also make the FFMPEG loglevel quiet as follows:

```bash

find audio_dir -type f -name "*.flac" | parallel -j+0 --eta ffmpeg -hide_banner -loglevel quiet -i {} new_dir/{/.}.wav;
```

If you require more options of encoding, check the official documentation of FFMPEG [here](https://ffmpeg.org/documentation.html){:target="_blank"} and more control options of GNU Parallel [here](https://www.gnu.org/software/parallel/parallel_tutorial.html#GNU-Parallel-Tutorial){:target="_blank"}.


