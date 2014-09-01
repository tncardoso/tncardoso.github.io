---
layout: post
title: artreepie, a procedural art bot
tags: [art, codegolf]
---

<div class="centered">
<img src="{{ "img/artreepie/icon.png" | prepend: site.baseurl }}" width="64px" height="64px" class="post-icon">
</div>

Last week, I stumbled upon a great 
[stackexchange discussion on procedural art](http://codegolf.stackexchange.com/questions/35569/tweetable-mathematical-art). 
The idea was to come up with three code snippets that would be used to generate an image.
More specifically, each snippet -- one for each
color channel (R, G, B) -- is a function that, given 
a pixel coordinate, returns its intensity. Each snippet should fit in a single tweet.

This very interesting exercise inspired me into creating
[_@artreepie_](http://www.twitter.com/artreepie). _artreepie_ is a Twitter bot that captures mentions to
him containing code snippets and generates art pieces. The three pieces of code sent to _artreepie_ from a
user are used to build the image, which is sent back to the user by the bot.

## Implementation

_artreepie_ uses a modified version of
[twik](http://blog.labix.org/2013/07/16/twik-a-tiny-language-for-go),
a tiny lisp-like language, in its rendering engine.
The code snippet to be tweeted to _artreepie_ may use a number of provided variables and functions:

| Variable | Description                                                  |
|:---------|:------------------------------------------------------------:|
|    i     | Column number of pixel being calculated (starting from zero) |
|    j     | Row of pixel being calculated (starting from zero)           |
|    w     | Image width in pixels (currently 1024)                       |
|    h     | Image height in pixels (currently 1024)                      |


| Function          | Syntax    | Example           |
|:------------------|:---------:|------------------:|
| If                | if        | (if true 1 0)     |
| And               | and       | (and true false)  |
| Or                | or        | (or true false)   |
| Sine              | sin       | (sin 1.0)         |
| Cosine            | cos       | (cos 1.0)         |
| Bitwise And       | &         | (& 1 2 3)         |
| Bitwise Or        | \|        | (\| 1 2 3 )       |
| Modulo            | %         | (% 10 2)          |
| Random [0.0, 1.0) | rnd       | (rnd)             |
| Square Root       | sqrt      | (sqrt 16)         |
| Square            | sq        | (sq 10)           |
| Less Than         | <         | (lt 1 4)          |
| Greater than      | >         | (gt 1 4)          |
| Equals            | ==        | (== 1 1)          |
| Not equal         | !=        | (!= 1 2)          |

## Ok, I am ready to use it

In order to use _artreepie_ all you need is a Twitter account. Tweets are processed
in the order they were posted: the first tweet will be used for the 
red channel, the second for the green channel and the third for the blue channel. 

For example, this 
<a href="{{"img/artreepie/example1.png" | prepend: site.baseurl }}" target="_blank">image</a>
was generated with the following tweets:

{% highlight lisp %}
@artreepie (& i j)
@artreepie (% (+ i j) 255)
@artreepie (& i j)
{% endhighlight %}

In case of errors during the computation of the image -- e.g. division by zero -- the whole
image is silently discarded. For testing different expressions before submitting it to _artreepie_,
you may use a command line version of the bot available on the project page on 
<a href="http://www.github.com/tncardoso/artreepie" target="_blank">GitHub</a>.

## Some generated images

Here are some example images, listed in the stackexchange thread, written using twik:

{% highlight lisp %}
@artreepie (if (and (!= i 0) (!= j 0)) (& (% i j) (% j i)) 0)
@artreepie (if (and (!= i 0) (!= j 0)) (+ (% i j) (% j i)) 0)
@artreepie (if (and (!= i 0) (!= j 0)) (| (% i j) (% j i)) 0)
{% endhighlight %}
See the <a href="{{"img/artreepie/example2.png" | prepend: site.baseurl }}" target="_blank">result</a>.

{% highlight lisp %}
@artreepie (var s (/ 3.0 (+ j 99))) (* (+ (% (+ (* j s) (* s (+ i w))) 2) (% (+ (* j s) (* s (- (* w 2) i))) 2)) 127)
@artreepie (var s (/ 3.0 (+ j 99))) (* (+ (% (+ (* j s) (* s (+ i w))) 2) (% (+ (* j s) (* s (- (* w 2) i))) 2)) 127)
@artreepie (var s (/ 3.0 (+ j 99))) (* (+ (% (+ (* j s) (* s (+ i w))) 2) (% (+ (* j s) (* s (- (* w 2) i))) 2)) 127)
{% endhighlight %}
See the <a href="{{"img/artreepie/example3.png" | prepend: site.baseurl }}" target="_blank">result</a>.

## Your turn!

What can you come up with? Tweet and get into <a href="https://twitter.com/artreepie/media" target="_blank">
@artreepie's gallery</a>

## TL;DR

1. Tweet three code snippets in twik mentioning <a href="http://www.twitter.com/artreepie" target="_blank">@artreepie</a>;
2. Check the <a href="https://twitter.com/artreepie/media" target="_blank">image gallery</a> on Twitter;
3. Fork the <a href="http://github.com/tncardoso/artreepie" target="_blank">repository</a> and have fun!
