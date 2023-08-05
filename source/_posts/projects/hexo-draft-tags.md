---
title: Hexo Draft Tags
subtitle: Draft content plugin for Hexo.js
category: projects
permalink : projects/hexo-draft-tags/
cover_image: 'drafts_cover.jpg'
tags:
show_date: false
---

![A draft table with a man's hand holding a pencil over a piece of draft paper. A metal ruler is in the foreground. Photo by Daniel McCullough on Unsplash.](drafts_post.jpg)

I've been using [Hexo](https://hexo.io/) to create websites, including this one, for some time. It allows me to write posts in markdown and then have them render into the website automatically.

Hexo allows for third-parties to create plugin to extend it's functionality. One feature that I noticed was missing was the ability to hide text from being rendered in production mode if it wasn't quite completed yet. By default you can mark an entire post as a draft, which means that it will show when running with the `--draft` option and not in production, but you can't exclude part of a post.

To fix this I created [hexo-draft-tags](https://www.npmjs.com/package/hexo-draft-tags), a very small plugin that defines tags to exclude blocks of text from the production rendering of your website.

The plugin is available on npm [here](https://www.npmjs.com/package/hexo-draft-tags) and the source code can be found on my GitHub [here](https://github.com/CallumHewitt/hexo-draft-tags).
