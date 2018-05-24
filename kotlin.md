---
layout: docs
title: Getting started with Kotlin
description: Let's start with a standalone Kotlin app
group: getting-started
toc: true
---

## Gradle Setup

First, add the Koin dependency like below:

{% highlight gradle %}
// Add Jcenter to your repositories if needed
repositories {
    jcenter()    
}
dependencies {
    // Koin for Kotlin apps
    compile 'org.koin:koin-core:{{ site.current_version }}'
}
{% endhighlight %}

## Declaring our first dependencies

Let's create a Repository to provide some data (`giveHello()`):

{% highlight kotlin %}
interface Repository {
    fun giveHello()
}

class MyRepository() : Repository {
    override fun giveHello() = "Hello Koin"
}
{% endhighlight %}

A Presenter class, for consuming this data:

{% highlight kotlin %}
// Use Repository - injected by constructor by Koin
class MyPresenter(val repository : Repository){
    fun sayHello() = repository.giveHello()
}
{% endhighlight %}

Use the `applicationContext` function to declare a module. Let's declare our first component:

{% highlight kotlin %}
// Koin module
val myModule : Module = applicationContext {
    provide { MyPresenter(get()) } // get() will resolve Repository instance
    provide { MyRepository() as Repository }
}
{% endhighlight %}

## Start Koin

Now that we have a module, let's start Koin. Open `main` function, or write one. Just call the `startKoin()` function:

{% highlight kotlin %}
fun main(vararg args : String){
    // Start Koin
    startKoin(listOf(myModule))
}
{% endhighlight %}

That's it, Koin is running in your app! 

## Writing the application

To retrieve our `MyPresenter` component, we need to create a Koin runtime component. Let's write a `MyApplication` class, and extend it with `KoinComponent` interface. This latter allows us to use the `by inject()` operators: 

{% highlight kotlin %}
// Application
class MyApplication : KoinComponent {

    // Inject MyPresenter
    val presenter : MyPresenter by inject()

    init {
        // Let's use our presenter
        Log.i("MyActivity","presenter : ${presenter.sayHello()}")
    }
}
{% endhighlight %}

Let's update our `main` function, to launch the `MyApplication` class:

{% highlight kotlin %}
fun main(vararg args : String){
    // Start Koin
    startKoin(this, listOf(myModule))
    // Launch app
    MyApplication()
}
{% endhighlight %}

## What's Next?

You have finished this starting tutorial. Below are some topics for further reading:

- [Declaring components with The Koin DSL]({{ site.baseurl }}/docs/{{ site.docs_version }}/reference/koin-dsl#provide-definitions)
- [Provide aliases - bean, factory & provide]({{ site.baseurl }}/docs/{{ site.docs_version }}/reference/koin-dsl#aliases-bean-factory-)
- [Named dependencies]({{ site.baseurl }}/docs/{{ site.docs_version }}/reference/koin-dsl/#named-dependencies)
- [Import & Multiple module declaration]({{ site.baseurl }}/docs/{{ site.docs_version }}/reference/koin-dsl#linking-definitions---using-definitions-in-several-modules)
- [Using properties]({{ site.baseurl }}/docs/{{ site.docs_version }}/reference/properties/)
- [Checking your modules configuration with Dry Run]({{ site.baseurl }}/docs/{{ site.docs_version }}/reference/dry-run/)