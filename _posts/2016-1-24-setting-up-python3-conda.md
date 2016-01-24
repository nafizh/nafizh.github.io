---
layout: post
title: Setting up Ipython notebook with python3 through Conda
---

Recently, I have been studying the book [Python Machine Learning](http://www.amazon.com/Python-Machine-Learning-Sebastian-Raschka/dp/1783555130/ref=sr_1_2?ie=UTF8&qid=1437754343&sr=8-2&keywords=python+machine+learning+essentials) by [Sebastian Raschka](http://sebastianraschka.com/). The book uses python 3 rather than python 2. My Mac OS had been setup with python 2 with [Anaconda](https://www.continuum.io/downloads) by Continuum Analytics. Anaconda is a python distribution that includes all essential packages needed for data science so that you do not need to worry about installing them yourself.

The great news is through their package management system Conda, you can just create a python 3 environment and open an ipython notebook from that environment that uses python 3. 

On Mac OS, just type the following command -

![alt text]({{ site.baseurl }}/images/teminal1.png)

**py3k** is your environment name, you can name it as you wish. Writing **anaconda** means you want all the packages. If you just want any specific package you can replace **anaconda** with that.

To activate the environment with python 3, write in your terminal-

![alt text]({{ site.baseurl }}/images/terminal2.png)

and to deactivate-

![alt text]({{ site.baseurl }}/images/terminal3.png)

To get the ipython notebook with python3, just activate the environment with python3 and open a notebook from the terminal. It should work fine. This stack overflow [post](http://stackoverflow.com/questions/24405561/how-to-install-2-anacondas-python-2-7-and-3-4-on-mac-os-10-9/24415581) was specially helpful.

