---
layout: post
title: "Introduction to iPython Notebook"
subtitle:  "CS N373 Software Engineering Week 2"
date:   2015-06-19 21:50:51
categories: ipython notebook
---

## Introduction to iPython Notebook
{: #intro}
When I first started interning at Sotera I was introduced to the iPython Notebook. At first sight I was awed and amazed that something like it existed. It was like the perfect interpreter. Markdown documentation, code blocks, autocomplete, what else can a developer ever need? (maybe git intergration) but I'm sure there's an plugin for that too. 

[iPython Notebook Introduction](#intro)  
[How to Install...](#instal)  
[Ready to Rock!](#using)  
[The Cool Parts](#functionality)  
[Saving your Notebook](#saving)   
[Viewing using nbViewer](#viewing)  
[Loading Python Notebooks from the Web](#loading)  


Notebook Collections
[A gallery of interesting IPython Notebooks](https://github.com/ipython/ipython/wiki/A-gallery-of-interesting-IPython-Notebooks)  
[NBViewer](http://nbviewer.ipython.org)  
[Notebook Gallery: Links to the best IPython and Jupyter Notebooks.](http://nb.bianp.net/sort/views/)  
[My Github](http://github.com/marek5050/Notebooks)  




## How to install...
{: #install}

The same person also told me that it's a pain in the a$$ to install and the best way to get it working was using [Contiuum's Anaconda package][anaconda]. It'll probably ask for your email address, but I haven't really received much spam from them, I don't think it even needs to be correct. The packages come in 2 different flavors, Python 2.4 and Python 3.4 so choose accordingly. If you have both, you'll just have to install them into seperate directories. 

Once you download the PKG file, just open and install it. Then open up the terminal. 

![dividing into code blocks]({{% site.url %}}/static/python_notebook/installation_succesful.png)

{% highlight bash %}
$ python --version
Python 3.4.3 :: Anaconda 2.2.0 (x86_64)

$ ipython --version
3.0.0
{% endhighlight %}

## Ready to Rock!  
{: #using}

To startup the iPython Notebook execute the following command, there are optional parameters like --notebook-dir=~\notebooks where you can specify the Notebooks folder. By default the first page that opens will be the terminal's current working directory. 

{% highlight bash %}
$ ipython notebook
{% endhighlight %}

In a brand new folder the page will look lonely, so let's try to create a new one.

+ Click on the +
+ Select Python
+ Open up [Professor Downing's Python Exceptions page ][exceptions] 
+ Copy the Python code into the new Notebook.

![new python notebook]({{% site.url %}}/static/python_notebook/new_notebook.png)

To execute the script we can do two things __control+Enter__ or press the Play arrow up top. 

![new python notebook]({{% site.url %}}/static/python_notebook/donefirsttime.png)

Okay to take advantage of the notebook let's divide the script into multiple code blocks. Scroll to the boottom, click on the last line, and hit __Option+Enter__ to create a new code block. Notice the __In [ ]: __ indicates code blocks and the number within the number of times it has been executed. Let's copy and paste the __print__ message and execute it. You'll notice it prints! 

![new python notebook]({{% site.url %}}/static/python_notebook/donetwice.png)

Also notice the top blocks haven't changed and we can still see the Done. right above. Let's try to re-evaluate the top block again with __control+Enter__. Boom, no more Done. 

## The cool parts
{: #functionality}

Let's divide the document into multiple code blocks. 

![dividing into code blocks]({{% site.url %}}/static/python_notebook/divided.png)

I pretty much ended up seperating the initialization code, exception 1, exception 2, and asserts. Now we can move one by one and execute the blocks.
Beginning with block one, we were told it won't raise an exception and it didn't. Let's try changing the False to True, just to experience the result.

![execute the first code block]({{% site.url %}}/static/python_notebook/exception1.png)


So now it failed, with a pretty heavy stack trace. Good thing we could chunk our code and we actually have a pretty good understanding where it failed. Change it back to False and __control+Enter__. The stack trace disappears.

![execute the first code block]({{% site.url %}}/static/python_notebook/exception1stacktrace.png)


Same story happens with block number 2. Try running the block with __control+Enter__ the code block is executed without any problems. Change up True to False, and we get a pretty nice AssertionError once again. 

![execute the second code block]({{% site.url %}}/static/python_notebook/exception1.png)

![execute the first code block]({{% site.url %}}/static/python_notebook/exception1stacktrace.png)

If you by accident press __option+Enter__ and create a new cell, you can delete it by clicking on the scissors up on the main toolbar. 

Another wonderful feature of iPython Notebooks is the documentation, create a new cell at the very beginning and convert it to __Markdown__. The __In[]:__ will dissapear and type in:  

{% highlight bash %}
# Exceptions.py
---

{% endhighlight %}
![nbViewer exceptions]({{% site.url %}}/static/python_notebook/exceptions_rendered.png)


Just like regular cells execute the markdown cell. It will render the Markdown. To change it again just double click the cell. 


## Saving your Notebook 
{: #saving}

On the top bar click on __untitled__ and give the notebook a name __Exceptions__. By default the notebook will be saved in the working directory where __python notebook__  was executed. To return click on the jupyter logo. 

For sharing the Notebooks we have two options.
  * [Gist][gist] - just dump the Python Notebooks for the fun of it.
  * Github Repository - you actually want to use git version control. 
      * The Github Repositories will actually render the ipynb files. 
      ![nbViewer exceptions]({{% site.url %}}/static/python_notebook/gist_example.png)
* Open the Notebook file with a text editor.
	![Open the file with an editor]({{% site.url %}}/static/python_notebook/textedit.png)

* Open up the browser and head to Github’s [Gist][gist]. 
* Click on the __+__ on the top bar to open a new Gist.
* Add a description "Sharing the amazing Exceptions file!"  
* __Name this file....__ call it Exceptions.
* Paste the contents of the Notebook file. 
* Click Create public Gist.
* Now the part you need from this page will be in the URL  
  * https://gist.github.com/username/__f55db2d73868b0bacd54__  
  The last identified is called the __Gist id__.
  That's what needs to be copied.
  * On the right side of the page we can also copy the URL by clicking the clipboard.


## Viewing using nbViewer
{: #viewing}


Now go to [nbViewer][nbviewer] page. 

* Paste in the __Gist id__
* Or the Github username if the Notebook is located in a public Github 	repository.
	* Try __marek5050/Notebooks__ and selecting one, maybe __Factorials.ipynb__.  

![nbViewer exceptions]({{% site.url %}}/static/python_notebook/nbviewer_exceptions.png)

The notebook will render within the browser and in the top right corner we have a couple of options. To Download the Notebook and to visit the Github/Gist page.

There’s a way to make the public Notebooks renderable without downloading, just haven’t tried it yet, that would probably be the third button.

## Loading Python Notebooks from the Web
{: #loading}

Up top I linked a couple of awesome resources and collections of notebooks. I’ll just walk you through loading one.

Download the Notebooks using the terminal in one of the following ways.
* Open up the terminal.
    {% highlight bash %}
$ wget https://github.com/marek5050/Notebooks/raw/master/notebook1.ipynb --no-check-certificate
$ ipython notebook  
    {% endhighlight %} 
    {% highlight bash %}
$ git clone https://github.com/marek5050/Notebooks
$ cd Notebooks
$ ipython notebook
    {% endhighlight %}


This will spin up the iPython Notebook server and you should be able to see the files.

* Open up timeseries.ipynb.

![timeseries]({{% site.url %}}/static/python_notebook/timeseries.png)

* Now execute the first cell.

![time series loaded Bo library]({{% site.url %}}/static/python_notebook/firstblockbo.png)

* Execute the next cell..

![Timeseries rendered first cell]({{% site.url %}}/static/python_notebook/timeseries1.png)

and the next.

![Timeseries rendered second cell]({{% site.url %}}/static/python_notebook/timeseries2.png)

 * Remember that cells might depend on the preceding cells. 
 * So it might not be possible to just jump to the last one and execute it. 


### Problem #1: 
If you already have Python feel free to download it anyways, theis packag will probably be more use in the future. 

### Problem #2:
I already installed Anaconda with Python 2.4 and want to update to 3.4. 
So Anaconda will try and install into the ~/anaconda directory, just change it to ~/anaconda2 and it'll install fine. 

Then you can just ln -s ~/anaconda2/bin/python /etc/bin/python3
to create a symbolic link to the updated python. 

[anaconda]: http://continuum.io/downloads
[exceptions]:https://github.com/gpdowning/cs373p/blob/master/examples/Exceptions.py
[gallery]: https://github.com/ipython/ipython/wiki/A-gallery-of-interesting-IPython-Notebooks
[gist]: https://gist.github.com  
[SymPy]:https://github.com/marek5050/Notebooks/blob/master/notebook1.ipynb
[SymPyDownload]: https://github.com/marek5050/Notebooks/raw/master/notebook1.ipynb
[nbviewer]: http://nbviewer.ipython.org