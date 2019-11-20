# qiansr.github.io
[个人博客](http://qiansr.github.io/)

hexo d -g   编译发布  
hexo server  本地预览


其他配置  _config.yml
```
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: 神刀
subtitle:
description:
author: 神刀
language: zh-Hans
timezone:

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://qiansr.github.io
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:

# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace:
  
# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 10
  order_by: -date
  
# Category & Tag
default_category: uncategorized
category_map:
tag_map:

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next #landscape

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git 
  repo: https://github.com/qiansr/qiansr.github.io.git
  branch: master

# 新增搜索
algolia:  
  applicationID: 'RINL8Q0DQF'
  apiKey: '55343f67f7a297303ab19bd5408f8330'
  adminApiKey: '94ad4def316f59b6f433de81469cc362'
  indexName: 'hexo-blog'
  chunkSize: 5000

# 侧边栏头像
avatar: /images/logo.jpeg

# feed
feed: 
  type: rss2
  path: rss2.xml
  limit: 5
  content: 'true'
```
