---
title: 'How to write Desktop-first responsive emails'
date: 2016-03-06 19:14:40
excerpt: Long-form articles maximize for time spent and quality of user. Currently, there is no system to allow an ad to be valued longer based on its time viewed. Instead, a 1 second glance and a 5 minute stare are valued the same. As a result, the time spent on an article is not a rewarded by the existing single ad-unit.
---

Most web pages are written with the assumption that all the content is loaded, then the CSS determines the priority for styling. In emails, you have to design your CSS/class/markup base on the assumption that you can have some parts of your code NOT run.

When it comes to responsive layouts, you have to design for the case where desktop is default (without the media queries) and any media queries are added, only if its possible to run inline styles. In the case where you have things like Gmail, where the style tags are stripped out, you need to account for this.

## Inliners
Most email browsers don’t read style tags. As a result, you have to write your emails with all the CSS properties as style tags in the html tag. This is called inlined CSS. Instead of writing the raw HTML with inlined CSS, there are many services or libraries for inlining a block of HTML and CSS.

The problem with email inliners, like Rail’s Roadie, was that it was inlining the media query styles that would run on mobile, which broke the default desktop layout. By having roadie ignore certain styles, we control the designed layout to render the desktop mode in all cases, and then serve the inline style tag with media queries to be run where possible.

To make things more complicated, Outlook is very outdated. Specifically, there are a lot of CSS3 properties and even CSS2 properties it doesn’t render. As a result, outlook has some legacy hacks where you can use comment flags to tell Outlook NOT to render certain content.

## Basics

For responsive emails, where you use media queries, you have to keep a few things in mind.

1. The email is being inlined
2. Email browsers render CSS2 on average
3. You have some email browsers that have comment hacks (ie. outlook)

## Write Desktop-First HTML and CSS

Based on this factor, you have to code your emails to be desktop-first. The CSS, without any media queries, need to look good on desktop email browsers. This way, you can assume at the very least, the email looks like this:

## Add Media Query for mobile CSS

Considering you want to add a media query, you need to add an inline media query on the email. This is pretty straight forward for desktop rendering. For example, you would get something like this:

## Setup your inliner

The problem comes up in emails, the two conditions above don’t work perfectly. For CSS to render in emails, it needs to be inlined. This means that the desktop or media query conditions don’t jive well together. This why we started with the Desktop-First css structure. One you inline your css, when theres a media query, you get this mess:

Consider your inliner to ignore media query for inline

When you have a media query, the media queries CSS gets inlined, and consequently overrides the desktop CSS. As a result, you get markup that looks broken. To prevent this, you have to queue your CSS inliner to ignore the media query CSS. To do this, you separate the media query CSS in a seperate style tag and you flag it in the inliner. In the case for Roadie, thats the `roadie-ignore` flag. This way you get this:

# Wrap up

By setting the inliner to ignore the media query style tag, you allow all email browsers to render the working desktop CSS. In the case that the email browser will parse the style tag, it can view the media query and optionally render the mobile styles when relevant. This is how you can have cross-email-browser functional CSS emails!