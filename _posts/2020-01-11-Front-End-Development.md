A collection of my JavaScript, HTML, and CSS notes, examples, and guides.

# JavaScript

First, a list of resource:
- https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md
- https://github.com/getify/You-Dont-Know-JS

## About placing `script` tag

The **modern approach** is to use either `async` or `defer` attributes on `script` tag and place the tag in the `head` of the `html` document. 

**Async**

With `async`, the file gets **downloaded asynchronously** and then **executed as soon as it’s downloaded**. `async` tells the browser it is safe to continue parsing while the script is being downloaded. If we have multiple scripts with the `async` attribute they are downloaded "asynchronously" (i.e., in parallel). Note, the browser is not blocked while the scripts are being downloaded, so the `DOM` construction is not disturbed.

**Defer**

With `defer`, the file gets downloaded **asynchronously**, but **executed only when the document parsing is completed**. With `defer`, scripts will **execute in the same order as they are called**. This makes `defer` the attribute of choice when a script depends on another script. For example, if you’re using `jQuery` as well as other scripts that depend on it, you’d use defer on them (`jQuery` included), making sure to call `jQuery` before the dependent scripts.

The **old approach** is to place the `script` tag either in the `head` section or at the end of the `body` section. 

The reasoning for placing it in the `head` is to have all the scripts download **before** the `DOM` is built. This way, the `script` may insert its own `HTML` in the `DOM` __before__ it is built. The reasoning for placing it at the end of the `body` section is that the DOM is built before the browser blocks to first download the script. A long script may take some time to download leaving the page looking static with the impression that nothing is happening. This would create a bad UX and may lead the user to leave.

See [source](https://stackoverflow.com/questions/436411/where-should-i-put-script-tags-in-html-markup).

## Do you need `"text/javascript"` specified in your `script` tags?

This attribute is optional. Since Netscape 2, the default programming language in all browsers has been `JavaScript`. HTML5 doesn't need the `type="text/javascript"` (it's the default).

## Safe Navigation Operator (i.e., "Elvis Operator")
Allows you to safely access properties of objects without getting a `Uncaught TypeError`. For example `null?.firstName` will return `undefined`. _It will __not__ raise an error!_

## Destructuring JavaScript Objects
{% highlight javascript linenos %}
const person = {
  first: 'Wes',
  last: 'Bos',
  country: 'Canada',
  city: 'Hamilton',
  twitter: '@wesbos'
};
const { first, last } = person; 
{% endhighlight %}

Instead of the annoying:

{% highlight javascript linenos %}
const first = person.first;
const last = person.last;
{% endhighlight %}

## What is a polyfill?
The term polyfill itself refers to some code that "allows you to have some specific functionality that you expect in current or “modern” browsers to also work in other browsers that do not have the support for that functionality built in.

For example, the `sessionstorage` property, which stores data for a given user session, is something that’s new in `HTML5`. Let’s say that we want to check to see if that property is available “natively” (which means built into) in the browser. So, we can write some JavaScript code like this to check to see if the `sessionstorage` property is defined:

{% highlight javascript linenos %}
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
{% endhighlight %}

Sources:
1. https://stackoverflow.com/questions/7087331/what-is-the-meaning-of-polyfills-in-html5
2. https://www.programmerinterview.com/html5/html5-polyfill/

**null vs undefined**

`undefined`: used when a variable has been declared but has not yet been assigned a value

`null`: null is an assignment value -- it can be assigned to a variable as a representation of no value.

