---
title: "Trilium Notes Layout for CTFs"
date: 2023-07-28
weight: 1
tags: ["CTF","Notes", "HTB"]
author: "Cole Phillips"
showToc: false
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Trilium Notes"
canonicalURL: "http://colephillips.dev/blog/page/"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "" # image path/url
    alt: "" # alt text
    caption: "" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
editPost:
    URL: "https://github.com/Trailingslashes/colephillips.dev/tree/main/content/"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---
I always thought Cherrytree was the best for note taking until I stumbled across Trilium Notes. Cherrytree is great if you're working with a small database, but once you start adding more screenshots and data it becomes a problem...

https://www.reddit.com/r/oscp/comments/hdxcip/need_help_to_repair_cherry_tree_corrupted_file/
https://www.reddit.com/r/oscp/comments/m0wp3i/help_to_recover_cherry_tree_file/

I personally have lost all my notes in Cherrytree before. Luckily, I was able to restore some of them from db file. It's not really something I'd like to risk if I'm going to sit down for an exam.
I have grown to love Trilium Notes. It's open source!

Some of the features I love:

- Syncing - Trilium Notes enables syncing across multiple devices through its server instance. This ensures your notes are always up to date, regardless of the device you're using. You'll have to start your own [server](https://github.com/zadam/trilium/wiki/Synchronization). I think it's worth it. The devs made a guide on how to setup you're own if you're interested - [Server installation](https://github.com/zadam/trilium/wiki/Server-installation).
- Rich Text Support - Trilium Notes supports rich text editing. This means users can format their notes, embed images, create tables, and even use mathematical notations using LaTeX.
- Sharing - You can easily share a single note with others. When you choose to share a note, Trilium generates a unique URL which you can distribute. Anyone with the URL can view the note. This is particularly useful if you want to share a specific piece of information without exposing your entire note collection. However, sharing does require you to have a server installation.
- My personal favourite, Extensive Search Functionality - Search is _really_ good in Trilium. It features an advanced search functionality that allows users to search their notes based on text, attribute, or relation.

I have a basic template that I work from in Trilium any time I want to do a CTF. I recommend you give it a try. Another popular app that has similar features is [Obsidian](https://obsidian.md).

![Note template](/images/blog/post2/notes.png)
