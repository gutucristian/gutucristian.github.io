A collection of my JavaScript, HTML, and CSS notes, examples, and guides.

# JavaScript

First, a list of resource:
- https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md
- https://github.com/getify/You-Dont-Know-JS

## About placing `script` tag

The **modern approach** is to use either `async` or `defer` attributes on `script` tag and place the tag in the `head` of the `html` document. `async` tells the browser it is safe to continue parsing while the script is being downloaded. If we have multiple scripts with the `async` attribute they are downloaded "asynchronously" (i.e., in parallel). Note, the browser is not blocked while the scripts are being downloaded, so the `DOM` construction is not disturbed. `defer` also downloads the scripts asynchronously but _one at a time_ and in the order that they are placed (i.e., first `script` 1 then `script` 2).

The **old approach** is to place the `script` either in the `head` section or at the end of the `body` section. The reasoning for placing it in the `head` is to have all the scripts download before the `DOM` is built. This way, the `script` may insert its own `HTML` in the `DOM` before it is built. The reasoning for placing it at the end of the `body` section is that the DOM is build before the browser blocks to first download the script. A long script may take some time to download leaving the page looking static with the impression that nothing is happening. This would create a bad UX and may lead the user to leave.

See [source](https://stackoverflow.com/questions/436411/where-should-i-put-script-tags-in-html-markup).

## Do you need `"text/javascript"` specified in your `script` tags?

This attribute is optional. Since Netscape 2, the default programming language in all browsers has been `JavaScript`. HTML5 doesn't need the `type="text/javascript"` (it's the default).

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

{% highlight html linenos %}
  <p *ngIf="serverCreated; else noServer">Server {{ serverName }} was created }}</p>
  <ng-template #noServer>
    <p>No server was created!</p>
  </ng-template>
{% endhighlight %}
