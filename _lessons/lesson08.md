---
title: Lab 3 (ch3)
tag: html
layout: lab
points: 50
---

## Overview

In this lab you are going to create a web form as shown below that allows the user to enter personal information for joining
a social network. The web page uses CSS to align the form widgets vertically.

## Videos

- [VSCode and Git Installed](https://www.youtube.com/watch?v=lix7G-S8ox8)
- [Class Coding standards](https://shanep-web.github.io/docs/coding-standards.html)
- [Cloning and Submitting](https://shanep-web.github.io/docs/github.html)

## Task 1 - Write HTML (25pts)

![Image showing what the webpage is supposed to look like]({% link assets/images/labs/lab3.png %})

The form should:

- Use the POST method and the "multipart/form-data" enctype
- Submit to https://wp.zybooks.com/form-viewer.php
- Use a `<label>` for each form field and the label text shown in the image above
- Use a `<div>` to surround each label and form widget and a `<div>` around the submit button
- Use a text `<input>` with name and id "fullName" to get the user's name
- Use a text `<input>` with name and id "email" to get the user's email address
- Use a `<textarea>` with 3 rows, 50 columns, and name and id "about" to get a short description about the user
- Use a text `<input>` with type "file" and name and id "picture" to get the user's image Use a submit `<input>` button
- Use the required attribute for name and email fields

## Task 2 - Unit testing (10pts)

Copy the tests below into your testing file and run all the tests. 100% of the tests must pass!

```javascript
QUnit.test('Test form tag', function(assert) {
   let form = document.querySelector('form');
   assert.ok(form !== null, '<form> tag exists');
   assert.equal(form.method.toUpperCase(), 'POST', 'Form method is POST');
   assert.equal(form.enctype, 'multipart/form-data', 'Form enctype is multipart/form-data');
   assert.equal(form.action, 'https://wp.zybooks.com/form-viewer.php', 'Form action is zybooks.com URL');
});

QUnit.test('Test name label', function(assert) {
   let divs = document.querySelectorAll('form div');
   assert.ok(divs.length > 0, '<div> surrounds full name label and input');
   let div = divs[0];
   let label = div.querySelector('label');
   assert.ok(label !== null, '<label> in div exists');
   assert.equal(label.innerText, 'Full name:', '<label> has correct text');
   assert.equal(label.getAttribute('for'), 'fullName', '<label> has correct for attribute value');
});

QUnit.test('Test full name input', function(assert) {
   let divs = document.querySelectorAll('form div');
   assert.ok(divs.length > 0, '<div> surrounds full name label and input');
   let div = divs[0];
   let input = div.querySelector('input');
   assert.ok(input !== null, '<input> in div exists');
   assert.equal(input.name, 'fullName', '<input> has correct name');
   assert.equal(input.id, 'fullName', '<input> has correct id');
   assert.ok(input.getAttribute("required") !== null, '<input> uses required attribute');
});

QUnit.test('Test email label', function(assert) {
   let divs = document.querySelectorAll('form div');
   assert.ok(divs.length > 1, '<div> surrounds email label and input');
   let div = divs[1];
   let label = div.querySelector('label');
   assert.ok(label !== null, '<label> in div exists');
   assert.equal(label.innerText, 'Email address:', '<label> has correct text');
   assert.equal(label.getAttribute('for'), 'email', '<label> has correct for attribute value');
});

QUnit.test('Test email input', function(assert) {
   let divs = document.querySelectorAll('form div');
   assert.ok(divs.length > 1, '<div> surrounds email label and input');
   let div = divs[1];
   let input = div.querySelector('input');
   assert.ok(input !== null, '<input> in div exists');
   assert.equal(input.name, 'email', '<input> has correct name');
   assert.equal(input.id, 'email', '<input> has correct id');
   assert.ok(input.getAttribute("required") !== null, '<input> uses required attribute');
});

QUnit.test('Test about label', function(assert) {
   let divs = document.querySelectorAll('form div');
   assert.ok(divs.length > 2, '<div> surrounds about label and textarea');
   let div = divs[2];
   let label = div.querySelector('label');
   assert.ok(label !== null, '<label> in div exists');
   assert.equal(label.innerText, 'About you:', '<label> has correct text');
   assert.equal(label.getAttribute('for'), 'about', '<label> has correct for attribute value');
});

QUnit.test('Test about textarea', function(assert) {
   let divs = document.querySelectorAll('form div');
   assert.ok(divs.length > 2, '<div> surrounds email label and input');
   let div = divs[2];
   let textarea = div.querySelector('textarea');
   assert.ok(textarea !== null, '<textarea> in div exists');
   assert.equal(textarea.name, 'about', '<textarea> has correct name');
   assert.equal(textarea.id, 'about', '<textarea> has correct id');
   assert.equal(textarea.getAttribute('cols'), 50, '<textarea> has correct cols attribute');
   assert.equal(textarea.getAttribute('rows'), 3, '<textarea> has correct rows attribute');
});

QUnit.test('Test picture label', function(assert) {
   let divs = document.querySelectorAll('form div');
   assert.ok(divs.length > 3, '<div> surrounds picture label and textarea');
   let div = divs[3];
   let label = div.querySelector('label');
   assert.ok(label !== null, '<label> in div exists');
   assert.equal(label.innerText, 'Photo:', '<label> has correct text');
   assert.equal(label.getAttribute('for'), 'picture', '<label> has correct for attribute value');
});

QUnit.test('Test for file input', function(assert) {
   let divs = document.querySelectorAll('form div');
   assert.ok(divs.length > 3, '<div> surrounds picture label and input');
   let div = divs[3];
   let input = div.querySelector('input');
   assert.ok(input !== null, '<input> in div exists');
   assert.equal(input.name, 'picture', '<input> has correct name');
   assert.equal(input.id, 'picture', '<input> has correct id');
   assert.equal(input.getAttribute('type'), 'file', '<input> has correct type attribute');
});

QUnit.test('Test for submit button', function(assert) {
    let divs = document.querySelectorAll('form div');
   assert.ok(divs.length > 4, '<div> surrounds submit input');
   let div = divs[4];
   let input = div.querySelector('input');
   assert.ok(input !== null, '<input> in div exists');
   assert.equal(input.getAttribute('type'), 'submit', '<input> has correct type attribute');
});

```

## Task 3 - Flipgrid (10pts)

Show off your website to the class. Once you have everything ready create a video using flipgrid!

- [How to record your screen](https://help.flip.com/hc/en-us/articles/360045940833-Screen-Recording-How-to-record-your-screen-using-the-Flipgrid-camera)
- [Flipgrid topic]({{site.data.semester-info.flip[page.slug]}})

You need to demo the following:

- Show the completed page(s).
- Show your unit tests passing

## Task 4 - Complete the Retrospective (5pts)

Once you have completed all the tasks open the file **Retrospective.md** and complete each section with a TODO comment.

## Task 5 - Add, Commit, Push your code

Once you are finished you need to make sure that you have pushed all your code to GitHub for grading!