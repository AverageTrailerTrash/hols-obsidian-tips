# Attribute Files

Attribute files provide additional metadata for another note in the vault. They are most useful when used alongside Dataview inline keys, as you can access the metadata through a link in the parent note.

For example, you may have the character `Bob`. In the body of their note, you have this:

```
attributes:: [[Bobs Attributes]]
```

In the header of `Bobs Attributes`, you have this:

```
---
height: tall
hair_color: red
skin_color: brown
---
```

Now, at any time in any file, you can grab that information with from that key.

For example, this:

```
`=[[Bob]].attributes.height` 
````

would return `tall`.

For more complex systems, you may want to have multiple attribute sets, or even nested attribute files, to avoid having to make one big massive attribute file.

Nested attribute files requires that you use Dataview inline keys to link the subattributes.

For example, `Bobs Attributes` may have this in its body instead:

```
appearance:: [[Bobs Appearance]]
birth_data:: [[Bobs Birth Data]]
personality_traits:: [[Bobs Personality Traits]]
```

Now the appearance data is in the header `Bobs Appearance`:

```
---
height: tall
hair_color: red
skin_color: brown
---
```

To get his height, we'd have to access it like this:

```
`=[[Bob]].attributes.appearance.height` 
````

which would return `tall`.