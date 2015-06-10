---
layout: post
title:  "Javascript Testing using Mocha and Istanbul"
subtitle: "Test what??"
date:   2015-06-10 21:50:51
categories: javascript testing mocha istanbul
---

You can download NodeJs on the [home page](https://nodejs.org/download/), using [Homebrew](http://brew.sh), or [MacPorts](http://macports.org), relatively simple and straight forward.
 
{% highlight bash %}
% sudo npm install mocha --global
% sudo npm install istanbul --global
   
% which node
/opt/local/bin/node
{% endhighlight %} 

Now we can create three files, the core module ( sum.js ), the run harness ( run.js ), and the  test( test.js ).


 
**sum.js**

{% highlight javascript %}
Calculator = {
   sum: function(a,b){
     return a+b;
    }
   }  

module.exports = Calculator;
{% endhighlight %} 


**run.js**

{% highlight javascript %}
#!/opt/local/bin/node#!/opt/local/bin/node 

var Calc = require('./sum.js');

console.log("1+2="+Calc.sum(1,2));
{% endhighlight %} 


**test.js**

{% highlight javascript %}  
var Calc = require("./sum.js");
var assert = require("assert");

 suite('sum',
    function () {
        "use strict";

        test('test_1',
            function () {
                assert.equal(Calc.sum(1,1), 2);});

        test('test_2',
            function () {
                assert.equal(Calc.sum(1,2), 3);});

        test('test_3',
            function () {
                assert.equal(Calc.sum(10,1), 11);});});
{% endhighlight %} 
 

Now to see it in action.

{% highlight bash %}  
% chmod +x ./run.js
% ./run.js
1+2=3
%
% mocha -u tdd test.js
  sum
    ✓ test_1
    ✓ test_2
    ✓ test_3
  3 passing (7ms)
%
% istanbul cover _mocha -- -u tdd test.js 
  sum
    ✓ test_1
    ✓ test_2
    ✓ test_3
  3 passing (5ms) 

==========================================================================

Writing coverage object [/Users/marekbejda/Desktop/School/oop/testing/coverage/coverage.json]
Writing coverage reports at [/Users/marekbejda/Desktop/School/oop/testing/coverage]

==========================================================================


========================= Coverage summary ===============================

Statements   : 100% ( 10/10 )
Branches     : 100% ( 0/0 )
Functions    : 100% ( 5/5 )
Lines        : 100% ( 10/10 )
============================================================================
{% endhighlight %} 

