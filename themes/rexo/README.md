> Related post: [Multilingual blogging with Hexo](http://bluemix.me/en/2017/multilingual-blogging-with-Hexo/)

## Rexo = RTL + Hexo 
[Live Example](https://bluemix.github.io/)



Rexo is a minimally customized version of [Vexo](https://github.com/yanm1ng/hexo-theme-vexo)

  ![screenshot](screenshot.png)


## Features

* RTL (e.g., Arabic, Hebrew, and Persian)
* Disqus comments
* Emoji left sidebar



## Post

The **front-matter** of a post looks like that:

```yaml
---
title: زهرة النرجس
date: 2017-07-27 10:42:29
lang: ar
direction: rtl
tags:
 - أزهار
 - نباتات
---
```

  **Please note**
  
  You can use [rtl](https://github.com/bluemix/hexo-tag-rtl) and [ltr](https://github.com/bluemix/hexo-tag-ltr) plugin tags to allow mixed wirtings, i.e.,
```
مقتطفات من بعض الحكم

{% ltr %}
“A small leak will sink a great ship.” - Benjamin Franklin
{% endltr %}

"لو أنك عشتَ في الماضي و تصرفت كأنك في الماضي ، سوف يكون صعباً على المُستقبلِ أن يراكَ." - Body of Lies

```

  ![mixied-writigs](mixed-writings.png)
## Install

1. Download/Checkout this theme into your project

   ```
   cd your-hexo-folder

   git clone https://github.com/bluemix/hexo-theme-rexo.git themes/rexo

   cp -R themes/rexo/_source/* source/
   ```

2. Edit your `_config.yml` theme paramters with `rexo`
3. Modify the theme's [`_config.yml`](/_config.yml) with your own info

    About page can be fount at [about.ejs](/layout/about.ejs), that has parameters from
    the theme's [`_config.yml`](/_config.yml) 

   ```
    # Comment
    comment:
        disqus: bluemixme

    about: 
      avatar: https://avatars3.githubusercontent.com/u/3332274?v=4&s=460
      description: Peace  ☮️  السلام 
      long_description:
    social:
      github: https://github.com/bluemix
      github_username: bluemix
      stackoverflow: https://stackoverflow.com/users/1150190/abdulmomen-عبدالمؤمن
      twitter: https://twitter.com/bluemix2
      facebook: https://fb.com/bluemix
      linkedin: https://www.linkedin.com/in/bluemix2
      instagram: https://instagram.com/bluemix
   ```

## Logo
You can replace your own logo in `themes/source/css/images` 

## Links

You can easily add your social account by just adding your settings into `_config.yml`



## LICENSE
MIT