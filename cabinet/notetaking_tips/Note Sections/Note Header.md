# Note Header

The "header" of a note usually refers to its YAML header. This is a special section you can add at the very, very top of your note, where you can use the YAML data format (or JSON) to add metadata to your note.

Your header **must** start with `---` on its own line, and end with `---` on its own line.

Like this:

```
---
# stuff here
---
```

You can add comments (additional information that isn't just keys and values) to your header using `#`, as shown above

You then separate keys and their values by a single colon, like so:

```
---
status: done
---
```

Your keys cannot have any spaces in them. So if you want to separate words, you need to use dashes or underscores.

```
---
outlining-status: done
writing_status: in progress
---
```

You could use something like camel case (below). 

But for performance reasons, it's best to always use lowercase letters in keys.

```
---
outliningStatus: done
writingStatus: in progress
---
```

To separate multiple values in a key, you need to wrap all the values with brackets and add commas between entries.

```
---
aliases: [bob, bobby, bobbette]
---
```

That mean that if your value has a comma in it, it needs to be wrapped in quotes to avoid breaking the list.

```
---
catch_phrases: ["Coolio!", "Catch up, buttercup!"]
---
```
