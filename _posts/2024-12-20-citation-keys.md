---
layout: post
title: Using Citation Keys for Personal Knowledge Management
date: 2024-12-20 20:01:00-0400
description: Citation keys can level up reference organisation and academic writing in your Second Brain
tags: Obsidian Zotero Pandoc citations academic-writing
categories: PKMS
featured: true
related_posts: false
related_publications: false
citation: false
tabs: true
toc:
  beginning: true
---

> This post details my use of citekeys in my personal knowledge management system using **Obsidian** and **Zotero**. In case you're unfamiliar with these two free programmes: [Zotero](https://www.zotero.org/) is a reference manager, and [Obsidian](https://obsidian.md/) is a tool that allows you to edit and link locally stored [Markdown](https://www.markdownguide.org/) files. For a more thorough introduction, check out [this video](https://www.youtube.com/watch?v=OUrOfIqvGS4).

# What Are Citations Keys, and Why Bother?

When I started using Obsidian as a personal knowledge management system, I soon wanted to move my academic writing there, too. The distraction-free editor and the prospect of minimising my contact with Microsoft Word were as appealing back then as they are now. Like many of Obsidian's more advanced uses, how to do this wasn't all that evident to me as someone without a very strong technical background. It only really clicked for me when I watched [this amazing tutorial](https://www.youtube.com/watch?v=J86Pm62XM_Q) which I cannot recommend enough for anyone looking to get into academic writing in Obsidian.

This post is not the place to go into depth about the writing workflow. I'll only say that much: [Pandoc](https://pandoc.org/) is a programme that converts `.md` files into a broad range of different formats, among them `.docx`, `.html.`, and `.pdf`. It comes with a feature called `citeproc` which, as the name suggests, allows processing/rendering citations in accordance with a specified citation style and based on a specified bibliography in `.bib` format.

Importantly, `citeproc` relies on a specific element of `.bib` files to select the right bibliographic information to render the citations: the so-called **citation key**. This is an identifier that is unique for each `.bib` file and can have all sorts of different formats. It sits in the beginning of each entry and allows `citeproc` to call the correct information from a bibliography.

This was the first time I have ever been in touch with citation keys, and over time I have come to use them more and more for reference management and writing in Obsidian. This post showcases these uses, and highlights the potential of citation keys that might enhance your Second Brain, too.

# Generating Citation Keys

All reference managers that come with a citation processor work based on citation keys but the exact formats differ. Some programmes even allow you to customize your citation keys, for instance [Citavi 6](https://www1.citavi.com/sub/manual6/en/index.html?cse_customizing_citation_keys.html). Zotero does not allow you to change the rules according to which citation keys are generated natively. There is, however, a plugin for that: **[BetterBibTex](https://retorque.re/zotero-better-bibtex/)**. This is a solution that adds a number of functions to Zotero (and Juris-M, another reference manager) of which two are interesting here.

First, [custom citation keys](https://retorque.re/zotero-better-bibtex/citing/index.html). Frankly, I just use the `zotero.clean` pre-set formula which effectively takes the surname of the first author, the first 'real' word of the title (which is not an article or a preposition), and the year of publication, all separated by underscores (e.g., `ahrens_how_2022`). That being said, you have a broad range of customisation options if you prefer any other format. BetterBibTex also ensures that citation keys are unique across your entire Zotero library.

Second, [advanced export features](https://retorque.re/zotero-better-bibtex/exporting/index.html). BetterBibTex makes it possible to keep exporting your library as a `.bib` file to a directory of your choice, and in intervals/upon triggers of your choice - quietly in the background. My full library gets exported every time I change something, to a folder within my Obsidian vault. That allows having a `.bib` file that contains all your references and is always up to date with your vault to use for various purposes.

What are these purposes, you ask? Keep reading...

# Using Citation Keys I: File Management

Before we move into the Obsidian vault, I'd like to highlight another plugin for Zotero which makes my life a lot easier: **[Zotfile](https://zotfile.com/)**. Storing bibliographical information in Zotero is all fine and well but you will often want a `.pdf` alongside that information. Zotfile is here to help with the management of attachments in your Zotero library. The essential functionality for me is the "Rename and move"-feature which, unsurprisingly, allows you to automatically rename an attachment to a given reference in accordance with a custom rule and to move it to a custom directory.

My custom directory is the Attachments folder within my vault. For the file names, I have set up Zotfile in a way that renames the `.pdf` attachments with the citation key of the reference that they're attached to. To achieve this, open the ZotFile preferences and go to the "Renaming Rules" tab. Once here, go to the field "Format for all Item Types except Patents" and simply put {% raw %}`{%b}`{% endraw %}.

Now, you can select any reference, right click, select the "Manage Attachments" option and click "Rename and move". The attached `.pdf` will be moved to the directory you specified and rename to the citation key. You can do this for single references but bulk renaming and moving is possible, too.

Even regardless of the renaming functionality, the bulk moving to another directory might be interesting for users with files that add up to more than the 300 MB of free storage that Zotero offers its users. Simply move your attachments to another folder that you're syncing with a cloud and you're good to go!

# Using Citation Keys II: Reference Notes in Obsidian

Let's start looking into Obsidian. Here, the key plugin that works with citation keys is called **[Obsidian Citations Plugin](https://github.com/hans/obsidian-citation-plugin/)**. Once you specify the path of your `.bib` bibliography, the plugin allows you to create notes for specific references from your Zotero. Select `Citations: Open literature note` from the command pane, and the plugin will load your bibliography and allow you to select a reference to create a note for.

In the plugin settings, you can specify the template for such a reference note. Importantly, the variables to choose from include {% raw %}`{{citekey}}`{% endraw %} which I use for the file title of the reference note: {% raw %}`@{{citekey}}`{% endraw %}. Here, it also comes in handy that I used ZotFile to rename the attached `.pdf` files to the citation key: I can exploit it for the template.

- If you want, say, to embed the `.pdf` in your reference note, simply put {% raw %}`![[{{citekey}}.pdf]]`{% endraw %} in your template.
- I'm using the **[Obsidian Annotator](https://github.com/elias-sundqvist/obsidian-annotator)** plugin to highlight my documents. This plugin requires frontmatter information which specifies the annotation target. So, I simply put {% raw %}`annotation-target:: "006 Attachments/{{citekey}}.pdf"`{% endraw %} at the top of my template and I can start annotating immediately upon the creation of my note.

The Obsidian Citations Plugin lets you specify a folder in which your reference notes will be created but just in case, you could also use **[Obsidian Auto Note Mover](https://github.com/farux/obsidian-auto-note-mover)** and add a rule that all files whose name begins with an `@` are to be moved to a given folder.

# Using Citation Keys III: Writing with Citations

I've briefly explained how to produce academic text with references in Obsidian and have them rendered with Pandoc. Here, the Obsidian Citations plugin comes in handy as well. Select `Citations: Insert Markdown citation` and the plugin will let you select a reference and subsequently put a Markdown citation, i.e., the citation key prefixed with an `@` in between square brackets.

The fact that this is also the format I have chosen for my reference notes means that whenever I open a reference note, all citations of that reference in my own writing within Obsidian will show up under the Unlinked mentions. This is a neat way of keeping track of how I've used a reference in my writing.

# Using Citation Keys IV: Bulk Processing Notes with Python

Consistently using standardised citation keys can also be a lifesaver when you're working on the `.md` files in your vault with an app other than Obsidian. This might not be the case very often but the fact that this is possible is a central pillar of why Obsidian is [considered](https://www.reddit.com/r/ObsidianMD/comments/q7aimp/how_futureproof_is_in_fact_obsidian/) a relatively future-proof way of storing your notes.

Here is a recent use-case of mine. I used to tag every note generated using the Obsidian Citations plugin with `#reference`. However, a friend of mine recently introduced me to a tagging system that was too genius not to implement it. Among other things, it entails nested tags to distinguish different types of resources (e.g., `#is/resource/book`, `#is/resource/paper` and so on). The problem: I had over 900 reference notes which I totally wasn't going to re-tag manually, and I couldn't use **[Obsidian Tag Wrangler](https://github.com/pjeby/tag-wrangler)** because `#reference` had to be split up into different tags based on the type of reference.

So, I started by sorting my Zotero library by item type, selected all my books, and exported them as a `books.bib` bibliography. Then, I asked ChatGPT to generate a Python script that would allow me to replace `#reference` with `#is/resource/book` - but only for those notes whose file title (i.e., the citation key) was contained in `books.bib`. This worked like a charm, and I repeated it for the other item types that got their own specific tag under the new system. So, yet another case where I got to exploit a structure I had long ago put in place for an entirely different purpose.

# [Bonus] Using Citation Keys V: Harmonising Citations Keys with Obsidian Web Clipper

This fall, Obsidian made its browser plugin **[Obsidian Web Clipper](https://obsidian.md/clipper)** available to all users who are now able to highlight and clip all sorts of online resources directly into their vault. I think this yet another amazing feature which the Obsidian developers have released and I can't wait to play around with it a bit more.

It does however, come with a little challenge to the citation key-based workflow. Obsidian Web Clipper cannot tap into the citation keys generated within Zotero, it only communications between my browser and my vault. Accordingly, clipping online content may create reference notes in my vault that are not named with the corresponding citation key.

So, after clipping the reference and automatically creating a reference note in my vault, I add the reference to my Zotero using the **[Zotero Connector](https://www.zotero.org/download/)** browser integration. Now, I have a reference with a citation key in Zotero, and an incorrectly named reference note in my vault.

For the next step, I wrestled with ChatGPT for quite a while until I managed to modify the Obsidian Citations plugin to add a new function that allows me to select a reference to whose citation key the active file should be renamed. I execute that command, select the correct reference, and the title generated by Obsidian Web Clipper is replaced by the citation key.

This is a homemade solution, and I am afraid I lack the technical capabilities to make a pull request to the plugin's GitHub repository for this to become an actual feature - which only adds to the hesitation I have based on the fact that AI modified the code of the plugin, not me. If you're still interested in me sharing this modified plugin with you, please [reach out](mailto:paulphilippstewens@gmail.com) to me directly.

# Final Thoughts

Over time, I have discovered an ever-wider range of opportunities to exploit my use of citation keys which I totally did not have in mind when I first started using them to name my reference notes. I think these use cases that have emerged over the years show that consistency pays off: in the sense that you never know what a given structural feature could be useful for in the future. After all, you might not have to re-invent the wheel even in times of more expansive re-structuring; finding creative uses for the structure you already have might just be enough at times.

#### Software and Plugins Used

{% tabs software %}

{% tab software Obsidian %}

    - Citations
    - Auto Note Mover
    - Annotator
    - Web Clipper (browser extension)

{% endtab %}

{% tab software Zotero %}

    - BetterBibTex
    - Zotfile
    - Connector (browser extension)

{% endtab %}

{% tab software Pandoc %}

    - citeproc filter

{% endtab %}

{% endtabs %}
