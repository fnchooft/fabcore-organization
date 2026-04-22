# Doc (2025-05-28)

## getting-started/gsl-example/README.md

# Generator Scripting Language - teaching Gemini

First of all a shout-out to the [GSL](https://github.com/zeromq/gsl)-tool.
It is fantastic, simple, robust and after you get the hang of it - extremely addictive.

Please read more about it - and about its author [Pieter Hintjes](https://en.wikipedia.org/wiki/Pieter_Hintjens).

During a recent course AI was explained and someone mentioned that
Gemini performs better then DeepSeek and ChatGPT in coding tasks.

So I took that opertunity to:

1. Teach Gemini about [GSL](https://github.com/zeromq/gsl)
2. Provide the README.txt file as context
3. Explain what I wanted, and asked it to generate some artifacts.

## The assignment

I want to teach you about GSL, more context is provided here: [README.txt](https://github.com/zeromq/gsl/blob/master/README.txt).

Ask: Write an entities.xml file which contains some entities with typical fields such as name,age etc.

Gemini produced: 

### entities.xml

<!-- include: entities.xml lang=xml -->
<!-- /include -->

After this I requested it to write the gsl-template to generate, on the basis of the entity-format C-structs.

### entity_generator.gsl
The resulting template after some back and forth looks like this:

<!-- include: entity_generator.gsl lang=c -->
<!-- /include -->

I then asked it to generate a main.c file and a Makefile in order to compile the entire example:

### main.c

<!-- include: main.c -->
<!-- /include -->

### Makefile

<!-- include: Makefile lang=makefile -->
<!-- /include -->


## Generated artifacts

As this should be a generic entity-generator, this first template generates C-code.
This might not be the cleanest code, however a 'seasoned' C-developer would be able
to provide a working version, which contains best practices for memory-management etc.

This working example could easily be taking by AI or a Human/AI - Centaur to generate
templates for other languages.

So, here come the generated artifiacts.

### Person.h

<!-- include: Person.h -->
<!-- /include -->

### Person.c

<!-- include: Person.c -->
<!-- /include -->


## Conclusions

1. Gemini is better at this then the other solutions I tried. It outperformes DeepSeek ( which I think is really good!)
2. Gemini might get stuck on a certain issue - for instance - in the templates, the code which needs to be generated should not have a dot as first character.
3. C-code might be better, but leave it to experts ( humanzzzz ) to provider better examples which we can incorporate.


Included file: by-date/2025/05/28-doc_assets/entities.xml

## getting-started/poc-space/README.md

---
description: Space to save Proof-of-Concepts
---

# PoC-space

Some Proof-of-Concepts will be added here.

By keep the folder self-contained it should be relatively easy to add content.



## gitbook-notes.md

---
description: >-
  While implementing this - we always find little nuggets of information which
  helped us along the way...
---

# GitBook notes

## Images online

[https://unsplash.com](https://unsplash.com/s/photos/projects) - I found unsplash to contain some great OpenSource images.

## Include files

Its a mess!
What you would want to have from GitBook is a proper include-option.

I mean by that add a snipplet of code which can include source-code as we love to provide usefull examples.
So we want to be able to:

1. Have a source-file in the repo.
2. Have the ability to tell Markdown/GitBook how to render it ( c, python, erlang, elixir, Makefile, bash , etc)
3. Have the option option of allow the user to copy this directly

I have not found a proper way - since all tell you to copy and paste the source in the MD-file, and since I am
lazy, I prefer using another technique.

### Back to M4

Pre-processing files is old-school, but it is super easy - after you know how to do it.

The idea is from here: https://stackoverflow.com/questions/4779582/markdown-and-including-multiple-files

Basically, iterate over each folder which has a "README.md.template" file, and in that folder execute:

```
cd $FOLDER && m4 -I. README.md.template > README.md
```

That's it!


## Links

- https://gitbook.com/docs/creating-content/blocks/code-block
- https://gitbook.com/docs/creating-content/blocks/insert-files
- https://mbreen.com/m4.html





