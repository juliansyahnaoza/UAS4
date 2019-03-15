---
title: Single Page Application
comments: true
---
**What, Why, and How. That's the main question.**

SPA (single page application) is a web app or site that only have 'one page' so you did not need to make too much page request. The popular example is gmail website. SPA is the new trend several years ago until now. 

Why a lot people decide to use SPA ? simple, it create desktop-application-like experience since you did not need to reload your webpage to do some action. So the rest action will be handled secretly by javascript ninja lol.

### Okay, then How? 
There are a lot of JS framework that can help you, like i used today, [AngularJS](https://angularjs.org)!

As a programmer who familiar with MVVM and data-binding concept, this framework help me so much. You just define the ViewModel (in angular, we use called `Controller`), you can attach the data model using directive `ng-model`.

below some cool [example](http://www.w3schools.com/angular/tryit.asp?filename=try_ng_module): 

![the source](https://raw.githubusercontent.com/putuyoga/putuyoga.github.io/master/assets/images/source_code_ng.JPG)

See that? you just update the property, without call any method, the model will be updated seamlessly. I am still trying explore this one. 

You can check [my repository](https://github.com/putuyoga/yoga-core) to follow what i've done so far with AngularJS and ASP.NET Core.

Cheers!