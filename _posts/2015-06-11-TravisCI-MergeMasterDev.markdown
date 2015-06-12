---
layout: post
title: "Automatic Dev -> Master merge and make check fail"
subtitle:  "Tear down the walls! Between branches! "
date:   2015-06-11 21:50:51
categories: git branches travisci
---

I've edited the makefile to fail on whenever make check fails. On a missing file it just creates a .failed file and at the end checks for its existence and continues appropriately. 

{% highlight bash %}
check:
	@for i in $(FILES);                                         \
        do                                                          \
        if [ -e $$i ]; then echo "$$i found" ; else touch .failed && echo "$$i NOT FOUND" ; fi \
        done ; \
        if [ -e .failed ]; then exit 1; else echo "Found all"; fi
{% endhighlight %}

Make push merges the dev and master branches, this will be invoked after the make check command in the .travis.yml

{% highlight bash %}
push:
ifeq ($(TRAVIS_BRANCH),dev)
	git config --global user.email "marek.bejda@gmail.com"
	git config --global user.name "Marek5050"
	git remote set-branches --add origin master
	git fetch
	git reset --hard
	git checkout master --force 
	git merge --no-ff --no-edit dev
	git push origin master
endif
{% endhighlight %}

{% highlight bash %}
.travis.yml
script:
    - make collatz-tests
    - make Collatz.html
    - make Collatz.log
    - make test
    - make check
    - make push
{% endhighlight %}


Links  
[.travis.yml][travis]  
[makefile][makefile]


[travis]: /static/git_push/travis.yml
[makefile]: /static/git_push/makefile