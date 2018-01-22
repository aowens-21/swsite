---
title: "Sorts With Python"
date: 2018-01-21
draft: false
---

## Sorts With Python
Code available [here](https://github.com/aowens-21/python-sorts).

This week I didn't have a whole lot of spare time to program, so I did something pretty standard for this blog: sorting.
I decided to implement 3 popular sorts (and maybe more to come) in Python: [Bubble Sort](https://en.wikipedia.org/wiki/Bubble_sort),
[Insertion Sort](https://en.wikipedia.org/wiki/Insertion_sort), and [Merge Sort](https://en.wikipedia.org/wiki/Merge_sort). I've seen and implemented these
sorts before, but I thought it would be a decent exercise to do them in Python since that's the language I've been most interested in recently.

### Bubble Sort
Bubble Sort is a very simple algorithm, but it becomes incredibly inefficient on large amounts of data. Here's my
implementation of Bubble Sort:

```python
                def bubble_sort(list):
                    for i in range(len(list) - 1):
                        for j in range(len(list) - i - 1):
                            if (list[j + 1] < list[j]):
                                temp = list[j]
                                list[j] = list[j + 1]
                                list[j + 1] = temp
                    return list
```

As you can see, it's pretty short. Essentially what this algorithm does is put the largest item in at the end until the list has been entirely sorted.
This is why in the second loop we can do `range(len(list) - i - 1)`. We only have to go through `i - 1` times for each iteration of the outer loop. For more information
about how the sort works, check out the Wikipedia page that I linked above. The runtime of this sort is O(n^2), though, which means you're much better off
learning one of the other more efficient sorts for practical uses.

### Insertion Sort
Insertion Sort is another simple algorithm that, while still inefficient, is a little better in general than Bubble Sort or Selection Sort. Here's my implementation:

```python
                def insertion_sort(list):
                    i = 1
                    while i < len(list):
                        j = i
                        while j > 0 and list[j - 1] > list[j]:
                            temp = list[j]
                            list[j] = list[j - 1]
                            list[j - 1] = temp
                            j = j - 1
                        i = i + 1
                    return list
```

The best way I've heard Insertion Sort explained is that it works in basically the same way we sort playing cards in our hand. We start out with one "sorted sublist" that just contains
the first element in the list. Then we repeatedly run through and "insert" items into the sorted sublist by swapping until it is in the correct position. It's a little harder to analyze
Insertion Sort's runtime, but the Big-O comes out to be O(n^2) just like Bubble Sort. However, Insertion Sort is very efficient at sorting small arrays, and it beats many
of the more complex sorting algorithms in that case. For more information check out the Wikipedia page I linked above.

### Merge Sorts
Merge Sort is one of the more efficient sorting algorithms, but it is initially a little harder to explain that Bubble Sort and Insertion Sort. Here's my implementation:

```python
                def merge_sort(list):
                    if len(list) <= 1:
                        return list

                    left = []
                    right = []

                    for i, item in enumerate(list):
                        if i < len(list) // 2:
                            left.append(item)
                        else:
                            right.append(item)

                    left = merge_sort(left)
                    right = merge_sort(right)

                    return merge(left, right)


                def merge(left, right):
                    merged_list = []

                    while left != [] and right != []:
                        if left[0] <= right[0]:
                            merged_list.append(left[0])
                            left = left[1:]
                        else:
                            merged_list.append(right[0])
                            right = right[1:]

                    if left != []:
                        merged_list += left
                    if right != []:
                        merged_list += right

                    return merged_list
```

Now you might be looking at this and thinking: "this seems way more complex than Bubble or Insertion Sort", but the reality is that even though Merge sort
has a larger implementation, its general idea is pretty simple. Merge sort takes in a list and then splits that list into two smaller lists: a left and a right.
It then calls merge sort on each of the sublists, which recursively call it again until only one element is returned, in which case the two elements (left and right), are
passed into a merge function. The way the merge function works is that it takes in two lists and iterates over each item in both lists, putting the items into a "merged list"
that is in sorted order, and then it returns the "merged list".

While this idea might be a little ambiguous without visual aid, there are some good diagrams on the Wikipedia page that display this sort very clearly. Merge Sort's Big-O runtime comes
out to be O(n log(n)), which is a very significant improvement over the other two algorithms I wrote.

### Conclusion
Sorting algorithms are a pretty interesting topic in Computer Science, even though most of the well established sorting algorithms have been around forever. This exercise
definitely taught me more about Python and I think next week I'll do something else, hopefully more interesting, in the language. I've been looking at [Django](https://www.djangoproject.com/), and I
think I'm going to check that out this week, so maybe I'll showcase that for the next blog post. Thanks for reading and I'll see you next time!
