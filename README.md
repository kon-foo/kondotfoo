# kon.foo

## Shoulders this Stand On
Written in [Obsidian](https://obsidian.md/) and built with [Quartz](https://github.com/jackyzha0/quartz.git)

## HowTo
Sync changes:
```
npx quartz sync --no-pull
```
Run locally:
```
npm quartz 

### Markdown Syntax

Besides standard Markdown syntax, supported flavours and features include:
- footnotes, strikethroughs, tables, tasklists ([Github Flavor](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax))
- callouts, wikilinks ([Obsidian Flavor](https://help.obsidian.md/Editing+and+formatting/Obsidian+Flavored+Markdown))

### Frontmatter

The frontmatter supports `title`, `draft` and `tags`:
```
---
title: Title of the page. If it isn’t provided, Quartz will use the name of the file as the title.
description: Description of the page used for link previews.
permalink: A custom URL for the page that will remain constant even if the path to the file changes.
aliases: Other names for this note. This is a list of strings.
draft: Whether to publish the page or not. This is one way to make pages private in Quartz.
date: A string representing the day the note was published. Normally uses YYYY-MM-DD format.
tags:
  - example-tag
---
```
The frontmatter keys are parsed by different plugins. See these for customization options:
- [Frontmatter Plugin](https://quartz.jzhao.xyz/plugins/Frontmatter)
- [CreateModifiedDate Plugin](https://quartz.jzhao.xyz/plugins/CreatedModifiedDate)
- [Description Plugin](https://quartz.jzhao.xyz/plugins/Description)


### Configuration

**`quartz.config.ts`**
Pretty self-explanatory. [Docs](https://help.obsidian.md/Editing+and+formatting/Obsidian+Flavored+Markdown)
