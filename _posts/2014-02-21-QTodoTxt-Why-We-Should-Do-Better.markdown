---
layout: post
title:  "QTodoTxt: Why We Should Do Better!"
date:   2014-02-21 07:00:00
categories: en
excerpt: Last month I released v1.2.0 of QTodoTxt, this release was both awesome and horrible...
---
![QTodoTxt_logo]({{site.url}}/assets/QTT256.png)


Last month I released v1.2.0 of [QTodoTxt](https://github.com/mNantern/QTodoTxt) and this release was both awesome and horrible.
Awesome because for the first time I've got external contributors, great new features and a lot of download (more than 300 in one month).

And it's when I saw those numbers that I realized that QTodoTxt wasn't a toy project anymore.
Obviously it's useful to someone and because I have users I have to make QTodoTxt better for them too !

And that's why the last release was horrible:

* One of the main feature (Mac Os X package) does not [work as expected](https://github.com/mNantern/QTodoTxt/issues/36) and binary packages of QTodoTxt in general are not good enough ([Windows package](https://github.com/mNantern/QTodoTxt/issues/48) as an issue and [Ubuntu too](https://github.com/mNantern/QTodoTxt/issues/45))
* We had a code regression because I was not paying enough attention to my tests.
* We introduced a lot of new features but never took the time to document it.

So the next version (v1.3.0) will be focused on packaging and documentation.
Not very glamorous but I think it's necessary. I will only include 1 new feature and the regression's fix.

My little list of things to do:

* Make a github site for QTodoTxt in order to show documentation, news and a proper download page
* Set up Travis CI to prevent regression ([already done](https://github.com/mNantern/QTodoTxt/issues/42))
* Improve packaging for Ubuntu...
* ... And Mac Os X
* Set up a PPA repository for Ubuntu users

If you have a little time, any help is welcome !
