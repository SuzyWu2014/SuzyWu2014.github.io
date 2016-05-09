# The Python Data Model

## Python objects
All Data in a Python program represented by objects or by relations between objects. Every objects has an identity, a type and a value.

+ Identity
    * never changes once it has been created
    * `is` operation compares the identity of two object. e.g. `if obj1 is obj2`
    * `id(obj)` is the memory address where obj is stored (CPython)
+ Type
    * unchangeable
    * `type(obj)` function returns a object's type
    * operation on immutable types may return a reference to any existing object with the same type and value.
    * operation on mutable type will always return two different reference;
        - example:
            + `a = 1; b =1;` a and b may and may not refer to same object with value 1
            + `c = [ ]; b = [ ];` c and d are guaranteed to refer two different, newly created empty list.
            + `c = d = [ ];` c and d refer to same object.
+ Value
    * an object's mutability is determined by its type; 
        - immutable: numbers,strings, tuples...
        - mutable: dictionaries, lists...
    * immutable container object with mutable object reference; e.g. a tuple contains a reference to a mutable object.
        - containers: objects contain references to other objects; e.g. tuples, list, dictionaries.
        - container value changes when its mutable object is changed
        - a value of a container: imply the value of the contained object     

## Garbage collection
Objects are never explicitly destroyed; they may be garbage-collected when they become unreachable. (Not guaranteed)

+ (Current CPython Implementation) reference-counting scheme with delayed detection of cyclically linked garbage; not guaranteed to collect garbage with circular references.

>#### Reference-counting 
> As a collection algorithm, reference counting tracks, for each object, a count of the number of references to it held by other objects. If an object's reference count reaches zero, the object has become inaccessible, and can be destroyed.

## Glance of special methods

```python
import collections

Card = collections.namedtuple('Card', ['rank', 'suit'])

class FrenchDeck:
    ranks = [str(n) for n in range(2, 11)] + list('JQKA')
    suits = 'spades diamonds clubs hearts'.split()
    def __init__(self):
        self._cards = [Card(rank, suit) for suit in self.suits
    def __len__(self):
        return len(self._cards)
    def __getitem__(self, position): 
        return self._cards[position]
```

#### Example usage of special methods
  Special methods are meant to be called by the python interpreter. By implementing special methods, your objects can behave like the built-in types.

    >>> beer_card = Card('7', 'diamonds') 
    >>> beer_card
    Card(rank='7', suit='diamonds')
    # __len__
    >>> deck = FrenchDeck() 
    >>> len(deck)
    52
    # __getitem__
    >>> deck[0]
    Card(rank='2', suit='spades') 
    >>> deck[-1]
    Card(rank='A', suit='hearts')
    >>> deck[:3]
    [Card(rank='2', suit='spades'), 
     Card(rank='3', suit='spades'), 
     Card(rank='4', suit='spades')]

#### NOTE: 
+ `__repr__`: used for debugging and logging
+ `__str__`: used for presentation to end user; invoked by `str()`
+ `len()`: in CPython implementation, what `len()` do is simply  read from a field in a C struct. Therefore, the built-in `len()` is not called as a method.


[1]: Python Language Reference: Chapter 3 - Data model
[2]: Fluent Python: Chapter 1 -  The Python Data Model

