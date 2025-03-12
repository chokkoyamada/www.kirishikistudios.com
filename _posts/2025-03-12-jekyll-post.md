---
layout: post
title: jekyllのpostをコマンドで生成する
date: 2025-03-12 10:13 +0900
categories: jekyll
---

jekyll 初心者なので、ブログ記事(post)を作るのに毎回 md ファイルを手動で作るのが面倒だと思っていたら、jekyll-compose という gem があることを知ったのでメモ

以下を Gemfile に追加して `bundle install` する。

```
gem 'jekyll-compose', group: [:jekyll_plugins]
```

あとは以下で現在時刻で md ファイルを生成できる。

```
bundle exec jekyll post "postsをコマンドで生成する"
```

jekyll compose の GitHub Repository

[jekyll/jekyll-compose: :memo: Streamline your writing in Jekyll with these commands.](https://github.com/jekyll/jekyll-compose)
