---
title: Lab 10 (ch10)
tag: Full Stack
layout: lab
points: 50
---

## Overview

The textbook introduces node.js and express. In order to get prepared for your
final project in this lab you will use the [express application
generator](https://expressjs.com/en/starter/generator.html) to quickly generate
a "hello world" application. Once you have your application generated you can
add database support using any supported database.

You are allowed to **copy/paste** code examples from the express site. It is not
plagiarism to use the sample code that they provide in the documentation!

## Task 1 - Express

Generate an express application.

```bash
npx express-generator --view=pug
```

The generator may install stale updates so lets make sure everything is up to
date before we go any further.

```bash
npm i -g npm-check-updates
ncu -u
npm install
```

You can now start the app:

```bash
DEBUG=myapp:* npm start
```

You can access the website on [localhost:3000](http://localhost:3000/)

## Task 2 - DB

Now lets get a database a installed and working. If you don't have any
preferences on which database to use, SQLite is well supported and cross
platform. [Integration
instructions](https://expressjs.com/en/guide/database-integration.html) are
located on the express website.

```bash
 npm install sqlite3
```

Install a sqlite [viewer for VSCode](https://marketplace.visualstudio.com/items?itemName=qwtel.sqlite-viewer)

## Task 3 - Explore and Compare

Take some time to explore all the files and research how everything is setup.
You can reference the textbook for more information on express and the templates
that they use.

- [basic routing](https://expressjs.com/en/starter/basic-routing.html)
- [serving static files](https://expressjs.com/en/starter/static-files.html)
- [more examples](https://expressjs.com/en/starter/examples.html)

I have created a [complete example](https://github.com/shanep/express-starter)
that you can reference or even use if you wish. It uses liquid templates instead
of pug and gives examples of creating a table, inserting data into a table,
deleting data, and rendering parameters in liquid.

## Task 4 - Add all your files (25pts)

Add all your files to your repo to show that you have worked through the
required material.

## Task 5 - Flipgrid (15pts)

Most of this lab was just configuring everything, so the majority of your grade
will be you showing off everything working in a video. Once you have everything
ready create a video using flipgrid!

- [How to record your screen](https://help.flip.com/hc/en-us/articles/360045940833-Screen-Recording-How-to-record-your-screen-using-the-Flipgrid-camera)
- [Flipgrid topic]({{site.data.semester-info.flip[page.slug]}})

You need to demo the following:

- Show your completed express application with database support
- Talk about how the application is structured

## Task 6 - Complete the Retrospective (10pts)

Once you have completed all the tasks open the file **Retrospective.md** and
complete each section with a TODO comment.
