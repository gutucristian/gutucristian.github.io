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

## Converting numbers between different bases

- https://stackoverflow.com/questions/1337419/how-do-you-convert-numbers-between-different-bases-in-javascript

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

## `HTTP` Request & Response Encoding

`HTTP` is a protocol for data transfer used by the World Wide Web. 

`HTTP` [headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers) must be encode encoded with `ISO-8859-1` (which can be nominally considered as an enhanced `ASCII` version, containing umlauts, diacritic and other characters of West European languages). This allows browsers to know, ahead of time, how to decode an incoming request's headers which (among other things) specifies the request `Content-Type`.

`Content-Type` is an `HTTP` request header that is used to indicate the media type of the resource -- the browser needs this in order to know how to parse the data. Example:

{% highlight html linenos %}
  Content-Type: text/html; charset=UTF-8
  Content-Type: multipart/form-data; boundary=something
{% endhighlight %}

Read: https://blog.dareboost.com/en/2018/11/content-encoding-meta-charset-content-type-header/

Sometimes the browser will perform "`MIME` sniffing" to guess the `Content-Type` if it is not present or it _may_ not follow the value of the header; to prevent this behavior, the header `X-Content-Type-Options` can be set to `nosniff` ([source](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type)).

## Charset detection

Character encoding detection, charset detection, or code page detection is the process of heuristically guessing the character encoding of a series of bytes that represent text. The technique is recognised to be unreliable and is only used when specific metadata, such as a HTTP Content-Type: header is either not available, or is assumed to be untrustworthy.

This algorithm usually involves statistical analysis of byte patterns, like frequency distribution of trigraphs of various languages encoded in each code page that will be detected; such statistical analysis can also be used to perform language detection. This process is not foolproof because it depends on statistical data.

Source: https://en.wikipedia.org/wiki/Charset_detection#:~:text=Character%20encoding%20detection%2C%20charset%20detection,of%20bytes%20that%20represent%20text.&text=However%2C%20badly%20written%20charset%20detection,8%20is%20some%20other%20encoding.

## How does the browser know which decoding standard to use?

