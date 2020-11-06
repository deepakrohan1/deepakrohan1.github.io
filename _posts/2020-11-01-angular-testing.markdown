---
layout: post
title:  "Angular Testing"
date:   2020-11-03 12:40:34 -0400
categories: App Module
---


# Jasmine and Karma
Unit Testing helps you to answer - 
`Is this going to work if I insert X?' or Does X still return the same results now that I have implemented Y?' scenarios`


Angular is built keeping in mind testing. The framework allows for you to `Simulate Server Side Requests` API Calls and also provide an `abstraction` of `DOM`


## Karma: 

Runs the tests of your application. It's by core `a Node server` watching the changes to test files, when it detects it kind of runs them in the browser and checks for mistakes

## Jasmine:

Is one of the most popular unit testing frameworks for JavaScript

## Creating a new project

```
ng new <any-name>
```

karma.conf.js

<{% highlight javascript %}
module.exports = function (config) {
  config.set({
    basePath: '',
    frameworks: ['jasmine', '@angular-devkit/build-angular'],
    plugins: [
      require('karma-jasmine'),
      require('karma-chrome-launcher'),
      require('karma-jasmine-html-reporter'),
      require('karma-coverage-istanbul-reporter'),
      require('@angular-devkit/build-angular/plugins/karma')
    ],
    client: {
      clearContext: false // leave Jasmine Spec Runner output visible in browser
    },
    coverageIstanbulReporter: {
      dir: require('path').join(__dirname, './coverage/sample-testing-project'),
      reports: ['html', 'lcovonly', 'text-summary'],
      fixWebpackSourcePaths: true
    },
    reporters: ['progress', 'kjhtml'],
    port: 9876,
    colors: true,
    logLevel: config.LOG_INFO,
    autoWatch: true,
    browsers: ['Chrome'],
    singleRun: false,
    restartOnFileChange: true
  });
};
{% endhighlight %}>


## Decoding the File

`reports: ['html', 'lcovonly', 'text-summary'],` tells you the type of output of the report. `lcov` file is used by SonarQube for coverage

`browsers: ['Chrome'],` tells you the list of browsers or runtime env's

`restartOnFileChange: true` keeps restarting the testcases as soon as `spec` file is saved


## Angular.json

{% highlight json %}

 "test": {
          "builder": "@angular-devkit/build-angular:karma",
          "options": {
            "main": "src/test.ts",
            "polyfills": "src/polyfills.ts",
            "tsConfig": "tsconfig.spec.json",
            "karmaConfig": "karma.conf.js",
            "assets": [
              "src/favicon.ico",
              "src/assets"
            ],
            "styles": [
              "src/styles.css"
            ],
            "scripts": []
          }

{% endhighlight %}

## test.js

{% highlight javascript %}

// First, initialize the Angular testing environment.
getTestBed().initTestEnvironment(
  BrowserDynamicTestingModule,
  platformBrowserDynamicTesting()
);
// Then we find all the tests.
const context = require.context('./', true, /\.spec\.ts$/);
// And load the modules.
context.keys().map(context);

{% endhighlight %}


`const context = require.context('./', true, /\.spec\.ts$/);` kind of intializes spec across the project. You can run a single component and child component test cases by updating the component. 

# Building Blocks

Writing test cases are similar to many other Test Driven Environments

* ___Suites___ the top-level element of the testing framework. They accept a title (explanation) and a function containing one or more specifications
  ```
  describe(string, function)
  ```
* ___Specs___ constructs that take a title and a function containing one or more expectations. Specs are nested into suites.
  ```
  it(string, function)
  ```
* ___expectations___ assertions leading to True or False
  ```
  expect(actual).toBe(expected)
  ```
* ___Assertions___ Redefined helpers for common assertions
  ```
  toEqual(expected)
  ```

{% highlight javascript %}

expect(fn).toThrow(e);
expect(instance).toBe(instance);
expect(mixed).toBeDefined();
expect(mixed).toBeFalsy();
expect(number).toBeGreaterThan(number);
expect(number).toBeLessThan(number);
expect(mixed).toBeNull();
expect(mixed).toBeTruthy();
expect(mixed).toBeUndefined();
expect(array).toContain(member);
expect(string).toContain(substring);
expect(mixed).toEqual(mixed);
expect(mixed).toMatch(pattern);

{% endhighlight %}

## Teardown

These are used to `prep` for the testcases. We need to load some data or mock service `before each` spec or may be reset a list of data `after each` testcases we use the methods `beforeEach` and `afterEach`

{% highlight javascript %}
// single line
beforeEach(module("itemsApp"));

// multiple lines
beforeEach(function() {
  module("itemsApp");
  //...
});

{% endhighlight %}


## Dependency Injects

{% highlight javascript %}

  TestBed.configureTestingModule({
      imports: [
        RouterTestingModule
      ],
      declarations: [
        AppComponent
      ],
      providers: [SampleService, Pipes],
      imports: [RouterTestingModule.withRoutes([])],
      schemas: [NO_ERRORS_SCHEMA]
    }).compileComponents();
  }));


  fixture = TestBed.createComponent(BannerComponent);
  component = fixture.componentInstance; 
  service = TestBed.get('SomeService');
  h1 = fixture.nativeElement.querySelector('h1');

    it('should display original title', () => {
        expect(h1.textContent).toContain(component.title);
    });

{% endhighlight %}


* ___imports___

### Change Detections 

In production world `change detection` happens automatically but here we kind of run `fixture.detectChanges()`  

### Eg.

{% highlight javascript %}

it('should check for some text', () => {
    component.user.name = 'John'
    fixture.detechChanges();
    expect(h1.textContent).toContain('John');
});

{% endhighlight %}

### Dispatch Events

To simulate `user inputs` combined with __detectChanges()__ to trigger the change detection

{% highlight javascript %}

someInput = 'John';
nameInput.dispatchEvent(new Event('input'));
fixture.detectChanges();

expect(nameDisplay.textContent).toBe('Quick Brown  Fox');

{% endhighlight %}

### Service Injection

To inject the `service` which a component is dependent on 

{% highlight javascript %}
TestBed.configureTestingModule({
   declarations: [ WelcomeComponent ],
// providers: [ UserService ],  // NO! Don't provide the real service!
   providers: [ { provide: UserService, useValue: userServiceStub } ],
});
{% endhighlight %}

we can use `useClass or useValue`. We stub the methods in the below way

{% highlight javascript %}

let userServiceStub = {
    getLoggedInUsername() {
        return of('John');
    },

    getUserPermissions() {
        return of(['profilePage', 'AccountPage'])
    }
}

{% endhighlight %}

### fixture.whenStable()

The fixture.whenStable() returns a promise that `resolves` when the JavaScript engine's `task queue becomes empty`. In this example, the task queue becomes empty when the observable emits the first quote.

The test resumes within the promise callback, which calls `detectChanges()` to update the quote element with the expected text.

{% highlight javascript %}



{% endhighlight %}