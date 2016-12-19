The key of `dict` must be hashable.

> Hashability makes an object usable as a dictionary key and a set member, because these data structures use the hash value internally.

#### What Is Hashable

Hashing is a process of converting some large amount of data to small amount in a repeatable way, so that it can be looked up in constant-time(O(1)).

In Python:

> An object is hashable if a it has a hash value which never changed during its lifetime(it needs a `__hash__` method), and can be compared to other objects(it needs an `__eq__` method). Hashable objects which compare equal must have the same hash value.

#### Hashable Types

- The *atomic* immutable types are all hashable, such as `str`, `bytes`, `numeric` types
- A `frozen set` is always hashable(its elements must be hashable by definition)
- A `tuple` is hashable only if all its elements are hashable
- `User-defined types` are hashable by default because their hash value is their id()



{% highlight python %}
>>> tt = (1, 2, (30, 40))
>>> hash(tt)
8027212646858338501
>>> tl = (1, 2, [30, 40])
>>> hash(tl)
TypeError: unhashable type: 'list'
{% endhighlight %}

#### Why Is List Not Hashable

> An object is hashable if all of these requirements are met:
>
> 1. It supports the hash() function via a hash() method that always return the same value over the lifetime of the object.
> 2. It supports equality via an eq() method.
> 3. If a == b is True then hash(a) == hash(b) must also be True.

(from Fluent Python)

Lists are mutable and two lists can be equal if they have same items.

In contrast, user defined class instances are mutable as well, but each two of the instances cannot be equal and they are hashable(by id()).

{% highlight python %}
>>> a = [1, 2]
>>> b = [1, 2]
>>> a == b
True
{% endhighlight %}



