# Index Headers

An index header is just a generic Dataview or native query that lists any [[Index Files]] that link to, tag, or mention the ID of your note. 

I keep a list like this near the top of all of my notes. It allows me to condense lots of lists of related links into just one list, so I don't have to scroll miles when I don't need that information.

## Example

For example, you might have the character file `Bob`. You want to be able to get to anything about Bob from the Bob note. So you want to link to all his relationships with other characters, scenes he's in, places he's lived, objects he's owned, art you've drawn of him, reference images of him, playlists that help you get in his mindset....

That would quickly clutter up a note.

But we can make each of those lists a separate note, and then link them from the index header, giving us something like:

```
## Indexes
* [[Bobs Relationships]]
* [[Scenes With Bob]]
* [[Bobs Places of Residence]]
* [[Bobs Inventory]]
* [[Art of Bob]]
* [[Bobs Playlists]]
```

If we have some system for identifying file types (like by tag) and identifying files (like by live link or id), we can even automate this with queries or Dataview.

## Demonstration

To demonstrate that kind of automation, I made some fake index files with the #demo_index tag in `utilities/demonstrations/Index Headers`, and linked them to this file. 

### Core Solution

Here, I'll use a default query block to collect every index that links to this file as its source:

    ```query
    tag:demo_index "source:: [[Index Headers]]"
    ```

```query
tag:demo_index "source:: [[Index Headers]]"
```

### Community Solution

Of course, Dataview lists are a lot less bulky.

You're also able to access metadata within Dataview. So you don't have to worry about title changes breaking the link you're looking for, and you can query based on other metadata keys with ease.

This makes a much prettier index header:

    ```dataview
    LIST FROM #demo_index WHERE contains(source, this.file.link)
    ```

```dataview
LIST FROM #demo_index WHERE contains(source, this.file.link)
```

If you don't mind using community plugins, I highly recommend Dataview for both index headers and the indexes themselves, wherever manual labor won't suffice.