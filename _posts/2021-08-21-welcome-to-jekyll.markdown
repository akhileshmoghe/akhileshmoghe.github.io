---
layout: single
title:  "Welcome to Jekyll!"
date:   2021-08-21 12:33:22 +0530
categories:
  - jekyll
  - IoT
  - AWS
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Akhilesh
  - Moghe
---


# Introduction
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

## Markup
Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

### Year
* Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file.
* After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

```
ubuntu@ip-172-31-6-183:~$ df -Th 
Filesystem     Type      Size  Used Avail Use% Mounted on 
udev           devtmpfs   16G     0   16G   0% /dev 
tmpfs          tmpfs     3.2G  824K  3.2G   1% /run 
/dev/xvda1     ext4      194G  128G   67G  66% / 
tmpfs          tmpfs      16G   88K   16G   1% /dev/shm 
tmpfs          tmpfs     5.0M     0  5.0M   0% /run/lock 
tmpfs          tmpfs      16G     0   16G   0% /sys/fs/cgroup 
/dev/loop0     squashfs   33M   33M     0 100% /snap/snapd/11588 
/dev/loop2     squashfs   56M   56M     0 100% /snap/core18/1997 
/dev/loop3     squashfs   34M   34M     0 100% /snap/amazon-ssm-agent/3552 
/dev/loop5     squashfs   56M   56M     0 100% /snap/core18/2074 
/dev/loop4     squashfs   33M   33M     0 100% /snap/snapd/12398 
tmpfs          tmpfs     3.2G     0  3.2G   0% /run/user/1000 
ubuntu@ip-172-31-6-183:~$
```

![useful image](/assets/images/teaser.jpg)

#### Code Snippet
Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

# Further Readings
Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
