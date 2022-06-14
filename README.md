# Writedown

A lightweight markup language and toolchain for writers based on Markdown.

![Writedown](assets/WritedownLogo.png)

Language Version 1.0

## Overview

Writedown is a pure text markup language based on Markdown that lets writers define the structure of their manuscripts and create annotations to support common writing workflows.

Where standard Markdown provides an easy to use text-based syntax to represent _document structure_ (headings, emphasis, etc.), Writedown brings the ability to also represent _writing project structure_ (chapters, scenes, outlines, comments, notes, todos, etc.) across multiple files; something that previously has only been available through the use of proprietary tools and non-human readable file formats.

Writedown allows you to declare *what* you're writing *when* you're writing it, and is wholly text-based.  This makes Writedown ideal for writers seeking to use freeflow and distraction-free tools such as the Freewrite Smart Typewriter and Traveler.  Because Writedown is 100% plain text, writers can also incorporate a variety of familiar text-based tools into their workflows that binary formats make prohibitive, such as using git for version control.

In addition to the Writedown language specification, a [functional specification](#commands) for supporting toolchains is also provided.  This helps to ensure that writers sharing their Writedown files with others (such as reviewers, editors, and publishers) can count on a common set of functionality to make their projects readable and usable by others regardless of the exact Writedown tools they are using.

## Getting Started

To start using Writedown in your projects, you only need two things:
1. **A plain text editor** to write your Writedown files in.  (Any will do, including Notepad, so long as you can save your files in plain text.)
2. **A Writedown parser** to process your files.  (See [Writedown Tools](#writedown-tools) for links to available tools.)

Once you have those things in order, you're ready to start writing with Writedown!

## Basic Syntax

Writedown syntax is simple yet powerful, with one of its goals being that it is human readable.

All Writedown follows the form of a Writedown instruction on a new line, followed by whatever arguments are used by that instruction.  All Writedown instructions start with an at-sign (@).

For example, to specify the start of a chapter called "The Beginning" in Writedown, you might write:

```
@chapter The Beginning
```

In the example above, **@chapter** is the Writedown instruction for a chapter, and "The Beginning" is an argument that gives the name of the chapter.

Using Writedown instructions, we can easily represent the structure of a writing project, such as a basic novel:

```
@title My Novel
@author My Name
@chapter
@scene The Big Opening
Jack Writer sat at the coffee shop table, unable to find the words to express lorem ipsum dolor sit amet...
```

We can even use instructions that support our own unique writing workflows, with support for things like comments, todos, and wordcount targets (to name a few):

```
@# This is a comment

@todo Don't forget to do this thing

@target 1000
```

And because Writedown is based on Markdown, you can use it anywhere in your manuscript text:

```
@scene The Thought
*I can't do this* Jack thought to himself as he tried to balance his laptop and latte in one arm.
```

To see all of the great things Writedown can represent with its instructions, check out the [Cheat Sheet](#cheat-sheet).

## File Structure

Writedown file structure is similarly simple and powerful, with the intent of also being human-readable.

All Writedown files are simple text files with a `.wd` file extension.

You can have as many or as few Writedown files as you'd like for your project, and you can structure them in whatever way makes the most sense for you and your workflow.

The simplest Writedown project can consist of one file, and can be named whatever you like with a `.wd` extension:

```
my-novel.wd
```

But often, working with one large file for a something the length of a novel can be frustrating.  And that's where Writedown's file structure really shines.

Writedown was intentionally designed to let writers split up their projects into whatever file structure makes the most sense for them, their project, and their workflow, using basic conventions to help facilitate this.

In essence, the combination of the Writedown files in your project make up your *Document*.

By default, Writedown will look for files in the target directory based on the glob path `**/*.wd`.  Files are read and processed in alphanumerical order:

```
intro.wd    <-- First file read
part1/
-- ch1.wd   <-- Second file read
-- ch2.wd   <-- Third file read
-- ch3.txt  <-- Would NOT be read (doesn't end in .wd)
part2/
-- ch1.wd   <-- Fourth file read
-- ch2.wd   <-- Fifth file read
```

This approach may be suitable for smaller projects and projects that follow a predictable file naming convention.  In cases where more control is required over which files are included and when, an **index.wd** file may be specified.

If an **index.wd** file is found in the target directory, it - *and only it* - will be read and processed:

```
index.wd    <-- Entry point (the only file that is implicitly read)
intro.wd    <-- Only read if included in index.wd
part1/
-- ch1.wd   <-- Only read if included in index.wd
-- ch2.wd   <-- Only read if included in index.wd
part2/
-- ch1.wd   <-- Only read if included in index.wd
-- ch2.wd   <-- Only read if included in index.wd
```

In order to include (and control the order of inclusion) of other files, you can use the `@include` instruction:

```
index.wd
--------
@include intro.wd
@include part1/ch1.wd  <-- Explicitly includes chapters in this part
@include part1/ch2.wd
@include part2/*.wd    <-- Includes chapters based on a glob pattern
```

There is nothing special about an **index.wd** file other than its name and the effect it has on Writedown's file discovery process.  Any Writedown instruction may be used in it.

The above file and directory names and structures are simply examples for demonstration purposes.  You can name and structure your files and directories however you see fit:

```
in-the-woods.wd

chapter_01/
-- in-the-woods.wd

chapter1/
-- scenes/
---- in-the-woods.wd

ch1/
-- scene1.wd
```

The only restriction you have in how you structure your files is based on the *scope* of the instructions you use.  A number of Writedown instructions can span multiple files (such as `@chapter`) so that larger parts of a project can be broken up.  Other Writedown instructions (such as `@scene`) are intended to be contained within one file.  The [Cheat Sheet](#cheat-sheet) includes descriptions of the behavior of each instruction.

## Output and Processing

Writedown requires the use of a Writedown parser in order to produce output and perform other processing on your projects.

With a Writedown parser, you can output your projects in various file formats and layouts, such as PDF.

Writedown parsers also give you the ability to inspect different aspects about your project, such as viewing todos, seeing everywhere a location or character occurs in your manuscript, and even tracking target wordcounts against actual wordcounts.

The minimal set of commands supported by Writedown parsers is available in the [Commands](#commands) section of this document.

To view a list of available Writedown parsers, see the [Writedown Tools](#writedown-tools) section.

When it comes to preparing your document for output, it is important to note that:
1. **The order of Writedown instructions matters.**  Instructions are emitted as they are encountered.
2. **Writedown respects whitespace and newlines.**  If you include a blank line in between Writedown instructions, that blank line will be included in your output.
3. **Prose-style paragraphs should start on a newline with a leading tab.**  Writedown will automatically indent lines starting with a tab appropriately.  Double-spacing (including a blank line) between paragraphs will result in double-spacing in your output, and is discouraged.

## Tutorials

Short tutorial examples can be found in the [tutorials](tutorials/) directory of this project.

## Templates

Templates for common writing project formats are provided in the [templates](templates/) directory of this project.

## Examples

Full book-length examples of Writedown projects based on Project Gutenberg texts are provided in the [examples/gutenberg](examples/gutenberg/) directory of this project for reference.

## Sample Output

Samples of Writedown generated output are in the [samples](samples/) directory of this project.

## Cheat Sheet

Writedown supports the following markup instructions in addition to any standard Markdown syntax:

| Instruction | Arguments | Description | Example Usage | Processed for Output |
| ----------- | --------- | ----------- | ------------- | -------------------- |
| **@act** \[number\] \[title\] | <div>**number** (optional) - The Act number (e.g., 1, 2, 3).  Must be a numeric value.</div><div>**title** (optional) - The Act title (e.g., "The Beginning").  The title should not start with the word "Act"; for example, "Act One" should simply be "One".</div> | Defines an Act.  All subsequent lines, up to the next **@act** instruction, are considered to belong to this Act. | <div>`@act`</div><div>`@act 1`</div><div>`@act The Beginning`</div><div>`@act 1 The Beginning`</div> | Yes |
| **@author** \<name\> | **name** (required) - The name of the Author of the work. | Defines the Author of the work. | `@author Jack Writer` | Yes |
| **@chapter** \[number\] \[title\] | <div>**number** (optional) - The Chapter number (e.g., 1, 2, 3).  Must be a numeric value.</div><div>**title** (optional) - The Chapter title (e.g., "The Beginning").  The title should not start with the word "Chapter"; for example, "Chapter One" should simply be "One".</div> | Defines a Chapter.  All subsequent lines, up to the next **@chapter**, **@part**, or **@act** instruction, are considered to belong to this Chapter. | <div>`@chapter`</div><div>`@chapter 1`</div><div>`@chapter The Beginning`</div><div>`@chapter 1 The Beginning`</div> | Yes |
| <div>**@character** \<name\> \[, alternate names\]</div><div>[notes]</div> | <div>**name** (required) - The main name of the Character.</div><div>**alternate names** (optional) - A comma-separated list of alternate names for the Character.</div><div>**notes** (optional) - Free-form notes about the Character; can be multi-line.  Must start on a newline after the instruction.</div> | Defines a Character.  The primary name should occur first, followed by a comma-separated list of alternate names.  Characters frequently referenced by their first name in the manuscript should contain separate entries for their first and full names.  Additional nicknames and alternate names may be additionally specified.  Any notes about the character (background, description, motivation, etc.) may be provided starting on a new line after the **@character** instruction. | <div>`@character Jack`</div><div>`@character Jack Writer`</div><div>`@character Jack, Jack Writer, Jack-o, Writer`</div><div>`@character Jack`<div>`Notes about Jack`</div></div> | No |
| <div>**@comment** \<comment\></div><div>**@\#** \<comment\></div><div>**@\*** \<comment\> **\*@**</div> | **comment** (required) - The comment. | Specifies a comment.  The **@comment** and (shorthand) **@\#** instructions support single-line comments, whereas the **@\*** ... **\*@** instruction supports multi-line comments.  The multi-line form can also be used to "comment out" a block of text from appearing in output, similar to commenting out code in programming. | <div>`@comment This section reveals his motive`</div><div>`@# This section reveals his motive`</div><div>`@* This section reveals his motive *@`</div> | No |
| **@eof** | None | **Reserved (Do Not Use).**  Generated by parser.  Denotes the end of a file.  Used to assist in processing instructions dependent on file boundaries. | | No |
| **@endsession** | None | Explicitly ends a Session. | `@endsession` | No |
| **@include** \<path\> | **path** (required) - The path relative to the current file to include.  Can be a file name, a directory, or a glob pattern (e.g., "dir/\*\*/\*.wd").  When a directory is provided, the same rules Writedown uses for determining an entry point are applied. | Include Writedown files for processing from the provided path. | <div>`@include file.wd`<div><div>`@include dir`</div><div>`@include dir/**/*.wd`</div> | Processed, but how output is affected depends on processed contents |
| **@location** \<name\> \[, geographic paths\] | <div>**name** (required) - The name of the Location.</div><div>**geographic paths** (optional) - A comma-separated list of additional geographic paths for the Location.</div> | Denotes that the following text (in the context of the current structure) occurs at the specified Location.  The specific (primary) name should occur first (e.g., "Coffee Shop") followed by an optional comma-separated list of geographic paths that give context to the Location (e.g., "Cityville, Countria").  The primary name is used to map to a corresponding **@place** if one is defined.  Useful for tracking the use of Locations throughout a manuscript. | <div>`@location Coffee Shop`</div><div>`@location Coffee Shop, Cityville`</div><div>`@location Coffee Shop, Cityville, Countria`</div> | No |
| **@pagebreak** | | Specifies where an explicit pagebreak should occur. | `@pagebreak` | Yes |
| **@part** \[number\] \[title\] | <div>**number** (optional) - The Part number (e.g., 1, 2, 3).  Must be a numeric value.</div><div>**title** (optional) - The Part title (e.g., "The Beginning").  The title should not start with the word "Part"; for example, "Part One" should simply be "One".</div> | Defines a Part.  All subsequent lines, up to the next **@part** or **@act** instruction, are considered to belong to this Part. | <div>`@part`</div><div>`@part 1`</div><div>`@part The Beginning`</div><div>`@part 1 The Beginning`</div> | Yes |
| <div>**@place** \<name\> \[, geographic paths\]</div><div>[notes]</div> | <div>**name** (required) - The name of the Place.</div><div>**geographic paths** (optional) - A comma-separated list of additional geographic paths for the Place.</div><div>**notes** (optional) - Free-form notes about the Place; can be multi-line.  Must start on a newline after the instruction.</div> | <div>Defines a Place.  The specific (primary) name should occur first (e.g., "Coffee Shop") followed by an optional comma-separated list of geographic paths that give context to the Place (e.g., "Cityville, Countria").  The primary name is used to cross reference matching **@location** instructions.  Any notes about the place (description, history, etc.) may be provided starting on a new line after the **@place** instruction.</div><div>*(If you are looking to specify the location of parts in a manuscript, use **@location** instead.  **@place** is used to define a Place that is referenced by locations.)*</div> | <div>`@place Coffee Shop`</div><div>`@place Coffee Shop, Cityville`</div><div><div>`@place Coffee Shop, Cityville, Countria`</div><div>`Notes about the place`</div></div> | No |
| **@scene** \[number\] \[title\] | <div>**number** (optional) - The Scene number (e.g., 1, 2, 3).  Must be a numeric value.</div><div>**title** (optional) - The Scene title (e.g., "The Beginning").  The title should not start with the word "Scene"; for example, "Scene One" should simply be "One".</div> | Defines a Scene.  All subsequent lines, up to the next **@scene**, **@chapter**, **@part**, or **@act** instruction, are considered to belong to this Scene. | <div>`@scene`</div><div>`@scene 1`</div><div>`@scene The Beginning`</div><div>`@scene 1 The Beginning`</div> | Only if belongs directly to an **@act** instruction.  This allows prose writers define and track their scenes without affecting their output. |
| **@section** \[number\] \[title\] | <div>**number** (optional) - The Section number.  Section numbers can be numeric values or numerical outline values (numeric values separated by a period).  For example, "1" and "1.1" are both valid Section numbers.</div><div>**title** (optional) - The Section title (e.g., "Punctuation in Practice").  The title should not start with the word "Section"; for example, "Section One" should simply be "One".</div> | Defines a Section.  All subsequent lines, up to the next **@section**, **@chapter**, **@part**, or **@act** instruction, are considered to belong to this Section.  Sections with a numerical outline value **number** will nest below their numerically equivalent parent (e.g., Section "1.1" will nest under Section "1"). | <div>`@section`</div><div>`@section 1`</div><div>`@section 1.1`</div><div>`@section Punctuation in Practice`</div><div>`@section 1 Punctuation in Practice`</div><div>`@section 1.1 The Oxford Comma`</div> | Yes |
| **@session** \<date\> \[target\] \[name\] | <div>**date** (optional if name is provided) - The Session date.  Must be in loose mm/dd/yyyy format (e.g., 1/15/2022).  If a date is provided that does not match the format, it will be left preserved as part of the **name**.</div><div>**target** (optional) - The target wordcount for the Session.</div><div>**name** (optional if date is provided) - The Session name (e.g., "At the coffee shop").  Can be used to provide meaningful context to the session. | Defines a Session.  All subsequent lines, up to the next **@session** or **@endsession** instruction, or until the end of the file (whichever occurs first), are considered to belong to this Session.  Useful for associating work performed in a manuscript with writing sessions. | <div>`@session 1/15/2022`</div><div>`@session 1/15/2022 1000`</div><div>`@session 1/15/2022 At the coffee shop`</div><div>`@session 1/15/2022 1000 At the coffee shop`</div><div>`@session At the coffee shop`</div> | No |
| **@status** \<new \| draft \| revision \| done\> | Choice: **new**, **draft**, **revision**, or **done** (required) - The state of the status. | Denotes the status of the nearest structural parent (**@scene**, **@section**, **@chapter**, **@part**, or **@act**).  Useful for tracking the status of areas throughout a manuscript. | `@status draft` | No |
| <div>**@tableofcontents**</div><div>**@toc**</div> | | Specifies where an automatically generated table of contents should occur. | <div>`@tableofcontents`</div><div>`@toc`</div> | Yes |
| **@tag** <tag ...> | **tag** (required) - A space-separated list of tags. | Associates one or more tags with the nearest structural parent (**@scene**, **@section**, **@chapter**, **@part**, or **@act**).  Useful for tracking user-defined information and metadata. | <div>`@tag fight-scene`</div><div>`@tag foreshadow romance`</div> | No |
| **@target** \<wordcount\> | **wordcount** (required) - The wordcount to target.  Must be a numeric value. | Defines a target wordcount for the nearest structural parent (**@scene**, **@section**, **@chapter**, **@part**, or **@act**) or nearest **@session** parent (whichever is closest). | `@target 1000` | No |
| **@title** \<title\> | **title** (required) - The title of the work. | Defines the Title of the work.  This instruction should appear only once in any manuscript. | `@title Adventures in Writing:  A Memoir` | Yes |
| **@todo** \<text\> | **text** (required) - The text of the todo. | Defines a todo.  Definitions are preserved in-place in a document, giving context to the todo itself.  Todos are also associated with their nearest structural parent (**@scene**, **@section**, **@chapter**, **@part**, or **@act**) for easy summarization.  Useful for keeping track of areas that require completion or follow up. | `@todo Make sure this lines up with his backstory` | No |
| **@writedown** \<version\> | **version** (required) - The Writedown version. |  Defines the Writedown version the document targets. This instruction should appear only once in any manuscript. | `@writedown 1.0` | No |

## Commands

Writedown tools intended for use by end users should conform to the following minimal functional specification in order to provide end users with a consistent set of base functionality.

This functional specification is concerned about ensuring that the *functionality* described is supported - not necessarily *how* that functionality is implemented or presented.  This leaves room for tool authors to choose approaches that make the most sense for their applications and to bring their own inventive perspectives to the table.

Additional functionality not covered by this specification may of course be included in Writedown tools.

The Writedown functional specification describes the following "commands" that should be implemented in whatever form makes sense for the tool:  For example, as console application commands, graphical user interface toolbars and menus, or even as actions in a smartphone application.  The only thing that matters is that users of a Writedown tool should be able to perform the following functions in some way, and that the output should be relatively representative of the samples provided.

A sample console application is presented for reference as a use case.

### info

The **info** command shows basic structural information about a Writedown document, including:
1. The Title
2. The Author
3. The number and types of structural nodes (Act, Part, Chapter, Scene, Section)
4. The number and types of definition nodes (Character, Place)
5. The number and types of informational nodes (Location, Tag, Todo, Comment, Note)

Sample usage:
```
writedown> info
```
```
A Work In Progress
by Jack Writer
1 part
3 chapters
2 scenes
2 locations
1 character
1 place
2 tags
3 todos
1 comment
```

### wordcount, wc

The **wordcount** command (shortcut **wc**) shows the number of pages, words, and characters in a document at its structural levels, including the amount of time it would take to read each structural level.

Sample usage:
```
writedown> wordcount
writedown> wc
```
```
                                                             reading time      pages      words      chars
A Work In Progress                                                0:01:11          1        326       2072
-- Part 1                                                         0:01:11          1        326       2072
---- Chapter 1: Writer's Block                                    0:01:11          1        326       2072
------ Scene 1: Realizations at the coffee shop                   0:00:52          1        239       1516
------ Scene 2: Escape through the parking lot                    0:00:18          0         87        556
---- Chapter 2                                                    0:00:00          0          0          0
---- Chapter 3                                                    0:00:00          0          0          0
```

### characters

The **characters** command shows which characters appear at the different structural levels of a document, and how many times each character is referenced.  For character usage to be recognized, a character must first be defined by a `@character` instruction.

Sample Usage:
```
writedown> characters
```
```
                                                             characters
A Work In Progress                                           
-- Part 1                                                    
---- Chapter 1: Writer's Block                               
------ Scene 1: Realizations at the coffee shop              Jack (2)
------ Scene 2: Escape through the parking lot               Jack (1)
---- Chapter 2                                               
---- Chapter 3                        
```

### locations

The **locations** command shows which locations appear at the different structural levels of a document.  For location usage to be recognized, a location must specified by a `@location` instruction, and optionally defined by a corresponding `@place` instruction.

Sample Usage:
```
writedown> locations
```
```
                                                             locations
A Work In Progress                                           
-- Part 1                                                    
---- Chapter 1: Writer's Block                               
------ Scene 1: Realizations at the coffee shop              Farbucks
------ Scene 2: Escape through the parking lot               Farbucks Parking Lot
---- Chapter 2                                               
---- Chapter 3                    
```

### status

The **status** command shows the status of structural levels in a document.  Statuses are indicated with the `@status` instruction.

Sample Usage:
```
writedown> status
```
```
                                                             status    
A Work In Progress                                                     
-- Part 1                                                              
---- Chapter 1: Writer's Block                               draft     
------ Scene 1: Realizations at the coffee shop              draft     
------ Scene 2: Escape through the parking lot                         
---- Chapter 2                                               new       
---- Chapter 3    
```

### tags

The **tags** command shows the tags associated with structural levels in a document.  Tags are associated using the `@tag` instruction.

Sample Usage:
```
writedown> tags
```
```
                                                             tags
A Work In Progress                                           
-- Part 1                                                    
---- Chapter 1: Writer's Block                               
------ Scene 1: Realizations at the coffee shop              awesome, good
------ Scene 2: Escape through the parking lot               
---- Chapter 2                                               awesome, stuck
---- Chapter 3                                               
```

### todo

The **todo** command shows todos defined in the document.  Todos are defined with the `@todo` instruction.

Sample Usage:
```
writedown> todo
```
```
A Work In Progress                                          
-- Part 1                                                   
---- Chapter 1: Writer's Block                              
------ Scene 1: Realizations at the coffee shop             
-------- tests/books/wip.wd:16           [TODO] What should we name the barista?
------ Scene 2: Escape through the parking lot              
-------- tests/books/wip.wd:26           [TODO] Should this take place in the parking lot or the bathroom?  How does he get the key?
---- Chapter 2                                              
------ tests/books/wip.wd:32             [TODO] Left off here
```

### targets

The **targets** command shows wordcount targets defined in the document using the `@target` instruction.  It shows the target wordcount, the actual wordcount, and the delta of actual minus target for each structural level of the document.

Sample Usage:
```
writedown> targets
```
```
                                                                 target     actual      delta
A Work In Progress                                                    -        326          -
-- Part 1                                                             -        326          -
---- Chapter 1: Writer's Block                                      326        326          0
------ Scene 1: Realizations at the coffee shop                       -        239          -
------ Scene 2: Escape through the parking lot                        -         87          -
---- Chapter 2                                                     2000          0      -2000
---- Chapter 3                                                        -          0          -
```

### sessions

The **session** command shows sessions defined in the document using the `@session` instruction.  For each session, the following information is displayed:
1. The location of the session definition
2. The date of the session
3. The title of the session
4. The target wordcount of the session
5. The actual wordcount of the session
6. The delta wordcount (actual minus target) of the session

Sample Usage:
```
writedown> sessions
```
```
                                                                                            target     actual      delta
tests/books/wip.wd:13                    05/10/2022 At the coffee shop, hahaha                 200        239        +39
tests/books/wip.wd:24                    05/11/2022                                            100         87        -13
tests/books/wip.wd:36                                                                         1000          0      -1000
```

### export

The **export** command enables a user to export a Writedown document to various output formats.  It is up to the tool to determine the export formats it will support and the flags and options each export format takes.

Sample Usage:
```
writedown> export draft
writedown> export text
writedown> export pdf
writedown> export strip
writedown> export dump
```

## Writedown Tools

Coming Soon!

## Credits

Writedown was created by Clifford Oravec.

## License

Writedown is licensed under the terms of the MIT license.