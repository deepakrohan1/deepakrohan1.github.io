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

[sample-api]: https://4xsgtpbbni.execute-api.us-east-1.amazonaws.com/prod/

___Ex___

Use the above API req to get users from the API

{% highlight typescript %}
 
 ## Service
 getUserData() {
    return this.httpClient.get('https://4xsgtpbbni.execute-api.us-east-1.amazonaws.com/prod/', {responseType: 'json'});
  }

  ## component

  userDetails: any;

  ngOnInit() {
    console.log('Initializing the Page')
    this.userDtService.getUserData().subscribe(res => {
        console.log("AppComponent -> ngOnInit -> res", res['body']);
        this.userDetails = JSON.parse(res['body']);

    }, err => {
    console.log("AppComponent -> ngOnInit -> err", err);
      
    });

  }

{% endhighlight %}

## Designing the view

{% highlight html %}

  <div *ngFor = "let user of userDetails">
    <h3>{ {user.name} }</h3>
    <span>{ {user.email} }</span> <br/>
    <span>{ {user.phone} }</span>
  </div>

#-> 
___Amina Brekke___
Matilda97@gmail.com
(405) 653-7537

___Clay Sawayn___
Imani_Haley18@hotmail.com
828.465.1540
{% endhighlight %}

CONTD ... Please proceed with showing other components of data on screen

## Directives

Most Commonly Used

`*ngFor`

Looping of data on the screen, table esp.

`*ngIf`

Adding condtional logic to screen elements

`*` represents they are __Structural Directives__

For More refer below link

[Structural Directives][st-dir]

[st-dir]: https://angular.io/guide/structural-directives

## Directives

We have our own custom implementation Directives, you can handle a field to take always number by adding `HostListeners` and checking `key codes`