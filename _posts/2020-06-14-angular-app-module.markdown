---
layout: post
title:  "App Module"
date:   2020-06-14 12:40:34 -0400
categories: App Module
---


## Decoding the App Module


__declarations__ The `components, directives, pipes` that belong to this module

__providers__ These are `services` which are created

__exports__ `component` this modules exports, so that these are visible to other modules or components

__imports__ other modules which are exported can be imported by this

__bootstrap__ the main application view, has the `root component` -> app component, Only the `root` module has the bootstrap property


[More on Modules][on-modules]

[on-modules]: https://angular.io/guide/architecture-modules

{% highlight typescript %}

import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HttpClientModule }    from '@angular/common/http';
import { UserHomeComponent } from './user-home/user-home.component';


@NgModule({
  declarations: [
    AppComponent,
    UserHomeComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpClientModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }


{% endhighlight %}