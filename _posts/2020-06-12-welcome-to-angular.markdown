---
layout: post
title:  "Getting Started On Angular"
date:   2020-06-12 12:40:34 -0400
categories: Angular Getting Started
---


## Software Packages

* [NodeJS][nodejs-download]
* [Angular CLI][angular-cli]
* Editor

[nodejs-download]: https://nodejs.org/en/download/
[angular-cli]:https://angular.io/cli

## TypeScript

Covering the basics

[Playground][online-playground]

[online-playground]: https://www.typescriptlang.org/play/index.html

### Variable declarations

`let` and `const` keywords.

*const* refers to a variable which doesn`t change in the scope

*let* refers to variable which can be reassigned and changed

{% highlight typescript %}

function hello(person: string) {
    let greeting = 'hello ';
    greeting = 'hey '
    console.log(greeting + person);
}
hello('Sam');
#=> prints 'hey Sam' to CONSOLE.

function hello(person: string) {
    const greeting = 'hello ';
    console.log(greeting + person);
}
hello('Sam');
#=> prints 'hello Sam' to CONSOLE.

function hello(person: string) {
    const greeting = 'hello ';
    greeting = 'hey '
    console.log(greeting + person);
}
hello('Sam');
#=> Assignment to constant variable ERROR

{% endhighlight %}

### == and === operator

It acts the same in javascript and typescript. In simple words `==` doest not check for type conversions where as `===` checks for strict equality

{% highlight typescript %}

if (9 == "9") {
    console.log('Type check not done')
}
#=> prints 'Type check not done' to CONSOLE.

if (9 === "9") {
    console.log('Type check not done')
} 

#=> prints nothing to CONSOLE

{% endhighlight %}

### Types

The most common types we use

1. String
2. Boolean
3. number
4. Array
5. _Any_ used to represent `dynamic` content

{% highlight typescript %}

let fullname: string = 'Sam'
let someNumber: number = 22;
let collectionOfItems: string[]  = ['pc', 'mac'];
let isDone = true;


{% endhighlight %}

## Getting started with Angular App

Develop a simple `crud`  based application which performs a `GET` and `POST` request on the API, to show content from the database and add content to the database.

Angular application generation using `CLI`

{% highlight bash %}

ng new `app-name`
ng new demo-app 

--dry-run

# =>
ng new demo-app
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? CSS
{% endhighlight %}

After this it has start installing the necessary node modules to start the angular project. Running a suffix of `--dry-run` will help you to understand what the command will do.

## package.json

The file lists all the `dependencies` needed by the project. List the `scripts` you can run on the code. Defination of the scripts are in `angular.json`

{% highlight json %}
{
  "name": "demo-app",
  "version": "0.0.0",
  "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build",
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e"
  },
  "private": true,
  "dependencies": {
    "@angular/animations": "~9.1.7",
    "@angular/common": "~9.1.7",
    "@angular/compiler": "~9.1.7",
    "@angular/core": "~9.1.7",
    "@angular/forms": "~9.1.7",
    "@angular/platform-browser": "~9.1.7",
    "@angular/platform-browser-dynamic": "~9.1.7",
    "@angular/router": "~9.1.7",
    "rxjs": "~6.5.4",
    "tslib": "^1.10.0",
    "zone.js": "~0.10.2"
  },
  "devDependencies": {
    "@angular-devkit/build-angular": "~0.901.6",
    "@angular/cli": "~9.1.6",
    "@angular/compiler-cli": "~9.1.7",
    "@types/node": "^12.11.1",
    "@types/jasmine": "~3.5.0",
    "@types/jasminewd2": "~2.0.3",
    "codelyzer": "^5.1.2",
    "jasmine-core": "~3.5.0",
    "jasmine-spec-reporter": "~4.2.1",
    "karma": "~5.0.0",
    "karma-chrome-launcher": "~3.1.0",
    "karma-coverage-istanbul-reporter": "~2.1.0",
    "karma-jasmine": "~3.0.1",
    "karma-jasmine-html-reporter": "^1.4.2",
    "protractor": "~5.4.3",
    "ts-node": "~8.3.0",
    "tslint": "~6.1.0",
    "typescript": "~3.8.3"
  }
}
{% endhighlight %}

Adding a dependency to the `package.json`

You can search for any package on the below site. You can add it to the package.json

[Node Package Manager][npm]

[npm]: https://www.npmjs.com/

{% highlight bash %}

npm install

{% endhighlight %}

___CheatSheet For Angular CLI___ 

You can generate different components in angular using the cli

[Angular CLI][angular-cli-cheatsheet]

[angular-cli-cheatsheet]: https://malcoded.com/posts/angular-fundamentals-cli/

## Starting up the Application

Once inside the root folder of the project you can start the application by 


{% highlight bash %}

ng serve

{% endhighlight %}