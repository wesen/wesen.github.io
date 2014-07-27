---
layout: post
title: Autocompleting blender python in Intellij
category: articles
---

Blender has internal python scripting, which is used for all kinds of purposes.
I like using intellij IDEA for python development, as the python support is very good.
It is also very nice to have autocompletion and go to source support.
This post will show how to setup autocompletion for intellij,
including for the binary modules in blender python.

# Setting up the SDK

Blender uses python 3 internally,
so we need to use a python 3 SDK in intellij.
Also note that this setup won't allow you to execute python code in blender
directly from intellij, you will need to use the run script functions in blender.

We will then include the python content of blender directly into the project,
which will cause them to be indexed and also allow us to navigate to source
or read other addons to learn about blender python programming.

Navigate to your blender folder (on MacOSX, this is inside the blender bundle
under Contents/MacOSX/x.yy)
and add the scripts folder to your project.
Mark the modules folder as "source folder" so that intellij will recognize
the `bpy` module.

![center](/figs/blender/2014-07-27--intellij-project-setup.png)

This will give you a good starting to develop blender scripts.

# Indexing the blender python bindings

The more interesting part is indexing the `bpy` API that is implemented in
blender in C. To do this, we need to use the intellij `generate3.py` script
to create stubs for the binary functionality.

First, add the intellij stubs directory to your SDK.
On my computer, this directory can be found under
`Library/Caches/intellijidea13/python_stubs/-1199934129`,
this may vary on your computer.

The `generator3.py` can be found in the application support directory of
your Intellij installation. For me on MacOSX, this is under `Library/Application Support/IntelliJIdea3/python/helpers`.

We now need to run this script from inside blender,
so that we can inspect the whole API and generate stubs for it.
For this, run the following scripts inside blender:

{% highlight python %}
import sys

# Edit this to the path to your IntellijIDEA13 directory
sys.path += ['/Users/manuel/Library/Application Support/IntelliJIdea13/python/helpers']

import generator3

path = '/Users/manuel/Library/Caches/IntelliJIdea13/python_stubs/-1199943129'
generator3.quiet = False

modules = ['mathutils', 'blf', 'gpu', 'aud', 'bpy_extras', 'bmesh', 'bgl', '_bpy']
# uncomment this line, switch to the blender game engine and press play to index
# the bge content
# modules = ['bge.types', 'bge.logic', 'bge.render', 'bge.texture', 'bge.events', 'bge.constraints']
for i in list(modules):
    generator3.process_one(i, None, True, path)
{% endhighlight %}

Run this script to create python stubs for the binary modules.

# Indexing the BGE modules

To index the bge content, we need to do something a bit special.
The BGE modules are only available while the BGE is running.
To execute the python script above,
we need to setup a trigger in the Logic Editor.


![center](/figs/blender/2014-07-27--intellij-bge-setup.png)


Press run in the BGE, and there you go, indexed BGE modules.
