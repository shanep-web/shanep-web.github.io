---
title: Lab 4 (ch4)
tag: css
layout: lab
points: 50
---

## Overview

In this lab you are going to style an existing web page to make something look fancy!

## Videos

- [VSCode and Git Installed](https://www.youtube.com/watch?v=lix7G-S8ox8)
- [Class Coding standards](https://shanep-web.github.io/docs/coding-standards.html)
- [Cloning and Submitting](https://shanep-web.github.io/docs/github.html)

## Task 1 - Write CSS (25pts)

Create an external stylesheet so the provided HTML produces the following web page:

![Image of the page you will create](../assets/images/labs/lab4.png)

The following style rules should be added to styles.css:

- The default fonts (in order of priority) should be Roboto, Helvetica, and sans-serif
- The heading "Comparison of Dealer Incentives and Loan Offers" should be 16px font using color #444444 and have centered text
- Define a container class whose contents is centered, has a 5 pixel margin, and defines a font size of 13px
- Each offer is defined in a `<div>` tag with the class offer. Define the offer class such that:
  - Each offer is displayed inline-block, with a fixed width of 400 pixels, a 4 pixel margin, and centered text
  - Each offer has a 1 pixel solid border using color #cccccc with a radius of 1 pixel
  - Each offer uses a shadow defined as:
    - `box-shadow: 0px 2px 2px #dddddd, 0px 0px 5px #dddddd;`
- The offer title is defined in a `<div>` tag with offerTitle class. Define the offerTitle class such that:
  - The title uses 14px bold font with color #ffffff
  - The title's background is colored #00b3b3
  - The title has no margin and 15 pixels of padding
- The offer contents are defined in a `<div>` tag with class offerContents. Define the offerContents class such that:
  - Each offer contents div has 20 pixels of padding, a 2 pixel border radius for the bottom border, and background colored #fafafa
- Define CSS rules such that all tables have the following styling:
  - The table should have a width of 100%, background colored #ffffff, no border spacing, a shadow defined as:
    - `box-shadow: 0px 2px 2px #dddddd, 0px 0px 5px #dddddd;`
  - Header cells should use a 12px bold font with color #777777
  - Data cells should use a font lighter than normal weight with color #444444
  - Header cells should have 10 pixels of padding and no margin
  - Data cells should have 10 pixels of padding on the left and right, 20 pixels of padding on top and bottom, and no margin
  - Both header cells and data cells should left align text
  - Both header cells and data cells should have a solid bottom border of 1 pixel using color #f1f1f1
- Define a class finalValue that defines a bold font weight with color #2222ff

Below is the html page that you will be styling. You will need to adapt it to work with the provided starter code.

```html
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>Auto Purchase Offer and Loan Comparison</title>
   <link href="TODO" rel="stylesheet">
</head>

<body>
 <div class="container">
      <h1>Comparison of Dealer Incentives and Loan Offers</h1>
  <div class="offer">
     <div class="offerTitle">Purchase Offer 1</div>
     <div class="offerContents">
     <table>
        <tr>
           <th>Item</th>
           <th>Cost</th>
        </tr>
        <tr>
           <td>Purchase price:</td>
           <td>$33,500</td>
        </tr>
        <tr>
           <td>Cash incentive/rebate:</td>
           <td>$0</td>
        </tr>
        <tr>
           <td>Loan term (months):</td>
           <td>48</td>
        </tr>
        <tr>
           <td>Annual percentage rate (APR):</td>
           <td>3.2</td>
        </tr>
        <tr>
           <td>Monthly payment:</td>
           <td>$725.33</td>
        </tr>
        <tr>
           <td>Total cost:</td>
           <td class="finalValue">$34,815.62</td>
        </tr>
     </table>
     </div>
  </div>
  <div class="offer">
     <div class="offerTitle">Purchase Offer 2</div>
     <div class="offerContents">
     <table>
        <tr>
           <th>Item</th>
           <th>Cost</th>
        </tr>
        <tr>
           <td>Purchase price:</td>
           <td>$33,500</td>
        </tr>
        <tr>
           <td>Cash incentive/rebate:</td>
           <td>$500</td>
        </tr>
        <tr>
           <td>Loan term (months):</td>
           <td>48</td>
        </tr>
        <tr>
           <td>Annual percentage rate (APR):</td>
           <td>4.1</td>
        </tr>
        <tr>
           <td>Monthly payment:</td>
           <td>$746.59</td>
        </tr>
        <tr>
           <td>Total cost:</td>
           <td class="finalValue">$35,836.15</td>
        </tr>
     </table>
     </div>
  </div>
 </div>

</body>

</html>
```

## Task 2 - Unit testing (10pts)

Copy the tests below into your testing file and run all the tests. 100% of the tests must pass!

```javascript

QUnit.test('h1 styling', function(assert) {
   // Expect 1 h1 tag.
   const h1Tags = document.getElementsByTagName('h1');
   const expectedStyles = {
      'font-family': 'Roboto, Helvetica, sans-serif',
      'font-size': '16px',
      color: 'rgb(68, 68, 68)',
   };
   const h1Styles = window.getComputedStyle(h1Tags[0], null);

   for (const expectedStyle in expectedStyles) {
      const value = h1Styles.getPropertyValue(expectedStyle);

      assert.equal(value, expectedStyles[expectedStyle], `${expectedStyle} of <h1> tag`);
   }
});

QUnit.test('Background colors', function(assert) {
   assert.equal(window.getComputedStyle(document.getElementsByClassName('offerTitle')[0], null).backgroundColor, 
                'rgb(0, 179, 179)', 'Offer title background color of rgb(0, 179, 179)');
   assert.equal(window.getComputedStyle(document.getElementsByClassName('offerContents')[0], null).backgroundColor, 
                'rgb(250, 250, 250)', 'Offer contents background color of rgb(250, 250, 250)');
   assert.equal(window.getComputedStyle(document.getElementsByTagName('table')[0], null).backgroundColor, 
                'rgb(255, 255, 255)', 'Table background of rgb(255, 255, 255)');
   assert.equal(window.getComputedStyle(document.getElementsByTagName('table')[0], null).backgroundColor, 
                'rgb(255, 255, 255)', 'Table background of rgb(255, 255, 255)');
});

QUnit.test('Display style for offer divs', function(assert) {
   assert.equal(window.getComputedStyle(document.getElementsByClassName('offer')[0], null).display, 
                'inline-block', 'First offer div style of inline-block');
   assert.equal(window.getComputedStyle(document.getElementsByClassName('offer')[1], null).display, 
                'inline-block', 'Second offer div style of inline-block');
});

QUnit.test('Border radii', function(assert) {
   const offerClass = document.getElementsByClassName('offer');
   const offerStyles = window.getComputedStyle(offerClass[0], null);

   const offerExpectedStyles = {
      'border-bottom-right-radius': '1px',
      'border-bottom-left-radius': '1px',
      'border-top-right-radius': '1px',
      'border-top-left-radius': '1px',
   };

   for (const expectedStyle in offerExpectedStyles) {
      const value = offerStyles.getPropertyValue(expectedStyle);

      assert.equal(value, offerExpectedStyles[expectedStyle], `${expectedStyle} of offer class`);
   }

   const offerContentsClass = document.getElementsByClassName('offerContents');
   const offerContentsStyles = window.getComputedStyle(offerContentsClass[0], null);

   const offerContentsExpectedStyles = {
      'border-bottom-right-radius': '2px',
      'border-bottom-left-radius': '2px',
      'border-top-right-radius': '0px',
      'border-top-left-radius': '0px',
   };

   for (const expectedStyle in offerContentsExpectedStyles) {
      const value = offerContentsStyles.getPropertyValue(expectedStyle);

      assert.equal(value, offerContentsExpectedStyles[expectedStyle], `${expectedStyle} of offerContents class`);
   }
});

QUnit.test('Box shadows', function(assert) {
   const expectedBoxShadowStyle = 'rgb(221, 221, 221) 0px 2px 2px 0px, rgb(221, 221, 221) 0px 0px 5px 0px';

   // Check for box shadow on desired elements
   assert.equal(window.getComputedStyle(document.getElementsByClassName('offer')[0], null).boxShadow, 
                expectedBoxShadowStyle, 'offer class box shadow of ' + expectedBoxShadowStyle);
   assert.equal(window.getComputedStyle(document.getElementsByTagName('table')[0], null).boxShadow, 
                expectedBoxShadowStyle, 'table  box shadow of ' + expectedBoxShadowStyle);

   // Check for no box shadow on one other element
   assert.equal(window.getComputedStyle(document.getElementsByClassName('container')[0], null).boxShadow, 
                'none', 'No box shadow for container class');
});

QUnit.test('Margins and padding', function(assert) {
   assert.equal(window.getComputedStyle(document.getElementsByClassName('container')[0], null).margin, 
                '5px', 'Margin for container class');
   assert.equal(window.getComputedStyle(document.getElementsByClassName('offer')[0], null).margin, 
                '4px', 'Margin for offer class');
   assert.equal(window.getComputedStyle(document.getElementsByTagName('th')[0], null).margin, 
                '0px', 'Margin for <th>');
   assert.equal(window.getComputedStyle(document.getElementsByTagName('td')[0], null).margin, 
                '0px', 'Margin for <td>');

   assert.equal(window.getComputedStyle(document.getElementsByClassName('offerTitle')[0], null).padding, 
                '15px', 'Padding for offerTitle class');
   assert.equal(window.getComputedStyle(document.getElementsByClassName('offerContents')[0], null).padding, 
                '20px', 'Padding for offerContents class');
   assert.equal(window.getComputedStyle(document.getElementsByTagName('th')[0], null).padding, 
                '10px', 'Padding for <th>');
   assert.equal(window.getComputedStyle(document.getElementsByTagName('td')[0], null).padding, 
                '20px 10px', 'Padding for <td>');
});

QUnit.test('<th> font colors, sizes, and weights', function(assert) {
   const thTags = document.getElementsByTagName('th');
   const thStyles = window.getComputedStyle(thTags[0], null);
   const expectedThStyles = {
      'font-size': '12px',
      color: 'rgb(119, 119, 119)',
   };

   for (const expectedStyle in expectedThStyles) {
      const value = thStyles.getPropertyValue(expectedStyle);

      assert.equal(value, expectedThStyles[expectedStyle], `${expectedStyle} of <th> tag`);
   }
   assert.ok(thStyles.fontWeight > 400, 'Bold font weight for <th>.');
});

QUnit.test('<td> font colors, sizes, and weights', function(assert) {
   const tdTags = document.getElementsByTagName('td');
   const tdStyles = window.getComputedStyle(tdTags[0], null);

   const expectedTdStyles = {
      'font-size': '13px', 
      color: 'rgb(68, 68, 68)',
   };

   for (const expectedStyle in expectedTdStyles) {
      const value = tdStyles.getPropertyValue(expectedStyle);

      assert.equal(value, expectedTdStyles[expectedStyle], `${expectedStyle} of <td> tag`);
   }
   assert.ok(tdStyles.fontWeight < 400, 'Ligher font weight for <td>.');
});

QUnit.test('Text alignment', function(assert) {
   assert.equal(window.getComputedStyle(document.getElementsByTagName('h1')[0], null).textAlign, 
                'center', '<h1> text align is center');
   assert.equal(window.getComputedStyle(document.getElementsByTagName('table')[0], null).textAlign, 
                'center', 'Offer <table> text align is center');
   assert.equal(window.getComputedStyle(document.getElementsByTagName('th')[0], null).textAlign, 
                'left', '<th> text align is left');
   assert.equal(window.getComputedStyle(document.getElementsByTagName('td')[0], null).textAlign, 
                'left', '<td> text align is center');
});

QUnit.test('finalValue class styling', function(assert) {
   const finalValueStyles = window.getComputedStyle(document.getElementsByClassName('finalValue')[0], null);

   assert.ok(finalValueStyles.fontWeight > 400, 'Bold font weight for finalValue class.');
   assert.equal(finalValueStyles.color, 'rgb(34, 34, 255)', 'Text color of rgb(34, 34, 255) for finalValue class');
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
