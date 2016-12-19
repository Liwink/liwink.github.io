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

#### Dict

Python use a hash table to implement a `dict`.

A hash table is a sparse array. In standard data structure texts, the cells in a hash table are often called "buckets."
In a `dict` hash table, the bucket for each item containing two fields: a reference to the key and a reference to the value of the item.

The **fetch the value** at `my_dict[search_key]`. Python calls `hash(serach_key)` to obtain the hash value of `search_key` and uses the least significant bits of that number as an offset to look up a bucket in the hash table.

If the bucket is empty, `KeyError` is raised.

Otherwise, the found buckets has an item -- a `found_key:found_value` pair -- and then Python check whether `search_key == found_key`.
If they match, that was the item sought: `found_value` is returned.

However, if `serach_key` and `found_key` do not match, this is a `hash collision`.

![dict_hash_search](images/dict_hash_search.png)

And here is a [sample code](https://gist.github.com/Liwink/1bd695e86dfba980fec69fa397c1cb38) to show how a `dict` uses `__hash__` and `__eq__` methods of the items.