`undefined` is a type itself (`undefined`) and `null` is an object. Unassigned variables are initialized by JavaScript with a default value of `undefined`. JavaScript never sets a value to `null`. That must be done programmatically [source](https://blog.ajaymatharu.com/javascript-difference-between-undefined-and-null/).

# Angular 8

Below is my personal notes regarding the Angular Web framework (namely, Angular 8).

## Getting Started

First, we need `Node.js`. `Node.js`, is used by the Angular command line interface (CLI) behind the scenes to bundle and optimize our project. We will also use the node package manager (`npm`) to manage the different dependencies that Angular has (e.g., the Angular framework itself and other libraries that the framework uses).

To install `Node.js` visit the `Node.js` website and download the long term support (lts) release. In my case this is `12.14.1`.

This will install a tarball in your `~/Downloads` folder with a name approximately similar to `node-v12.14.1-linux-x64.tar.xz`. To be able to call the `Node.js` interpreter and run `Node.js` code from [Bash](https://www.gnu.org/software/bash/manual/html_node/What-is-Bash_003f.html) we must add the `Node.js` interpreter executable to our `$PATH`. The `$PATH` variable specifies the directories in which executable programs are located on the machine that can be started without knowing and typing the whole path to the file on the command line. To do this:

1. Unpack and decompress `node-v12.14.1-linux-x64.tar.xz`:

    `tar xf node-v12.14.1-linux-x64.tar.xz`

2. Now you will see `node-v12.14.1-linux-x64` in your current directory. Navigate to `/usr/local/lib/` and make `nodejs` directory:

    `cd /usr/local/lib/ && mkdir nodejs`

3. Move `node-v12.14.1-linux-x64` to `/usr/local/lib/`:

    `mv ~/Downloads/node-v12.14.1-linux-x64 .`

4. Export `/usr/local/lib/nodejs/bin` which contains `node` executable to `$PATH` in `~/.bashrc`:

    append `export PATH=$PATH:/usr/local/lib/nodejs/node-v12.14.1-linux-x64/bin` to `~/.bash_rc`

**Note 1:** it is recommended to append after existing `$PATH`.

**Note 2:** `~/.bash_rc` is executed by `bash` on **non-login** shells.

## Directives

Directives are instructions in the DOM (document object model). For example, `<p appTurnGreen>Receives a green background!</p>`. Directives are usually added with an attribute selector. Interestingly, Angular components are directives!

A directive has to be defined (unless it is provided by Angular).

#### `*ngIf`

`*ngIf` is prefixed with `*` because it is a _structural directive_ which means that it changes the structure of the `DOM`. The purpose of `*ngIf` is to dynamically add or not add something to the `DOM`. If the element is not there, it is not hidden, it's just not there. E.g.: `<p *ngIf="serverCreated">Server {{ serverName }} was created }}</p>` will display the name of the server that was created when it was created. The expression between the `""` in `*ngIf` needs to be an expression that evaluates to `True` or `False`. It could be a variable, a function, or some inline code.

#### `*ngIf` with `else`

The easiest way to construct an `*ngIf` with an `else` is to use the regular `*ngIf` and negate the conditional expression. For example, `<p *ngIf="!serverCreated">No server created.</p>`. An alternative is to use an `ng-template` with local reference (marked with the prefix `#`) and then refer to it in the `else` section of the `*ngIf` directive. For example:

```
  <p *ngIf="serverCreated; else noServer">Server {{ serverName }} was created }}</p>
  <ng-template #noServer>
    <p>No server was created!</p>
  </ng-template>
```

# CSS

- `class` selector is defined by full stop “.”
- `id` selector is a defined by the name of the id preceded by the pound symbol (“#”)

# HTML

HTML structures the webpage, identifying its elements such as paragraphs, headings, and lists.

## `section` tag

`<section>` means that the content inside is grouped (i.e. relates to a single theme), and should appear as an entry in an outline of the page.

`<div>`, on the other hand, does not convey any meaning, aside from any found in its class, lang and title attributes. A `div` is a generic container for flow content, which does not inherently represent anything. It can be used to group elements for styling purposes

In that vein, a div is relevant only from a pure CSS or DOM perspective, whereas a section is relevant also for semantics and, in a near future, for indexing by search engines.

Source: https://stackoverflow.com/questions/6939864/what-is-the-difference-between-section-and-div

## `template` tag

The HTML Content Template `<template>` element is a mechanism for holding `HTML` that is not to be rendered immediately when a page is loaded but **may be instantiated subsequently during runtime using JavaScript.**

Think of templates as a content fragment that is being stored for subsequent use in the document. We use JavaScript to populate this template with data and eventually insert it dynamically at runtime into some container. Fundamentally, templates allow us to define the general structure of something we may want to insert into the DOM and we only need to use JavaScript to copy this template (using `template.content.cloneNode`), populate it with real data, and inject it into the DOM (hence why it is not rendered by default). This is easier than building the template HTML structure manually using JavaScript in addition to the other steps we have to take to add it into the DOM. Read more on MDN [here](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/template).

## `UTF-8` in `HTTP` Headers

HTTP is a well-known hypertext protocol for data transfer. HTTP messages are encoded with ISO-8859-1 (which can be nominally considered as an enhanced ASCII version, containing umlauts, diacritic and other characters of West European languages). At the same time, the message body can use another encoding assigned in `Content-Type` header.

Nowadays, Unicode – a universal character set, defining all the characters necessary to write the majority of languages – has become a standard, no matter what platform, device, application or language you’re targeting. UTF-8 is one of the Unicode encodings and the one that should be used for Web content according to the W3C.

The easiest ways to specify a charset in an `HTML` page is to put in a `<meta>` tag in the `<head>` element:

`<meta charset="utf-8">`

Declaring a character set this way requires certain constraints to be respected, one of them being that the element containing the character encoding declaration must be serialized completely within the first `1024` bytes of the document, to ensure that the browser will receive the information with the first IP packets transiting through the network and can use it to decode the rest of the document. As the charset `<meta>` tag is the only one with this kind of requirement, the most common tip is to place it directly after the element opening tag:

```
<html …>
  <head …>
    <meta charset="utf-8">
```

Alternatively, the `Content-Type` entity header is used to indicate the media type of the resource.

In **responses**, a `Content-Type` header tells the client what the content type of the returned content actually is. Browsers will do `MIME` sniffing in some cases and will not necessarily follow the value of this header; to prevent this behavior, the header `X-Content-Type-Options` can be set to `nosniff`.

In **requests**, (such as `POST` or `PUT`), the client tells the server what type of data is actually sent.
