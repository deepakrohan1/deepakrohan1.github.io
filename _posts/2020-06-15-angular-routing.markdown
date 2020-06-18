---
layout: post
title:  "Routing"
date:   2020-06-15 12:40:34 -0400
categories: App Module
---


Routing to different pages on button click, or based on the URL

{% highlight typescript %}

import { Routes, RouterModule } from '@angular/router';
import { UserHomeComponent } from './user-home/user-home.component';


const routes: Routes = [
  {path: 'user', component: UserHomeComponent}
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }

#-> when routed to http://localhost:4200/user , this routes you to the user details page

{% endhighlight %}

You can safegaurd these routes by using `Guards`. You can do lazy loading of components by modularizing them. 

___Route Based Loading___

Once the modules are configured to be lazy loaded, you can configure the code such that the browser loads the modules only when they are accessed.

This reduces time to load angular package.