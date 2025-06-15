---
layout: post
title: A live preview mode for editing citation styles
date: 2025-06-09
description: Here's how to edit a citation style (CSL) with a live preview using pandoc and VS Code.
tags: academic-writing Pandoc Zotero
keywords: academic-writing, obsidian, pandoc, conversion, markdown, csl, citation-style, vs-code, bibliography, citations, referencing- live-server, live-preview
categories: PKMS
og_image: assets/img/publication-preview/book-wall.jpg
featured: false
related_posts: false
related_publications: false
citation: false
tabs: false
toc:
  sidebar: left
---

It's 2025, and we still haven't moved any closer to a [harmonisation of citation styles](https://blogs.bmj.com/bmj/2015/07/29/deepak-balak-and-enes-hajdarbegovic-towards-harmonisation-of-referencing-styles/) for academic writing. Almost every journal insists on its own, unique style guide; [conferences](https://media.icml.cc/Conferences/ICML2025/Styles/example_paper.pdf) often have their own citation styles, as do [some universities](https://libguides.graduateinstitute.ch/legal_citation/international-law-department-style). Differences might be subtle, but large enough to prevent academics from using, for example, one of the over 10,000 styles which are available from the [Zotero Styles Repository](https://www.zotero.org/styles). Submitting a piece of writing to more than one outlet will often require painstaking manual adjustments to the manuscript, or at least to the citation style. But what if this process could be just a bit less annoying? I this post I will explain **how to edit a citation style in `.csl` format in a live preview mode**.

## The Basics
Let's start with a bit of background. If you're not doing your citations by hand (which I hope and strongly encourage), you're probably using a piece of software. This could be Endnote, Mendeley, Citavi, Zotero, you name it. If we break the automatic generation of references down just a bit further, we can distinguish three different components.

1. **The bibliography file**. This is a file with the extension `.bib` which stores all the bibliographical information of the titles in your reference manager. When you create a new title and fill in all the fields, the reference manager stores this information in a certain format, the `.bib` format. It can be written and read by all reference managers and citation processors. In the `.bib` file, every title in the bibliography gets its unique identifier, the so-called citation key.
2. **The citation style**. This is a file which contains the rules according to which the bibliographical information is transformed into actual citations. For example, if the bibliography file contains the first and last name of the author, the citation style determines whether the first name is abbreviated, whether the name is put in small caps, italicised, and so on. Some reference managers use closed, proprietary formats for their citations styles. But: there is also an open file format, the `.csl` format ([Citation Style Language](https://docs.citationstyles.org/)) which can be used and edited by anyone. 
3. **The citation processor**: This is the programme which generates the citations. To do so, it relies on the first two components. It reads the bibliographical information from the `.bib` file and transforms it using the rules from the `.csl` file. The output can vary. Most often, this will be text in a Microsoft Word document (created by the add-in of a reference manager) but programmes like [pandoc](https://pandoc.org/) can also generate these citation in a `.pdf` or `.html` file. 

If you're using a citation programme, these three components are not really visible because it all happens 'under the hood' of one and the same software. But: If you want, you can be in charge of its different components, and make sure they meet your specific needs.

## What You'll Need
For your convenience, I'll put the list first and the explanation second.

- [ ] pandoc (installed on your device)
- [ ] VS Code with the [Live Server extension](https://github.com/ritwickdey/vscode-live-server) (installed on your device)
- [ ] [Node.js](https://nodejs.org/) (Long Term Support version, installed on your device)
- [ ] a Markdown file (`.md`) with a manuscript that contains citations in this format: `[@citationkey]`
- [ ] a bibliography file (`.bib`)
- [ ] a citation style file (`.csl`) - can be downloaded from the [Zotero Styles Repository](https://www.zotero.org/styles) 

What we'll be doing, in essence, is convert our Markdown manuscript to HTML using pandoc, and then display this HTML file using the Live Server extension. Also, we will create an automation which executes the pandoc conversion command every time we make a change to either the bibliography or the citation style. The current version of our manuscript with the rendered citations will thus be displayed live. 

## Detailed Explanation
### Step 1: Set up your workspace
1. Create a folder titled `citation-styles`.
2. Save the citation style you'd like to modify in this folder.
3. Locate the paths to your `.md` and `.bib` files. These can be within the `citation-styles` folder but don't have to be.
4. Open the folder in VS Code.

### Step 2: Create the conversion command
1. Create a file named `convert.bat`.[^1]
2. Paste the following code:

```
@echo off

pandoc --citeproc -s "your-manuscript.md" -o "csl-test.html"
```

{:start="3"}
3. Add the filename of your manuscript. If it is not located within the `citation-styles` folder, you will need to paste the full path to the file.

[^1]: **NB**: These instructions are designed to be used on devices that run Windows. 

### Step 3: Set up a file watcher
1. Install `nodemon`. To do that, open a terminal and execute the following code through the CLI:

```
npm install -g nodemon
```

{:start="2"}
2. Run the file watcher. To do that, open `citation-styles` in a terminal and run the following code:

```
nodemon --watch your-style.csl --watch your-library.bib --exec "./convert.bat"
```

{:start="3"}
3. Add the filenames of your citation style and bibliography. If these are not located within the `citation-styles` folder, you will again need to paste the full path to the file.

If everything goes well, this sets up a file watcher that will notice every change you make (and save) to the `.csl` and `.bib` files and execute the conversion command. This will generate the most up-to-date version of your manuscript in `.html` format. If you want the file watcher to cover your manuscript as well, simply add `--watch your-manuscript.md` to the file watcher command.

### Step 4: Launch live preview
1. Enable the Live Server extension in VS Code.
2. Right-click on `csl-test.html` and select `Open with Live Server`. 
3. You should now be able to see the most up-to-date version of your manuscript in your browser; any change to the citation style (or bibliography) will be reflected here once you save it.
4. Start editing the `.csl` file.

### Step 5 [BONUS]: Automation
If you modify citation styles on a regular basis, you might not want to manually paste and execute the file watcher command over and over again. To avoid that, we can save this as a command in VS Code.

1. Go to the top of the window, select the three dots, then `Terminal`, then `Configure Tasks...`.
2. Then, select `Create tasks.json from template`. 
3. Select `MSBuild`. This will create an invisible folder titled `.vscode` and a file with the name `tasks.json`. Replace the entire content of that file with the following:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "CSL Preview",
      "type": "shell",
      "command": "nodemon --watch your-style.csl --watch your-library.bib --exec \"./convert.bat\"",
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "presentation": {
        "reveal": "always"
      },
      "problemMatcher": []
    }
  ]
}
```

{:start="4"}
4. Now, you can press `Ctrl+Shift+B` to run the file watcher any time you have the `citation-styles` folder open in VS Code.

## Concluding Thoughts
This little setup replicates the live previews which some proprietary reference managers offer. In a different live I used to work with Citavi and spent a lot of time modifying citation styles using their visual editor. It displayed the different pieces of bibliographical information as blocks that you could edit and move around. As you did your modifications, the editor would display a live preview of your references list in the current version of the citation style.

I really liked this feature, and wanted to replicate at least part of it. This replication makes it available outside of proprietary referencing software, by using open formats and software. Admittedly, Zotero also offers a [visual editor](https://editor.citationstyles.org/) for citation styles but I've found it extremely hard to use. Ironically, even working directly with the XML code turned out to be more intuitive. 

Be that as it may, is this setup as convenient as the proprietary citation style editors? Certainly not. But: it puts you in charge to a large extent, and it helps you create a citation style that you can freely share with colleagues, collaborators, students, you name it. And I don't know about you, but I'm willing to give up a bit of comfort for that.

> ##### **AI Disclaimer**
> 
> I used [Le Chat](https://chat.mistral.ai/), a European AI chatbot developed by Mistral, to build this tool. IN all honesty: I wouldn't have been capable to come up with all of it myself. Nevertheless, I do, fundamentally at least, understand the setup and its different components. I have not used generative AI to write this post. If you're interested in replication, here is my **initial prompt**: 
> 
> "Pretend you're a programmer. I need help with the following issue. I'm working on a citation style in .csl format in VS Code. I want to use the .csl style to convert a .md file to .docx using pandoc. My problem is that every time I make changes to the .csl file, I need to to execute a new pandoc command, open the .docx file again, and check whether my citations are accurately rendered. I would like to have a live preview (in HTML) of how my my citations are rendered under the current version of the citation style in VS Code. The output should be HTML and change every time I make changes to the .csl file and save them. Brainstorm ideas how this could be achieved."

---