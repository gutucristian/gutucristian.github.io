A collection of my JavaScript notes, examples, and guides.

First, a list of my favorite resource:
- https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md
- https://github.com/getify/You-Dont-Know-JS

# About placing `script` tag

The **modern approach** is to use either `async` or `defer` attributes on `script` tag and place the tag in the `head` of the `html` document. `async` tells the browser it is safe to continue parsing while the script is being downloaded. If we have multiple scripts with the `async` attribute they are downloaded "asynchronously" (i.e., in parallel). Note, the browser is not blocked while the scripts are being downloaded, so the `DOM` construction is not disturbed. `defer` also downloads the scripts asynchronously but _one at a time_ and in the order that they are placed (i.e., first `script` 1 then `script` 2).

The **old approach** is to place the `script` either in the `head` section or at the end of the `body` section. The reasoning for placing it in the `head` is to have all the scripts download before the `DOM` is built. This way, the `script` may insert its own `HTML` in the `DOM` before it is built. The reasoning for placing it at the end of the `body` section is that the DOM is build before the browser blocks to first download the script. A long script may take some time to download leaving the page looking static with the impression that nothing is happening. This would create a bad UX and may lead the user to leave.

See [source](https://stackoverflow.com/questions/436411/where-should-i-put-script-tags-in-html-markup).

# Do you need "text/javascript" specified in your `script` tags?

This attribute is optional. Since Netscape 2, the default programming language in all browsers has been `JavaScript`. HTML5 doesn't need the `type="text/javascript"` (it's the default).

# What is a polyfill?

The term polyfill itself refers to some code that "allows you to have some specific functionality that you expect in current or “modern” browsers to also work in other browsers that do not have the support for that functionality built in.

For example, the `sessionstorage` property, which stores data for a given user session, is something that’s new in `HTML5`. Let’s say that we want to check to see if that property is available “natively” (which means built into) in the browser. So, we can write some JavaScript code like this to check to see if the `sessionstorage` property is defined:

```
/*
  we define the isThereSessionStorage variable
  which will store either true or false
*/

var isThereSessionStorage = (function() {
  try {
    return typeof window.sessionStorage !== 'undefined';
  } catch (e) {
    return false;
  }
})(); 

if(!isThereSessionStorage) {
  // our polyfill code goes here.... 
}
```

Sources:
1. https://stackoverflow.com/questions/7087331/what-is-the-meaning-of-polyfills-in-html5
2. https://www.programmerinterview.com/html5/html5-polyfill/
