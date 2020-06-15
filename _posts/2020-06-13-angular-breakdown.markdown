---
layout: post
title:  "Angular Components"
date:   2020-06-13 12:40:34 -0400
categories: Angular Getting Started
---

## Components

{% highlight typescript %}
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'demo-app';
}

{% endhighlight %}

## Decoding the code

### Selector

This tells you what portion on the `HTML` will be replaced by this component. So in the HTML where ever you notice `app-root` replace it with app component

Below file is the `index.html` where the app-root section is replaced by the HTML component

{% highlight html %}

<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>DemoApp</title>
  <base href="/">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
</head>
<body>
  <app-root></app-root>
</body>
</html>

{% endhighlight %}


## templateUrl

This can also be replaced by `template` and write `Markup` code in there. Refer the `app.component.html` for the template to be displayed.

{% highlight html %}
<span> { {title} } </span>
#=> renders a template with a span and title rendering the varible from component
{% endhighlight %}

## StyleUrls

You can add `reference to style.css` where you can style the html components

Try adding more content to the `component` and render them on the UI