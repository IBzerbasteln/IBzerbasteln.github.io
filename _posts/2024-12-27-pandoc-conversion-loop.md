---
layout: post
title: Keeping Obsidian and MS Word in Sync with Pandoc
date: 2024-12-26 02:03:12-0400
description: Annoyed by having to manually convert your .md file over and over again? Here's an easy fix!
tags: academic-writing Obsidian Pandoc
categories: PKMS
featured: true
related_posts: false
---

> This post details a solution for academic writing within my my personal knowledge management system using **Obsidian**. In case you're unfamiliar with this free programme: [Obsidian](https://obsidian.md/) is a tool that allows you to edit and link locally stored [Markdown](https://www.markdownguide.org/) files. For a more thorough introduction, check out [this video](https://www.youtube.com/watch?v=OUrOfIqvGS4).

# Academic Writing: in Obsidian, and with Pandoc

One of the many things I've come to do within my vault is academic writing: a task which I have migrated to Obsidian quite early into my personal knowledge management journey. Since late 2021, I do virtually all my academic writing in Obsidian.[^1] This is not the place to outline my entire workflow in detail but I will credit two fantastically detailed write-ups by [Regina Martínez Ponciano](https://martinezponciano.es/2021/04/05/research-workflow-as-a-phd-student-in-the-humanities/) and [Chris Grieser](https://web.archive.org/web/20211007182222/https://chris-grieser.de/a62298be91934043b11006be1ddc553a) which have helped me a great deal in developing it. I'll just give a brief overview over the writing workflow per se to contextualise the solution I'm presenting here.

[^1]: The chief exception are co-authored publications for which I have not yet found a sufficient collaborative functionality within Obsidian.

Essentially, I rely on **[Pandoc](https://pandoc.org/)**: a free software that converts a broad range of different text formats. For my academic uses, that typically involves `.md`, `.docx`, and `.pdf` but dozens of other formats are supported. Pandoc offers a broad range of functions which are set out in detail in its [documentation](https://pandoc.org/MANUAL.html), including the rendering of citations, the use of formatting templates, and many, many more.

Most of the time, I use Pandoc to convert an academic text that I have created in Obsidian in Markdown format into a `.docx` that I can send to collaborators, my supervisor, or submit to a journal. Pandoc uses a CLI (command line interface), or as we technically less averse individuals know it: the scary black screen with the white letters. Such a command will typically look something like this:

```
pandoc --citeproc --reference-doc="template.docx" "paper.md" -s -o "paper.docx"
```

This command instructs Pandoc to convert the file `paper.md` into the file `paper.docx`, to process the citations in the Markdown file, and to apply the format of `template.docx` to the newly created document.

For those seeking to avoid using the CLI, Olivier Balfour has developed the **[Obsidian Pandoc Plugin](https://github.com/OliverBalfour/obsidian-pandoc)** which allows calling Pandoc from the command palette within Obsidian. This plugin also allows you to style an HTML output using CSS, or to add arguments like bibliographies or templates to the conversion command.

# The Hassle of Manual Conversion

Pandoc makes document conversion pretty smooth and effortless, especially once you've figured out the conversion command for the respective file. At that point, you can just keep converting it using the command palette, or re-using the same command on the CLI. But: You have to manually issue a command every time you want to convert your `.md` file into a Word document.

This is something I've found increasingly cumbersome, particularly in contexts where I'm dealing with word counts or page counts. Here, I have no way of knowing exactly how many A4 pages a non-paginated `.md` file will consist of, and Obsidian's native word count is rather rudimentary and will not correspond to that of Microsoft Word after conversion.[^2] For example, when I was submitting assignments during my master's that had a word limit of 2,000, I had to make sure that the `.docx` file would not have more than two thousand words as I was working on it. I quickly grew tired of manually converting the file over and over again so I thought of something.

[^2]: Note, however, that there is a [Better Word Count plugin](https://github.com/lukeleppan/better-word-count).

# Syncing Obsidian with MS Word

My solution consisted of creating a Windows batch file (`.bat`) that automatically executes the conversion command in a specific time interval. Here's the code:

```
cd "my_directory"
:start
timeout /t 60 /nobreak
pandoc --citeproc --reference-doc="template.docx" "paper.md" -s -o "paper.docx"
goto start
```

When I execute the file, it first sets my working directory and then waits 60 seconds before having Pandoc convert my file. It then jumps back to right before the conversion command and again waits 60 seconds etc. until I manually cancel it using `Ctrl+C`. This solution since it keeps my `.docx` file in sync while I'm working on the `.md` file. The loop also keeps on running if Pandoc can't overwrite the current `.docx` file because it's open. If you want to suppress the countdown that the Windows shell does by default, try putting this:

```
timeout /t 60 /nobreak > NUL
```

An unaddressed shortcoming of this solution is that the terminal has to stay open in order to keep the script running and the files synced. However, the memory that it consumes seems negligible so its not much more than a potential visual annoyance on the screen.

# Final Thoughts

This little script fixes an issue many/most people might not even encounter but for those who use or might be thinking of using Obsidian with Pandoc in a way similar to mine, this is a neat quality of life improvement that makes the academic writing experience just a little bit less tedious.

#### Software and Plugins Used

{% tabs software-and-plugins %}

{% tab software-and-plugins Software %} - Obsidian - Pandoc - Microsoft Word
{% endtab %}

{% tab software-and-plugins Plugins %} - Obsidian Pandoc Plugin - Obsidian Better Word Count Plugin
{% endtab %}

{% endtabs %}

#### Footnotes
