---
layout: post
title:  "Getting Started Python 1"
date:   2020-07-01 12:40:34 -0400
categories: Python Basics
---

# Python 1

## Fundamental DataTypes in Python

* int
* float
* book
* str
* list
* tuple
* set
* dict

## Classes

Custom types

## Special Data Types

Not built into Pythons special packages in python. 

## None

Nothing `0` 

### Integers

Whole Numbers

{% highlight python %}

print (2 + 4)
print (type (2 + 4))

{% endhighlight %}

## String

`str('test')`

### string format

You can format the string in print statement

{% highlight python %}
a = 'John'
print(f'hello {a})

print('hello {0}'.format({a}))
{% endhighlight %}

### String indexes

Prints the String along with `start, stop and step`

{% highlight python %}

a = '1234567890'

`a[start: stop: step]`

{% endhighlight %}

Inverse of a String

{% highlight python %}

a = '123456689'
`a[::-1]`

{% endhighlight %}

## Immutable

Strings are immutable

{% highlight python %}

a = '12345'
a[0] = '5' # This wont work, you need to reassign the entire variable.

{% endhighlight %}

## List

`a = [1,2,3,4,5]` assume an array.

### index of

```
a.indexOf(element, start, stop)
```
### sort vs sorted 

`.sort` method sorts the array and returns nothing, updates the list calling it

`sorted` returns a new sorted array, doesn't update the existing one

{% highlight python %}
a = [1,2,3,4,5]

a.sort()

sorted(a)

{% endhighlight %}

`copy and reverse` are used to copy and reverse the list

Reverse an array

{% highlight python %}

a[::-1]

{% endhighlight %}