Nowadays, (Unicode)[https://www.w3.org/International/articles/definitions-characters/#httpheader] – a universal character set, defining all the characters necessary to write the majority of languages – has become a standard, no matter what platform, device, application or language you’re targeting. `UTF-8` is one of the Unicode encodings and the one that should be used for web content according to the World Wide Web Committee (W3C).

If the response is an `HTML` document, then there are atleast two ways to specify its encoding and, thus, how it should be decoded.

First, and probably easiest, it to specify a `charset` in the `HTML` page itself in the `<meta>` tag in the `<head>` element:

`<meta charset="utf-8">`

However, declaring a character set this way requires certain constraints to be respected. One constratint is that the element containing the character encoding declaration must be serialized completely within the first `1024` bytes of the document, to ensure that the browser will receive the information with the first packet transiting through the network. This is crucial, so that the browser knows how to decode the rest of the document (which may contain characters encoded using some specific standard). As the charset `<meta>` tag is the only one with this kind of requirement, the most common tip is to place it directly after the element opening tag:

{% highlight html linenos %}
  <html …>
    <head …>
      <meta charset="utf-8">
{% endhighlight %}

This works because up to the first `1024` bytes the browser will decode assuming `ASCII`. 

A second more general way to specify the encoding is via the `Content-Type` `HTTP`.

Values for this header may include `UTF-8 `and `UTF-16` encoding standards (among others). Both are methods to encode Unicode strings to byte sequences.

What happens if `Content-Type` and `<meta>` tag with `charset` is missing?

- If the page starts with a UTF-8 or UTF-16 Byte order mark (BOM), then the encoding is taken from that. This happens before and in preference to looking at the HTTP header and the <meta> elements ([source](https://stackoverflow.com/questions/18794465/how-does-a-browser-interpret-a-page-with-no-charset-in-the-response-header-or-me)).

- If there's no BOM either, then it will read some amount of the HTML code from the page and then try to guess the encoding. If it cannot figure it out then it will default to the browser's default character set. Depending on the browser it will often be something like Windows-1252 (a superset of Latin-1 also called ISO 8859-1) or UTF-8 ([source](https://stackoverflow.com/questions/18794465/how-does-a-browser-interpret-a-page-with-no-charset-in-the-response-header-or-me)).

Note however that, since the HTTP header has a higher precedence than the in-document meta declarations, content authors should always take into account whether the character encoding is already declared in the HTTP header. If it is, the meta element must be set to declare the same encoding ([source](w3.org/International/questions/qa-html-encoding-declarations)).

## Base64

- https://stackoverflow.com/questions/47973487/how-is-encoded-data-sent-over-a-network
- https://stackoverflow.com/questions/201479/what-is-base-64-encoding-used-for
- https://stackoverflow.com/questions/3538021/why-do-we-use-base64
- https://medium.com/swlh/powering-the-internet-with-base64-d823ec5df747
- https://stackoverflow.com/questions/41315553/why-use-base64

`Base64` is a method to encode a byte sequence to a string.

When you have some binary data that you want to ship across a network, you generally don't do it by just streaming the bits and bytes over the wire in a raw format. Why? because some media are made for streaming text. You never know -- some protocols may interpret your binary data as control characters (like a modem), or your binary data could be screwed up because the underlying protocol might think that you've entered a special character combination (like how FTP translates line endings).

**Why 64?**

Because you can generally rely on the same `64` characters being present in many character sets, and you can be reasonably confident that your data's going to end up on the other side of the wire uncorrupted.

So, people use `Base64` for "binary-to-text" encoding.

One example usecase involves encoding image binaries as Base64 so that it can be embedded within the HTML document itself. Example:

{% highlight html linenos %}
  <div>
    <p>Image encoded in HTML as Base64</p>
    <img src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAAUA
      AAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO
          9TXL0Y4OHwAAAABJRU5ErkJggg==" alt="Red dot" />
  </div>
{% endhighlight %}

https://stackoverflow.com/questions/11736159/advantages-and-disadvantages-of-using-base64-encoded-images

`Base64` is an encoding to represent any byte sequence by a sequence of printable characters (i.e. A–Z, a–z, 0–9, +, and /).

From a high level, the encoding works as follows:
1. Take existing data binaries
2. Separate into groups of `6` bits
3. Now, each `6` bit group maps to some character defines in the `Base64` chart 

`Base64` solves these problems by giving us a way to encode aribtrary bytes to bytes which are known to be safe to send without getting corrupted (ASCII alphanumeric characters and a couple of symbols).

To send text reliably you can first encode to bytes using a text encoding of your choice (for example `UTF-8`) and then afterwards `Base64` encode the resulting binary data into a text string that is safe to send encoded as `ASCII`. The receiver will have to reverse this process to recover the original message. This of course requires that the receiver knows which encodings were used, and this information often needs to be sent separately ([source](https://stackoverflow.com/questions/3538021/why-do-we-use-base64)).

`UTF-8` is like the other `UTF` encodings a character encoding to encode characters of the Unicode character set UCS.

See: The Absolute Minimum Every Software Developer Absolutely, Positively Must Know About Unicode and Character Sets (No Excuses!)

Sources: 
1. https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-unicode-and-character-sets-no-excuses/
2. https://stackoverflow.com/questions/3866316/whats-the-difference-between-utf8-utf16-and-base64-in-terms-of-encoding
3. https://dzone.com/articles/utf-8-in-http-headers
4. https://stackoverflow.com/questions/3866316/whats-the-difference-between-utf8-utf16-and-base64-in-terms-of-encoding
5. https://stackoverflow.com/questions/201479/what-is-base-64-encoding-used-for

## HTTP Request and Response headers

The `Content-Type` entity header is used to indicate the media type of the resource.

In **responses**, a `Content-Type` header tells the client what the content type of the returned content actually is. Browsers will do `MIME` sniffing in some cases and will not necessarily follow the value of this header; to prevent this behavior, the header `X-Content-Type-Options` can be set to `nosniff`.

In **requests**, (such as `POST` or `PUT`), the client tells the server what type of data is actually sent.

## UTF-8 vs Unicode

https://stackoverflow.com/questions/643694/what-is-the-difference-between-utf-8-and-unicode

# CORS

https://security.stackexchange.com/questions/170389/http-access-control-cors-purpose

https://stackoverflow.com/questions/29167428/same-origin-policy-and-cors-whats-the-point
