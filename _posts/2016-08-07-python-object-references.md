**points**:

- variables only *refer to* object
- `==` and `is`
- copy and deepcopy
- function parameters are passed as aliases
- del and reference counting

#### Variables Are Not Boxes

Variables are not *boxes*, it's better to think of them as *labels* attached to objects.

{% highlight python %}
>>> class Gizmo:
...     def __init__(self):
...         print("Grimo id: {}".format(id(self)))

>>> x = Gizme() * 10
# Gizmo id: 4301489432
# TypeError: unsupported operand type(s) for *: 'Gizmo' and 'int'

>>> dir()
# ['Gizmo', '__builtins__', '__doc__', '__loader__', '__name__', '__package__', '__spec__']
{% endhighlight %}

Variables are assigned to objects only after the objects are created.

#### Identity, Equality, and Aliases

charles and lewis refer to the same object

{% highlight python %}
>>> charles = {'name': 'Charles L. Dodgson', 'born': 1832}
>>> lewis = charles
>>> charles == lewis
True
>>> id(charles) == id(lewis)
True
>>> charles is lewis
True
>>> lewis["balance"] = 950
>>> charles
{'name': 'Charles L. Dodgson', 'balance': 950, 'born': 1832}
{% endhighlight %}

alex and charles compare equal, byt alex is not charles

{% highlight python %}
>>> alex = {'name': 'Charles L. Dodgson', 'born': 1832, 'balance': 950}
>>> alex == charles # 1
True
>>> alex is not charles
True
{% endhighlight %}

1. alex and charles compare equal, because of the `__eq__` implementation in the dict class

> Every object has an **identity**, a **type** and a **value**. An object's identity never changes once it has been created; you may think of it as the object's *address in memory*. The `is` operator compares the identity of two objects.
>
> — [The Python Language Reference](https://docs.python.org/3/reference/datamodel.html#objects-values-and-types)

#### Function Parameters as References

The paremeters inside the function become aliases of the actual arguments. The result of this scheme is that a function may change any mutable object passed as a parameter.

{% highlight python %}
>>> def f(a, b):
...     a+=b
...     return a
>>> a = [1, 2]
>>> b = [3, 4]
>>> f(a, b)
>>> (a, b)
([1, 2, 3, 4], [3, 4])
{% endhighlight %}

#### Mutable Types as Parameter Defaults: Bad Idea

> The default values are only evaluated at the point of function definition in the *defining* scope.

defensive programming with mutable parameters

{% highlight python %}
class Defensive:
    def __init__(self, para=None):
        if para is None:
            self.para = []
        else:
            self.para = list(para) # 1
{% endhighlight %}

1. `list(para)` produces a *shallow copy* or convert it to a list

#### del and Garbage Collection

> Objects are **never explicitly** destroyed; however, when they become unreachable they may be garbage-collected.
>
> — The Python Language Reference

The `del` statement deletes names, not objects.

In CPython, the primary algorithm for garbage collection is *reference counting*. Essentially, each object keeps count of how many references point to it. As soon as that *refcount* reaches zero, CPython calls the `__del__` method on the object which should not be called by your code.



