# Iteratables and Iterators

- Iterables don't return self when iter() method is used externally. The id of the object passed to the iter() method and the id of object would be different since self is not returned for iterable.


- Iterator's iter function is going to return itself. Iterators are exhaustable, they have both next and iter methods. To make the iterator non-exhaustable, we can use a iterator factory;

Field   | Iterators | Iterables
--------|-----------|-----------
__iter__ | Yes  | Yes
__next__ | Yes  | Not necessarily required


- Added below Iterator factory to make existing code a iterable.

```
class PolygonSequence:

...

    def __iter__(self):
        return self.PolygonSeqIterator(self)

    class PolygonSeqIterator:
        def __init__(self, pseq_obj):
            self.pseq_obj = pseq_obj
            self.index = 0

        def __next__(self):
            if self.index >= len(self.pseq_obj):
                raise StopIteration
            else:
                polygon = self.pseq_obj.sequence[self.index]
                self.index += 1
                return polygon

        def __iter__(self):
            return self

```

Below are additional test cases:

```
    p = PolygonSequence(n, R)
    p_iter = iter(p)


    assert id(p) != id(p_iter),  "Not an iterable!"
    assert id(p_iter) == id(iter(p_iter)), "Not an iterator"

    _ = [next(p_iter) for i in range(n-2)] 

    with pytest.raises(StopIteration):
        next(p_iter)

    assert '__iter__' in dir(p_iter), "iterator not found"
    assert '__next__' in dir(p_iter), "iterator needs __next__"

    assert '__iter__' in dir(p), "iterable needs __iter__"
    assert '__next__' not in dir(p), "iterable doesn't need __next__"


```

[Colab Notebook] (https://colab.research.google.com/drive/1h98BR71HV8-oIhbYxpm62KrfWRvAn7UT?usp=sharing) 

