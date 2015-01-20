---
layout: post

title: The destination of the allocated memory
subtitle: "The missed feature of PHP profilers"

excerpt: "The missed feature of PHP profilers"
---

**Disclaimer**: *I am not an expert of PHP internals and I don’t know the chances of this feature to be implemented,
but as any developer I am optimistic and open for dreams :)*

2014 was a really lucky year for PHP developers worrying about performance of their applications.
Two new profilers were launched: [Blackfire by SensioLabs](https://blackfire.io/)
and [Qafoo profiler](https://qafoolabs.com/).
Both of them are really interesting inheritors of [Xhprof](http://pecl.php.net/package/xhprof) because
they represent old-known facts in efficient manner and provide helpful feedback to developers.

In December Blackfire team introduced the brand new feature: tallying
[the spent time of garbage collector](http://blog.blackfire.io/performance-impact-of-the-php-garbage-collector.html).
It gives the missed knowledge about unobvious activity of PHP to developers.

Today I want to make the public request of another missed feature: **destination of the allocated memory**.

Existent profilers work in terms of a stack trace. This approach helps to discover problems with execution
time of an application because of CPU and IO activities. But when you try to detect memory leaks it’s hard
to do that in such terms. Developer receives information about a context (a function or a method)
when memory is allocated, but there is no chance to see its destination.
This understanding is crucial when you work with complex applications where memory is being allocated
and freed many times (especially in long running CLI scripts).
Thus a profiler gives you a lot of information that doesn’t help you to understand where the allocated memory
is **accumulated** during request.

When I work with PHP applications I assume that all possible destinations may be classified by three categories.

## #1 System memory

Memory is accumulated in the PHP core itself or its extensions. There is nothing to do with your application,
so you should update PHP (every next version of PHP is more efficient) or update / tune / remove some of extensions.

**Expected report format**:

* core – x<sub>0</sub> Mb, y<sub>0</sub>%
* ext1 – x<sub>1</sub> Mb, y<sub>1</sub>%
* ext2 – x<sub>2</sub> Mb, y<sub>2</sub>%
* &hellip;

## #2 Blocked memory

Your application works properly, but some of data was removed from the application and wasn’t removed
from memory because of cyclic dependencies. This issue may be resolved by garbage collector automatically
or direct calling of ``gc_collect_cycles``. You should decide which approach is best for your application.

**Expected report format**: blocked memory – x Mb, y%

## #3 Busy memory

You should explore a report of your application’s memory usage for possible anomalies in terms of PHP entities
(global variables / class fields / object properties). This part is the most valuable because it provides
clear understanding about memory usage of the application by its components for the developer.

**Expected report format**:

* Global variables:
  * ``$_SERVER`` – x<sub>0</sub> Mb, y<sub>0</sub>%
  * ``$_GET`` – x<sub>1</sub> Mb, y<sub>0</sub>%
  * &hellip;
* Class fields:
  * ``SomeClass::$property`` – k<sub>0</sub> Mb, m<sub>0</sub>%
  * ``AnotherClass::$property`` – k<sub>1</sub> Mb, m<sub>1</sub>%
  * &hellip;
* Object properties:
  * ``Id0@SomeClass->$property`` – z<sub>0</sub> Mb, q<sub>0</sub>%
  * ``Id1@SomeClass->$property`` – z<sub>1</sub> Mb, q<sub>1</sub>%
  * &hellip;

``Id0`` and ``Id1`` are object’s identifiers for splitting information about objects of the same class.
It would be awesome if ``{Concrete}`` profiler will provide ``{Concrete}ProfilerObjectInterface`` with
``get{Concrete}ProfilerIdentifier`` method for defining custom identifier.

I realize that different variables may refer to the same memory (because of PHP object model),
so sum of memory usage may be greater than 100%. In other words these values answer the question
"How much memory will be busy if other variables are removed".

Unfortunately none of current tools give such information, otherwise let me know about it!

Happy to discuss the proposed feature in comments.