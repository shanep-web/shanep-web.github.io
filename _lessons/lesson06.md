---
title: Lab 2 (ch2)
tag: html
layout: lab
points: 50
---

## Overview

In this lab you are going to create a webpage about your hometown. This doesn't have to necessarily be about your hometown,
you can select any town (or city) you want!

## Videos

- [VSCode and Git Installed](https://www.youtube.com/watch?v=lix7G-S8ox8)
- [Class Coding standards](https://shanep-web.github.io/docs/coding-standards.html)
- [Cloning and Submitting](https://shanep-web.github.io/docs/github.html)

## Task 1 - Write HTML (25pts)

Create a web page about your hometown like the example below. The example is an image, you will need to create the correct
HTML.

![Image showing what the webpage is supposed to look like]({% link assets/images/labs/lab2.png %})

Your web page must meet the following requirements:

- Use an appropriate title for the web page `<title>`.
- Use `<h1>` tags containing the hometown name followed by 3 sections, each using `<section>` tags.
- The 1st section should contain a heading in `<h2>` tags entitled "Location". A single paragraph should briefly describe the
  town's location.
- The 2nd section should contain a heading in `<h2>` tags entitled "About". Create at least 3 subsections that each use
  `<section>` tags with `<h3>` headings. Each subsection should have a single paragraph describing something about your hometown.

The 3rd section should contain a heading in `<h2>` tags entitled "Things To Do". Create an unordered list of interesting things to do in your hometown.

## Task 2 - Unit testing (10pts)

Copy the tests below into your testing file and run all the tests. 100% of the tests must pass!

```javascript
QUnit.test('Test page title', function(assert) {
   assert.equal(document.getElementsByTagName('title').length, 1, 'One <title> tag exists');
   assert.ok(document.getElementsByTagName('title')[0].text.length > 1, '<title> tag contains text');
   assert.ok(document.querySelector('head > title') !== null, '<title> is inside head, not <body>');
});

QUnit.test('Test h1 heading', function(assert) {
    assert.equal(document.getElementsByTagName('h1').length, 2, 'One <h1> tag exists');
    assert.ok(document.querySelectorAll('body > h1:first-child').length !== 0, '<h1> is first tag in <body>');
    assert.ok(document.getElementsByTagName('h1')[0].innerText.length >= 1, '<h1> tag contains text');
});

QUnit.test('Test 3 outer sections', function(assert) {
   let sections = document.querySelectorAll("body > section");
   assert.equal(sections.length, 3, 'Three <section> tags exist');
});

QUnit.test('Test section h2 headings', function(assert) {
   let h2 = document.querySelectorAll('body > section > h2');
   assert.equal(h2.length, 3, 'Three <h2> tags exist in sections');
   h2 = document.querySelectorAll('body > section > h2:first-child');
   assert.equal(h2.length, 3, 'Three <h2> tags are at the top of each section');
   for (let i = 0; i < h2.length; i++) {
      assert.ok(h2[i].innerText.length >= 1, '<h2> tag for section ' + (i+1) + ' contains text');
   }
});

QUnit.test('Test 1st section paragraph', function(assert) {
   let paras = document.querySelectorAll('body > section > p');
   assert.ok(paras.length >= 1, 'At least one <p> tag exists in 1st section');
   assert.ok(paras[0].innerText.length >= 1, 'First <p> tag contains text');   
});

QUnit.test('Test 2nd section\'s subsection h3 headings', function(assert) {
   const sections = document.querySelectorAll('body > section:nth-of-type(2) > section');
   assert.ok(sections.length >= 3, 'At least 3 subsections exist in 2nd section');      
   let h3 = document.querySelectorAll('body > section:nth-of-type(2) > section > h3:first-child');
   assert.equal(h3.length, sections.length, 'Number of 2nd section\'s subsections that have <h3> tags at top');
   
   for (let i = 0; i < h3.length; i++) {
      assert.ok(h3[i].innerText.length >= 1, 'Subsection ' + (i+1) + '\'s <h3> tag contains text');
   }
});

QUnit.test('Test 2nd section\'s subsection paragraphs', function(assert) {
   const sections = document.querySelectorAll('body > section:nth-of-type(2) > section');
   assert.ok(sections.length >= 3, 'At least 3 subsections exist in 2nd section');
   const paras = document.querySelectorAll('body > section:nth-of-type(2) > section > p');
   assert.equal(paras.length, sections.length, '<p> tags exist in every 2nd section\'s subsections');
   
   for (let i = 0; i < paras.length; i++) {
      assert.ok(paras[i].innerText.length >= 1, 'Subsection ' + (i+1) + '\'s <p> tag contains text');
   }
});

QUnit.test('Test 3rd section list', function(assert) {
   const list = document.querySelectorAll('body > section:nth-of-type(3) > ul');
   assert.ok(list.length >= 1, 'At least one <ul> exists in 3rd section');
   
   const listItems = document.querySelectorAll('body > section:nth-of-type(3) > ul > li');
   assert.ok(listItems.length >= 1, 'At least one <li> exists in 3rd section\'s unordered list');
});

```

## Task 3 - Flipgrid (10pts)

Show off your website to the class. Once you have everything ready create a video using Flipgrid!

- [How to record your screen](https://help.flip.com/hc/en-us/articles/360045940833-Screen-Recording-How-to-record-your-screen-using-the-Flipgrid-camera)
- [Flipgrid topic]({{site.data.semester-info.flip[page.slug]}})

You need to demo the following:

- Show the completed page(s).
- Show your unit tests passing

## Task 4 - Complete the Retrospective (5pts)

Once you have completed all the tasks open the file **Retrospective.md** and complete each section with a TODO comment.

## Task 5 - Add, Commit, Push your code

Once you are finished you need to make sure that you have pushed all your code to GitHub for grading!
