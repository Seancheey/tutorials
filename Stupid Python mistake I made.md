Last night I was doing a LeetCode question about Sudoku, when I wanted to generate a list of 9 empty list elements, which is like this:
```python
[[],[],[],[],[],[],[],[],[]]
```

Of course as a ***smart*** programmer I won't waste my time in typing 9 empty columns but thought up another way to create that list: `[[]] * 9`, which at least made perfect sense to me.

However, something magic happened when I start to put elements in those lists:
```
>>> a = [[]]*9
>>> a[3].append(5)
>>> a
[[5], [5], [5], [5], [5], [5], [5], [5], [5]]
```
:/ .... emmmm

So Python, in fact, is doing pretty smartly here: Whenever we called `[]`, python will actually translate it into `list()`, which is a constructor that allocates free memory and creates a pointer for us to use. So when I used `[[]]*9`, Python actually only copied literally 9 pointers which points to the same memory.

So the correct way to create n empty list of list could be like this:
```python
[[] for _ in range(9)]
# or
[list() for _ in range(9)]
```

:/..... Right, it does feel weird to use list generator, but it is my best solution to do that at least.

***I hate Python :D***
