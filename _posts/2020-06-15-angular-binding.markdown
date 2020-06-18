---
layout: post
title:  "Binding"
date:   2020-06-15 12:40:34 -0400
categories: Binding
---

## Interpolation

Lets start with interpolation, you need access or display any variable on template. You cam use `{ { } }` to access and display them.

Assuming Component has the below object.

```
const user = {
    'name': 'John',
    'email': 'John@email.com'
}

HTML
----

{ {user.name} }
```

You can also do a bunch of operation on them `{ { 1 + 1 } }`

Do a tertiary condition inside `{ { i !== null ? i : '' } }`


## Expression

You can use them as expression when they are integrated with HTML

{% highlight html %}

<h4>{ {user.name} }</h4>
<img [src]="user.imageSrc">

<ul>
  <li *ngFor="let user of users">{{user.name}}</li>
</ul>

{% endhighlight %}

May be a button click

{% highlight html %}


  <input type="button" (click) = "onSaveClick($event)">{{user.name}}</li>


{% endhighlight %}

`onSaveClick` is a method in the component which handles the logic.

Good practices to follow
* Simplicity
* It should be resolved Quickly
* No Visible side effects

## Oneway Vs Two way binding

### Oneway

* Interpolation `{ { } }`
* Property `[]`
* Attributes `style = "abc"`
* Event `(click) = "someMethod()`

### TwoWay

It is a combination of target (property) and event binding 

`[] + () => [()]`

{% highlight html %}

<input [(ngModel)]="name"> 

{% endhighlight %}