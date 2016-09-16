---
layout: post
title: "Python - An Array of Sequences"
description: "Notes for 'Fluent Python'"
category: Python
tags:  [Python,Python Data Structure]
---
{% include JB/setup %}

# Types of sequences

## Container versus Flat

+ Container sequences - can hold items of different types
    - list, tuple, and collections.deque
+ Flat sequences - hold items of one type (within its own memory space, limited to primitive values)
    - str, bytes, bytearray, memoryview, and array.array

## Mutable versus Immutable

+ Mutable sequences
    - list, bytearray, array.array, collections.deque, and memoryview
+ Immutable sequences
    - tuple, str, and bytes

# List Comprehensions and Generator Expressions

## example of Listcomp

```python
>>> symbols = '$¢£¥€¤'
>>> codes = [ord(symbol) for symbol in symbols]
>>> codes
[36, 162, 163, 165, 8364, 164]
```
## Notes

### Cartesian products: arranged by color first

```python
>>> colors = ['black', 'white']
>>> sizes = ['S', 'M', 'L']
>>> tshirts = [(color, size) for color in colors for size in sizes]
>>> tshirts
[('black', 'S'), ('black', 'M'), ('black', 'L'), ('white', 'S'),
('white', 'M'), ('white', 'L')]
```

### Line breaks are ignored inside pairs of `[]`, `{}` or `()`. Means you can do following without use `"\"`

```python
 cards = [Card(rank, suit) for suit in self.suits
                           for rank in self.ranks]
```

### Variable Leak in Python `2.x` but no longer in python `3.x`
Python `2.x`

```python
>>> x = 'my precious'
>>> dummy = [x for x in 'ABC']
>>> x
'C'
```

Python `3.x`

```python
>>> x = 'ABC'
>>> dummy = [ord(x) for x in x]
>>> x
'ABC'
>>> dummy
[65, 66, 67]
```

### Efficiency: listcomp is the best
example `code`:

```python
import timeit

TIMES = 10000

SETUP = """
symbols = '$¢£¥€¤'
def non_ascii(c):
    return c > 127
"""

def clock(label, cmd):
    res = timeit.repeat(cmd, setup=SETUP, number=TIMES)
    print(label, *('{:.3f}'.format(x) for x in res))

clock('listcomp        :', '[ord(s) for s in symbols if ord(s) > 127]')
clock('listcomp + func :', '[ord(s) for s in symbols if non_ascii(ord(s))]')
clock('filter + lambda :', 'list(filter(lambda c: c > 127, map(ord, symbols)))')
clock('filter + func   :', 'list(filter(non_ascii, map(ord, symbols)))')
```

result:

```python
listcomp        : 0.015 0.015 0.015
listcomp + func : 0.021 0.024 0.023
filter + lambda : 0.022 0.023 0.024
filter + func   : 0.020 0.022 0.025
```

## Example of Generator Expressions

Initializing a tuple and an array from a generator expression

```python
>>> symbols = '$¢£¥€¤'
>>> tuple(ord(symbol) for symbol in symbols)
(36, 162, 163, 165, 8364, 164)
>>> import array
>>> array.array('I', (ord(symbol) for symbol in symbols)) array('I', [36, 162, 163, 165, 8364, 164])
>>> colors = ['black', 'white']
>>> sizes = ['S', 'M', 'L']
>>> for tshirt in ('%s %s' % (c, s) for c in colors for s in sizes):
...     print(tshirt)
...
black S
black M
black L
white S
white M
white L
```

## Notes

A genexp saves memory because it yields items one by one using the iterator protocol instead of building a whole list just to feed another constructor.

# Tuples

## Tuples as Records

Tuples hold records: each item in the tuple holds the data for one field and the position of the item gives its meaning.

```python
>>> lax_coordinates = (33.9425, -118.408056)
>>> city, year, pop, chg, area = ('Tokyo', 2003, 32450, 0.66, 8014)
>>> traveler_ids = [('USA', '31195855'), ('BRA', 'CE342567'),
... ('ESP', 'XDA205856')]
>>> for passport in sorted(traveler_ids):
... print('%s/%s' % passport)
...
BRA/CE342567
ESP/XDA205856
USA/31195855
```

Tuple Unpacking


