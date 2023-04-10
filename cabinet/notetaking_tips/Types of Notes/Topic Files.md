# Topic Files
#educators see also: [[Benchmark Files]], [[Exercise Files]], [[Course Outlines]]

Topic files focus on a contained subject. They at least provide a description, as well as links to their subtopics, their parent topic, and the highest level prerequisites. These kinds of files mostly serve to represent the subject outside of any particular context.

For example, an art teacher may have a topic note on `perspective`, which links to topics like `vanishing points`, `station point`, `picture planes`, `measuring points`, `celestial sphere`, etc. as its subtopics, and `constructive drawing` as its parent topic.

(Of course, that could be broken down further, like having many of those topics inside `mechanical perspective` as a subtopic of  `perspective`.)

Topic notes help us get a sense of how different topics relate, and are a chance for us to archive all the topics we're familiar with in our discipline. They are most useful when building courses and tutorials, to get the relevant topics in one spot and ensure all the necessary bases are being covered.

We can drag all of these notes into a canvas and organize them -- with immediate access to their contents, including our prerequisites etc -- when we're ready to start brainstorming specific courses and tutorials.

>[!note] Creating topic files is also helpful when studying, especially as a companion tool to the Feynman Technique.

I recommend using Dataview inline keys when linking to the parent and prerequisites, like `parent:: [[whatever]]` or `prerequisite:: [[whatever]]`, both to distinguish them and to help build some automation. It helps to make descriptions metadata in general.

For example, you can create an index that has all the prerequisites of a topic you want to base your course around, including all the prerequisites of its prerequisites of its prerequisites... Again, that then becomes an easy pool for creating canvas files, as you can just drag links from the table into the canvas when you're planning out content.

A topic note's structure may be something like this:

```
# My Topic Name
#topics parent:: [[the immediate parent file of this note]]
description:: A semi-brief description of the topic should go here. 

## Prerequisites
* prerequisite:: [[something students need to grasp before understanding this]]
* prerequisite:: [[another thing students need to graps before...]]

## Subtopics

[[Some other topic]] is like the x of this topic, which is closely related to [[other thing]]. It's also important that students learn about [[this thing]].

```

## Automation

One discipline may have thousands of topics that need to be covered, so it's especially helpful to have some kind of automation build into your templates for this note type. You want to be able to get your initial topic files setup very quickly.

### Parent-First Approach

One way we can achieve that is with the community plugin Templater, by having it give us prompts and suggestions as the note is being setup.

This example asks for the note's name, creates a new note with that as the title, and adds it as the first header. It then asks for a description of the topic, and lets you input some subtopics.

I've (obviously) used some JavaScript to make this more thorough:

```
# <%* 
    var title = await tp.system.input("What is the title of this file?", true);
    tp.file.create("your/path/" + title); 
    tR += title; 
  -%>
#topics parent:: 
description:: <% await tp.system.input("Please provide a description of this topic.", false) %>

## Prerequisites:
* 

## Subtopics:
<%*
  var keepAsking = true;
  while (keepAsking == true) {
    var subtopics = 
      "* [[" + await tp.system.input("What are some subtopics to this?", true);
    var keepAsking = 
      tp.system.suggester(
        ["yes","no"],
        [true, false],
        true,
        "Would you like to add another subtopic?");
    subtopics += "]]";
    if (keepAsking == true) {
      subtopics += "\n";
    }
  }
  tR += subtopics; -%>
```

You can assign a hotkey to this template, so you only have to hit a hotkey to create a new note that prompts you with all you need to fill out a topic. 

You'll probably also want a version of it without the title prompt or `tp.file.create`:

```
# <% tp.file.title %>
#topics parent:: 
description:: <% await tp.system.input("Please provide a description of this topic.", false) %>

## Prerequisites:
* 

## Subtopics:
<%*
  var keepAsking = true;
  while (keepAsking == true) {
    var subtopics = 
      "* [[" + await tp.system.input("What are some subtopics to this?", false);
    var keepAsking = 
      tp.system.suggester(
        ["yes","no"],
        [true, false], 
        true,
        "Would you like to add another subtopic?", true);
    subtopics += "]]";
    if (keepAsking == true) {
      subtopics += "\n";
    }
  }
  tR += subtopics; -%>
```

By assigning that version to the folder your topic notes are in, and setting the location for new files to the folder of the current file, you can create new topic notes with all that automation just by clicking on a link in your subtopic section. 

(You can open links without a mouse by moving there with arrow keys and hitting `ctrl`+`enter`, by the way!)

### Child-First Approach
The previously discussed approach requires a fair bit of manual maintenance after its setup. To minimize that, we can use the community plugin Dataview.

Instead of linking our subtopics directly, we'll collect them from any file listing this one as its `parent` key, having a table like this in subtopics. We'll also make our description a key in the template 

    ```dataview
    TABLE description FROM #topics WHERE contains(parent, this.file.link)
    ```

We'll still use Templater to automate other aspects of our template.

We'll also use a combination of Dataview and Templater to prompt for a `parent` value from the vault's existing topic files.

Creating a new topic from a hotkey:

```
# <%* 
	var title = await tp.system.input("What is the title of this file?", true);
	tp.file.create("your/path/" + title); 
	tR += title; 
  -%>
#topics parent::
description:: <% await tp.system.input("Please provide a description of this topic.", false) %>

## Prerequisites:
*   
## Subtopics:
    ```dataview
    TABLE description FROM #topics WHERE contains(parent, this.file.link)
    ```
```
Creating a new topic from a link:

```
# <% tp.file.title %>
#topics parent:: 
description:: <% await tp.system.input("Please provide a description of this topic.", false) %>

## Prerequisites:
*   

## Subtopics:
     ```dataview
     TABLE description FROM #topics WHERE contains(parent, this.file.link)
     ```
```

>[!note] You need to un-indent the Dataview tables here. I only have them indented so they don't break the code blocks around them.