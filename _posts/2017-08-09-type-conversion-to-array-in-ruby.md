---
layout: post
title: Array.wrap
---

There are a few ways to perform type conversion to arrays in Ruby.
The obvious and most prominently used method from my experience is `#to_a`. I learned,
however, that there are differences in the ways you can convert various types
of data into arrays.

### `#to_a`
Ruby allows you to call `#to_a` on several different types of data structures such as
**enumerables**:

{% highlight ruby %}
irb(main):001:0> (0..1).to_a
=> [0, 1, 2, 3, 4, 5]
{% endhighlight %}

or **NilClass**:

{% highlight ruby %}
irb(main):002:0> nil.to_a
=> []
{% endhighlight %}

or even **hashes** (notice the conversion!):

{% highlight ruby %}
irb(main):001:0> { foo: 'bar' }.to_a
=> [[:foo, "bar"]]
{% endhighlight %}

However, there's a strict limitation to calling `#to_a` on a variable that might
be a string, or an integer.

{% highlight ruby %}
irb(main):01:0> 1.to_a
NoMethodError: undefined method `to_a' for 1:Fixnum
{% endhighlight %}

### `Array()`
If we wanted to handle wrapping integers and strings, we can use the `Array()`
method to convert our unknown data structure.

{% highlight ruby %}
irb(main):01:0> Array(1)
=> [1]
irb(main):02:0> Array('1')
=> ["1"]
{% endhighlight %}

Once again though, there's a gotcha with this method in that it behaves the
same way as `#to_a` when converting a hash:

{% highlight ruby %}
irb(main):01:0> Array({ foo: 'bar' })
=> [[:foo, "bar"]]
{% endhighlight %}

### `Array#wrap`
Here's where the `Array#wrap` method comes in handy.

```
irb(main):01:0> Array({ foo: 'bar' })
=> [{:foo => "bar"}]
```

**NOTE**: This method is only available in Rails!

It also covers all the other cases like `nil`, strings, and integers.

### After Thoughts
`nil.to_a.to_h.to_s.to_i.to_f`
