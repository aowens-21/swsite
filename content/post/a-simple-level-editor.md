---
title: "A Simple Level Editor"
date: 2018-01-15
draft: false
---

Level Editor
------------
Code available [here](https://github.com/aowens-21/grid-level-editor).  

This is the second week of 2018 and it's the second week I decided to do some programming
and blogging. This week I wanted to write something in Python because I like the language a lot
and I'm going to be using it for a class this semester.  I decided to make a very small grid-based level editor using the 2D game framework [pygame](https://pygame.org/). This level editor is super simple, and it needs a lot of work before I'd use it for an actual game, but it was a good learning experience in both python and pygame.

This is a screenshot of what it looks like using the actual editor:  

<p align="center">
  <img src="/img/level-editor-screenshot.png">
</p>

As you can see, I probably won't be selling it any time soon, but it does resemble a 2D platformer level.

<br/>

Uses
----
If you aren't familiar with how this might be used, I can think of a couple of examples that might illustrate its purpose a little better.

#### Import into a 2D Platformer Engine
This level editor exports to a text file that has an array of integers, so for example something like:  

```
0 0 0  
0 0 0  
1 1 1  
```

would be empty space with a platform on the bottom of the level. You could quickly make levels in my tool and then have your engine read in the text file to populate the structure of your level. This saves more time than hand coding each level (or it's at least a little more enjoyable).

#### Raycasting Engine
[Wolfenstein 3D](https://en.wikipedia.org/wiki/Wolfenstein_3D) used a technique called [Raycasting](https://en.wikipedia.org/wiki/Ray_casting) to render a level that appears 3D. There is a very popular implementation of a similar, simple raycasting engine available at [Lode Vandevenne's site](http://lodev.org/cgtutor/raycasting.html). In this implementation, the 3D map is represented in an almost identical way, meaning you could use my tool to make maps for a raycasting engine and then read them in through a renderer.

<br/>

Python Is Good
--------------
I'm not going to go into the entire codebase, because you can see it publicly on my github if you're curious, but there are a couple of awesome features in Python that, in my opinion, made my code cleaner/less cluttered.

#### List Comprehensions
Digital Ocean has [an amazing article](https://www.digitalocean.com/community/tutorials/understanding-list-comprehensions-in-python-3) discussing what you can do with Python 3 list comprehensions, and I used quite a few in this level editor.  

For example, here is the code to initialize an empty level that runs on startup:

```python
                def init_level_structure(self):
                    return [[Block(x=x, y=y) for x in range(self.width_in_blocks)] for y in range(self.height_in_blocks)]
```

In other languages, this would probably have to be nested for loops on multiple lines and you'd have to have a local variable to store the array before you filled it. In Python, however, you can use a list comprehension to keep the code less-cluttered but still readable.

#### Enumerate
Python's [enumerate](https://docs.python.org/2/library/functions.html#enumerate) function is also incredibly useful, and I think it might be underused. Here's an example of its usefulness:

```python
                for x, row in enumerate(self.level_structure):
                    for y, block in enumerate(row):
                        txt_level[x][y] = 1 if block.filled else 0
```

This function essentially combines the iterator for loop with an indexed for loop. In this example I'm looping over a 2D array called `self.level_structure`. `row`, as you might expect, is the row in the array (the data), whereas `x` keeps an index of the corresponding row. This way I am able to keep an index and iterate over the data; this is the "pythonic" way of doing this kind of task.

If you're learning Python and you want to find more little tips like this, I recommend checking out Brett Slatkin's book: [Effective Python](https://effectivepython.com/). Or if you don't care for programming books, just google "effective python tips" and I'm sure you'll find something similar.

<br/>

Conclusion
----------
I learned a lot about Python with this little project, and I'll probably do many more of these blogs about Python and Pygame over the next year or so, because I really like Python. If you enjoyed this post, check out my other blogs or [my github](https://github.com/aowens-21), where you can find all the code from all of these blogs. Thanks a lot for reading!
