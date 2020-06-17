---
layout: post
title:  "Services"
date:   2020-06-13 12:40:34 -0400
categories: Angular Services
---

## Services

They are used to interact with API's. They can be also used to share data across components. The main `objective of components` is to present data 

Generating a service

{% highlight bash %}

ng g service getUserData

#-> Creates a spec and service file

{% endhighlight %}

## Decoding a Service Class

Below is a sample Code

{% highlight typescript %}

import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class UserDataService {

  constructor() { }

  getUserDetails() {
      return [{
          name: 'John',
          place: 'NJ'
      }];
  }

}

{% endhighlight %}

`@Injectable` annotation tells that class is part of the `dependency injection`

The Service can get details for `User` from a mock data, API call

__providedIn__ a single instance is created the Service, any class which asks for it Angular Injects it.

Below is the example of component injecting the service.

{% highlight typescript %}

import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit{
  title = 'demo-app';

  construcor(private userData: UserDataService) {

  }

  ngOnInIt() {
      this.userData.getUserDetails();
  }

}

{% endhighlight %}

You can try getting data `async` using Observables.

{% highlight typescript %}

import { Observable, of } from 'rxjs';

getUserDetails(): Observable<any> {
    return of ([ {
        name: 'Sam',
        place: 'NYC'
    }])
}

#-> `of` converts the data into Observable
{% endhighlight %}

`any` represents the data type could be any Object

___Observables___ are `publisher/subscriber` pattern. The `Observer` is a entity which monitors a stream of data. If you subscribe to the stream you will be able to see the content of the streams.

__Subscribing to a stream in the component__

{% highlight typescipt %}

getUserData(): void {
    this.userDetail.getUserDetails()
        .subscribe(res => { 
            console.log(res)
    }, err => {
        console.error('Some err in HTTP call');
    });
}

{% endhighlight %}

[Sample API][sample-api]

[sample-api]: 

