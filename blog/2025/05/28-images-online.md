---
description: "Images and include files"
tags:
  - gitbook
  - tips
  - include
---

# Images online

I found unsplash to contain some great OpenSource images: [https://unsplash.com](https://unsplash.com/s/photos/projects).

## Include files

Its a mess!

What you would want to have from GitBook is a proper include-option.

I mean by that add a snipplet of code which can include source-code as we love to provide usefull examples.
So we want to be able to:

1. Have a source-file in the repo.
2. Have the ability to tell Markdown/GitBook how to render it ( c, python, erlang, elixir, Makefile, bash , etc)
3. Have the option option of allow the user to copy this directly
4. Inform the lines ( ranges ) of text you want to include

I have not found a proper way - since all tell you to copy and paste the source in the MD-file, and since I am
lazy, I prefer using another technique.

### Back to M4

Pre-processing files is old-school, but it is super easy - after you know how to do it.

The idea is from here: https://stackoverflow.com/questions/4779582/markdown-and-including-multiple-files

Basically, iterate over each folder which has a "README.md.template" file, and in that folder execute:

```
cd $FOLDER && m4 -I. README.md.template > README.md
```

That's it! And its not clean, nor helpfull.

Another attempt will be made to improve this.

## Links

- https://gitbook.com/docs/creating-content/blocks/code-block
- https://gitbook.com/docs/creating-content/blocks/insert-files
- https://mbreen.com/m4.html





