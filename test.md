# Heading 1
Vestibulum id ligula porta felis euismod semper. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Maecenas faucibus mollis interdum. Maecenas faucibus mollis interdum. Curabitur blandit tempus porttitor.

Maecenas faucibus mollis interdum. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Cras justo odio, dapibus ac facilisis in, egestas eget quam. Donec id elit non mi porta gravida at eget metus. Donec ullamcorper nulla non metus auctor fringilla. Sed posuere consectetur est at lobortis. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.

Duis mollis, est non commodo luctus, nisi erat porttitor ligula, eget lacinia odio sem nec elit. Sed posuere consectetur est at lobortis. Aenean lacinia bibendum nulla sed consectetur. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec sed odio dui. Integer posuere erat a ante venenatis dapibus posuere velit aliquet.

## Heading 2

Aenean lacinia bibendum nulla sed consectetur. Nullam quis risus eget urna mollis ornare vel eu leo. Donec ullamcorper nulla non metus auctor fringilla. Lorem ipsum dolor sit amet, consectetur adipiscing elit.

### Heading 3

Maecenas sed diam eget risus varius blandit sit amet non magna. Maecenas sed diam eget risus varius blandit sit amet non magna. Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Cras mattis consectetur purus sit amet fermentum.

#### Heading 4

Maecenas sed diam eget risus varius blandit sit amet non magna. Praesent commodo cursus magna, vel scelerisque nisl consectetur et. Aenean lacinia bibendum nulla sed consectetur. Integer posuere erat a ante venenatis dapibus posuere velit aliquet. Integer posuere erat a ante venenatis dapibus posuere velit aliquet.

##### Heading 5

Maecenas sed diam eget risus varius blandit sit amet non magna. Praesent commodo cursus magna, vel scelerisque nisl consectetur et. Aenean lacinia bibendum nulla sed consectetur. Integer posuere erat a ante venenatis dapibus posuere velit aliquet. Integer posuere erat a ante venenatis dapibus posuere velit aliquet.

###### Heading 6

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus sagittis lacus vel augue laoreet rutrum faucibus dolor auctor. Nullam id dolor id nibh ultricies vehicula ut id elit. Nullam quis risus eget urna mollis ornare vel eu leo.


**Example `diff`**

```diff
diff --git a/index.html b/index.html
index c1d9156..7764744 100644
--- a/index.html
+++ b/index.html
@@ -95,7 +95,8 @@ StringStream.prototype = {
     <script>
       var editor = CodeMirror.fromTextArea(document.getElementById("code"), {
         lineNumbers: true,
-        autoMatchBrackets: true
+        autoMatchBrackets: true,
+      onGutterClick: function(x){console.log(x);}
       });
     </script>
   </body>
diff --git a/lib/codemirror.js b/lib/codemirror.js
index 04646a9..9a39cc7 100644
--- a/lib/codemirror.js
+++ b/lib/codemirror.js
@@ -399,10 +399,16 @@ var CodeMirror = (function() {
     }
 
     function onMouseDown(e) {
-      var start = posFromMouse(e), last = start;    
+      var start = posFromMouse(e), last = start, target = e.target();
       if (!start) return;
       setCursor(start.line, start.ch, false);
       if (e.button() != 1) return;
+      if (target.parentNode == gutter) {    
+        if (options.onGutterClick)
+          options.onGutterClick(indexOf(gutter.childNodes, target) + showingFrom);
+        return;
+      }
+
       if (!focused) onFocus();
 
       e.stop();
@@ -808,7 +814,7 @@ var CodeMirror = (function() {
       for (var i = showingFrom; i < showingTo; ++i) {
         var marker = lines[i].gutterMarker;
         if (marker) html.push('<div class="' + marker.style + '">' + htmlEscape(marker.text) + '</div>');
-        else html.push("<div>" + (options.lineNumbers ? i + 1 : "\u00a0") + "</div>");
+        else html.push("<div>" + (options.lineNumbers ? i + options.firstLineNumber : "\u00a0") + "</div>");
       }
       gutter.style.display = "none"; // TODO test whether this actually helps
       gutter.innerHTML = html.join("");
@@ -1371,10 +1377,8 @@ var CodeMirror = (function() {
         if (option == "parser") setParser(value);
         else if (option === "lineNumbers") setLineNumbers(value);
         else if (option === "gutter") setGutter(value);
-        else if (option === "readOnly") options.readOnly = value;
-        else if (option === "indentUnit") {options.indentUnit = indentUnit = value; setParser(options.parser);}
-        else if (/^(?:enterMode|tabMode|indentWithTabs|readOnly|autoMatchBrackets|undoDepth)$/.test(option)) options[option] = value;
-        else throw new Error("Can't set option " + option);
+        else if (option === "indentUnit") {options.indentUnit = value; setParser(options.parser);}
+        else options[option] = value;
       },
       cursorCoords: cursorCoords,
       undo: operation(undo),
@@ -1402,7 +1406,8 @@ var CodeMirror = (function() {
       replaceRange: operation(replaceRange),
 
       operation: function(f){return operation(f)();},
-      refresh: function(){updateDisplay([{from: 0, to: lines.length}]);}
+      refresh: function(){updateDisplay([{from: 0, to: lines.length}]);},
+      getInputField: function(){return input;}
     };
     return instance;
   }
@@ -1420,6 +1425,7 @@ var CodeMirror = (function() {
     readOnly: false,
     onChange: null,
     onCursorActivity: null,
+    onGutterClick: null,
     autoMatchBrackets: false,
     workTime: 200,
     workDelay: 300,
```

**Example `ts`**

```ts
class Greeter {
  greeting: string;
  constructor (message: string) {
    this.greeting = message;
  }
  greet() {
    return "Hello, " + this.greeting;
  }
}   

var greeter = new Greeter("world");

var button = document.createElement('button')
button.innerText = "Say Hello"
button.onclick = function() {
  alert(greeter.greet())
}

document.body.appendChild(button)
```

**Example `markdown`**

```markdown
Markdown: Basics
================

<ul id="ProjectSubmenu">
    <li><a href="/projects/markdown/" title="Markdown Project Page">Main</a></li>
    <li><a class="selected" title="Markdown Basics">Basics</a></li>
    <li><a href="/projects/markdown/syntax" title="Markdown Syntax Documentation">Syntax</a></li>
    <li><a href="/projects/markdown/license" title="Pricing and License Information">License</a></li>
    <li><a href="/projects/markdown/dingus" title="Online Markdown Web Form">Dingus</a></li>
</ul>


Getting the Gist of Markdown's Formatting Syntax
------------------------------------------------

This page offers a brief overview of what it's like to use Markdown.
The [syntax page] [s] provides complete, detailed documentation for
every feature, but Markdown should be very easy to pick up simply by
looking at a few examples of it in action. The examples on this page
are written in a before/after style, showing example syntax and the
HTML output produced by Markdown.

It's also helpful to simply try Markdown out; the [Dingus] [d] is a
web application that allows you type your own Markdown-formatted text
and translate it to XHTML.

**Note:** This document is itself written using Markdown; you
can [see the source for it by adding '.text' to the URL] [src].

  [s]: /projects/markdown/syntax  "Markdown Syntax"
  [d]: /projects/markdown/dingus  "Markdown Dingus"
  [src]: /projects/markdown/basics.text


## Paragraphs, Headers, Blockquotes ##

A paragraph is simply one or more consecutive lines of text, separated
by one or more blank lines. (A blank line is any line that looks like
a blank line -- a line containing nothing but spaces or tabs is
considered blank.) Normal paragraphs should not be indented with
spaces or tabs.

Markdown offers two styles of headers: *Setext* and *atx*.
Setext-style headers for `<h1>` and `<h2>` are created by
"underlining" with equal signs (`=`) and hyphens (`-`), respectively.
To create an atx-style header, you put 1-6 hash marks (`#`) at the
beginning of the line -- the number of hashes equals the resulting
HTML header level.

Blockquotes are indicated using email-style '`>`' angle brackets.

Markdown:

    A First Level Header
    ====================

    A Second Level Header
    ---------------------

    Now is the time for all good men to come to
    the aid of their country. This is just a
    regular paragraph.

    The quick brown fox jumped over the lazy
    dog's back.

    ### Header 3

    > This is a blockquote.
    >
    > This is the second paragraph in the blockquote.
    >
    > ## This is an H2 in a blockquote


Output:

    <h1>A First Level Header</h1>

    <h2>A Second Level Header</h2>

    <p>Now is the time for all good men to come to
    the aid of their country. This is just a
    regular paragraph.</p>

    <p>The quick brown fox jumped over the lazy
    dog's back.</p>

    <h3>Header 3</h3>

    <blockquote>
        <p>This is a blockquote.</p>

        <p>This is the second paragraph in the blockquote.</p>

        <h2>This is an H2 in a blockquote</h2>
    </blockquote>



### Phrase Emphasis ###

Markdown uses asterisks and underscores to indicate spans of emphasis.

Markdown:

    Some of these words *are emphasized*.
    Some of these words _are emphasized also_.

    Use two asterisks for **strong emphasis**.
    Or, if you prefer, __use two underscores instead__.

Output:

    <p>Some of these words <em>are emphasized</em>.
    Some of these words <em>are emphasized also</em>.</p>

    <p>Use two asterisks for <strong>strong emphasis</strong>.
    Or, if you prefer, <strong>use two underscores instead</strong>.</p>



## Lists ##

Unordered (bulleted) lists use asterisks, pluses, and hyphens (`*`,
`+`, and `-`) as list markers. These three markers are
interchangeable; this:

    *   Candy.
    *   Gum.
    *   Booze.

this:

    +   Candy.
    +   Gum.
    +   Booze.

and this:

    -   Candy.
    -   Gum.
    -   Booze.

all produce the same output:

    <ul>
    <li>Candy.</li>
    <li>Gum.</li>
    <li>Booze.</li>
    </ul>

Ordered (numbered) lists use regular numbers, followed by periods, as
list markers:

    1.  Red
    2.  Green
    3.  Blue

Output:

    <ol>
    <li>Red</li>
    <li>Green</li>
    <li>Blue</li>
    </ol>

If you put blank lines between items, you'll get `<p>` tags for the
list item text. You can create multi-paragraph list items by indenting
the paragraphs by 4 spaces or 1 tab:

    *   A list item.

        With multiple paragraphs.

    *   Another item in the list.

Output:

    <ul>
    <li><p>A list item.</p>
    <p>With multiple paragraphs.</p></li>
    <li><p>Another item in the list.</p></li>
    </ul>



### Links ###

Markdown supports two styles for creating links: *inline* and
*reference*. With both styles, you use square brackets to delimit the
text you want to turn into a link.

Inline-style links use parentheses immediately after the link text.
For example:

    This is an [example link](http://example.com/).

Output:

    <p>This is an <a href="http://example.com/">
    example link</a>.</p>

Optionally, you may include a title attribute in the parentheses:

    This is an [example link](http://example.com/ "With a Title").

Output:

    <p>This is an <a href="http://example.com/" title="With a Title">
    example link</a>.</p>

Reference-style links allow you to refer to your links by names, which
you define elsewhere in your document:

    I get 10 times more traffic from [Google][1] than from
    [Yahoo][2] or [MSN][3].

    [1]: http://google.com/        "Google"
    [2]: http://search.yahoo.com/  "Yahoo Search"
    [3]: http://search.msn.com/    "MSN Search"

Output:

    <p>I get 10 times more traffic from <a href="http://google.com/"
    title="Google">Google</a> than from <a href="http://search.yahoo.com/"
    title="Yahoo Search">Yahoo</a> or <a href="http://search.msn.com/"
    title="MSN Search">MSN</a>.</p>

The title attribute is optional. Link names may contain letters,
numbers and spaces, but are *not* case sensitive:

    I start my morning with a cup of coffee and
    [The New York Times][NY Times].

    [ny times]: http://www.nytimes.com/

Output:

    <p>I start my morning with a cup of coffee and
    <a href="http://www.nytimes.com/">The New York Times</a>.</p>


### Images ###

Image syntax is very much like link syntax.

Inline (titles are optional):

    ![alt text](/path/to/img.jpg "Title")

Reference-style:

    ![alt text][id]

    [id]: /path/to/img.jpg "Title"

Both of the above examples produce the same output:

    <img src="/path/to/img.jpg" alt="alt text" title="Title" />



### Code ###

In a regular paragraph, you can create code span by wrapping text in
backtick quotes. Any ampersands (`&`) and angle brackets (`<` or
`>`) will automatically be translated into HTML entities. This makes
it easy to use Markdown to write about HTML example code:

    I strongly recommend against using any `<blink>` tags.

    I wish SmartyPants used named entities like `&mdash;`
    instead of decimal-encoded entities like `&#8212;`.

Output:

    <p>I strongly recommend against using any
    <code>&lt;blink&gt;</code> tags.</p>

    <p>I wish SmartyPants used named entities like
    <code>&amp;mdash;</code> instead of decimal-encoded
    entities like <code>&amp;#8212;</code>.</p>


To specify an entire block of pre-formatted code, indent every line of
the block by 4 spaces or 1 tab. Just like with code spans, `&`, `<`,
and `>` characters will be escaped automatically.

Markdown:

    If you want your page to validate under XHTML 1.0 Strict,
    you've got to put paragraph tags in your blockquotes:

        <blockquote>
            <p>For example.</p>
        </blockquote>

Output:

    <p>If you want your page to validate under XHTML 1.0 Strict,
    you've got to put paragraph tags in your blockquotes:</p>

    <pre><code>&lt;blockquote&gt;
        &lt;p&gt;For example.&lt;/p&gt;
    &lt;/blockquote&gt;
    </code></pre>

## Fenced code blocks (and syntax highlighting)

~~~javascript
for (var i = 0; i < items.length; i++) {
    console.log(items[i], i); // log them
}
~~~
```

**Example `yaml`**


```yaml
--- # Favorite movies
- Casablanca
- North by Northwest
- The Man Who Wasn't There
--- # Shopping list
[milk, pumpkin pie, eggs, juice]
--- # Indented Blocks, common in YAML data files, use indentation and new lines to separate the key: value pairs
  name: John Smith
  age: 33
--- # Inline Blocks, common in YAML data streams, use commas to separate the key: value pairs between braces
{name: John Smith, age: 33}
---
receipt:     Oz-Ware Purchase Invoice
date:        2007-08-06
customer:
    given:   Dorothy
    family:  Gale

items:
    - part_no:   A4786
      descrip:   Water Bucket (Filled)
      price:     1.47
      quantity:  4

    - part_no:   E1628
      descrip:   High Heeled "Ruby" Slippers
      size:       8
      price:     100.27
      quantity:  1

bill-to:  &id001
    street: |
            123 Tornado Alley
            Suite 16
    city:   East Centerville
    state:  KS

ship-to:  *id001

specialDelivery:  >
    Follow the Yellow Brick
    Road to the Emerald City.
    Pay no attention to the
    man behind the curtain.
...
```

**Example `json`**

```json
{
  "@context": {
    "name": "http://schema.org/name",
    "description": "http://schema.org/description",
    "image": {
      "@id": "http://schema.org/image",
      "@type": "@id"
    },
    "geo": "http://schema.org/geo",
    "latitude": {
      "@id": "http://schema.org/latitude",
      "@type": "xsd:float"
    },
    "longitude": {
      "@id": "http://schema.org/longitude",
      "@type": "xsd:float"
    },
    "xsd": "http://www.w3.org/2001/XMLSchema#"
  },
  "name": "The Empire State Building",
  "description": "The Empire State Building is a 102-story landmark in New York City.",
  "image": "http://www.civil.usherbrooke.ca/cours/gci215a/empire-state-building.jpg",
  "geo": {
    "latitude": "40.75",
    "longitude": "73.98"
  }
}
```

**Example `scss`**

```scss
/* Some example SCSS */

@import "compass/css3";
$variable: #333;

$blue: #3bbfce;
$margin: 16px;

.content-navigation {
  #nested {
    background-color: black;
  }
  border-color: $blue;
  color:
    darken($blue, 9%);
}

.border {
  padding: $margin / 2;
  margin: $margin / 2;
  border-color: $blue;
}

@mixin table-base {
  th {
    text-align: center;
    font-weight: bold;
  }
  td, th {padding: 2px}
}

table.hl {
  margin: 2em 0;
  td.ln {
    text-align: right;
  }
}

li {
  font: {
    family: serif;
    weight: bold;
    size: 1.2em;
  }
}

@mixin left($dist) {
  float: left;
  margin-left: $dist;
}

#data {
  @include left(10px);
  @include table-base;
}

.source {
  @include flow-into(target);
  border: 10px solid green;
  margin: 20px;
  width: 200px; }

.new-container {
  @include flow-from(target);
  border: 10px solid red;
  margin: 20px;
  width: 200px; }

body {
  margin: 0;
  padding: 3em 6em;
  font-family: tahoma, arial, sans-serif;
  color: #000;
}

@mixin yellow() {
  background: yellow;
}

.big {
  font-size: 14px;
}

.nested {
  @include border-radius(3px);
  @extend .big;
  p {
    background: whitesmoke;
    a {
      color: red;
    }
  }
}

#navigation a {
  font-weight: bold;
  text-decoration: none !important;
}

h1 {
  font-size: 2.5em;
}

h2 {
  font-size: 1.7em;
}

h1:before, h2:before {
  content: "::";
}

code {
  font-family: courier, monospace;
  font-size: 80%;
  color: #418A8A;
}
```

**Example `r`**

```r
X <- list(height = 5.4, weight = 54)
cat("Printing objects: "); print(X)
print("Accessing individual elements:")
cat(sprintf("Your height is %s and your weight is %s\n", X$height, X$weight))

# Functions:
square <- function(x) {
  return(x * x)
}
cat(sprintf("The square of 3 is %s\n", square(3)))

# In R, the last expression in a function is, by default, what is
# returned. The idiomatic way to write the function is:
square <- function(x) {
  x * x
}
# or, for functions with short content:
square <- function(x) x * x

# Function arguments with default values:
cube <- function(x = 5) x * x * x
cat(sprintf("Calling cube with 2 : %s\n", cube(2))  # will give 2^3
cat(sprintf("Calling cube        : %s\n", cube())   # will default to 5^3.

powers <- function(x) list(x2 = x*x, x3 = x*x*x, x4 = x*x*x*x)

cat("Powers of 3: "); print(powers(3))

# Data frames
df <- data.frame(letters = letters[1:5], '#letter' = 1:5)
print(df$letters)
print(df$`#letter`)

# Operators:
m1 <- matrix(1:6, 2, 3)
m2 <- m1 %*% t(m1)
cat("Matrix product: "); print(m2)

# Assignments:
a <- 1
b <<- 2
c = 3
4 -> d
5 ->> e
```

**Example `python`**

```python
# Literals
1234
0.0e101
.123
0b01010011100
0o01234567
0x0987654321abcdef
7
2147483647
3L
79228162514264337593543950336L
0x100000000L
79228162514264337593543950336
0xdeadbeef
3.14j
10.j
10j
.001j
1e100j
3.14e-10j


# String Literals
'For\''
"God\""
"""so loved
the world"""
'''that he gave
his only begotten\' '''
'that whosoever believeth \
in him'
''

# Identifiers
__a__
a.b
a.b.c

#Unicode identifiers on Python3
# a = x\ddot
a⃗ = ẍ
# a = v\dot
a⃗ = v̇

#F\vec = m \cdot a\vec
F⃗ = m•a⃗ 

# Operators
+ - * / % & | ^ ~ < >
== != <= >= <> << >> // **
and or not in is

#infix matrix multiplication operator (PEP 465)
A @ B

# Delimiters
() [] {} , : ` = ; @ .  # Note that @ and . require the proper context on Python 2.
+= -= *= /= %= &= |= ^=
//= >>= <<= **=

# Keywords
as assert break class continue def del elif else except
finally for from global if import lambda pass raise
return try while with yield

# Python 2 Keywords (otherwise Identifiers)
exec print

# Python 3 Keywords (otherwise Identifiers)
nonlocal

# Types
bool classmethod complex dict enumerate float frozenset int list object
property reversed set slice staticmethod str super tuple type

# Python 2 Types (otherwise Identifiers)
basestring buffer file long unicode xrange

# Python 3 Types (otherwise Identifiers)
bytearray bytes filter map memoryview open range zip

# Some Example code
import os
from package import ParentClass

@nonsenseDecorator
def doesNothing():
    pass

class ExampleClass(ParentClass):
    @staticmethod
    def example(inputStr):
        a = list(inputStr)
        a.reverse()
        return ''.join(a)

    def __init__(self, mixin = 'Hello'):
        self.mixin = mixin

# Python 3.6 f-strings (https://www.python.org/dev/peps/pep-0498/)
f'My name is {name}, my age next year is {age+1}, my anniversary is {anniversary:%A, %B %d, %Y}.'
f'He said his name is {name!r}.'
f"""He said his name is {name!r}."""
f'{"quoted string"}'
f'{{ {4*10} }}'
f'This is an error }'
f'This is ok }}'
fr'x={4*10}\n'
```

**Example `css`**

```css
/* Some example CSS */

@import url("something.css");

body {
  margin: 0;
  padding: 3em 6em;
  font-family: tahoma, arial, sans-serif;
  color: #000;
}

#navigation a {
  font-weight: bold;
  text-decoration: none !important;
}

h1 {
  font-size: 2.5em;
}

h2 {
  font-size: 1.7em;
}

h1:before, h2:before {
  content: "::";
}

code {
  font-family: courier, monospace;
  font-size: 80%;
  color: #418A8A;
}
```

**Example `dockerfile`**

```dockerfile
# Install Ghost blogging platform and run development environment
#
# VERSION 1.0.0

FROM ubuntu:12.10
MAINTAINER Amer Grgic "amer@livebyt.es"
WORKDIR /data/ghost

# Install dependencies for nginx installation
RUN apt-get update
RUN apt-get install -y python g++ make software-properties-common --force-yes
RUN add-apt-repository ppa:chris-lea/node.js
RUN apt-get update
# Install unzip
RUN apt-get install -y unzip
# Install curl
RUN apt-get install -y curl
# Install nodejs & npm
RUN apt-get install -y rlwrap
RUN apt-get install -y nodejs 
# Download Ghost v0.4.1
RUN curl -L https://ghost.org/zip/ghost-latest.zip -o /tmp/ghost.zip
# Unzip Ghost zip to /data/ghost
RUN unzip -uo /tmp/ghost.zip -d /data/ghost
# Add custom config js to /data/ghost
ADD ./config.example.js /data/ghost/config.js
# Install Ghost with NPM
RUN cd /data/ghost/ && npm install --production
# Expose port 2368
EXPOSE 2368
# Run Ghost
CMD ["npm","start"]
```

**Example `less`**

```less
@media screen and (device-aspect-ratio: 16/9) { … }
@media screen and (device-aspect-ratio: 1280/720) { … }
@media screen and (device-aspect-ratio: 2560/1440) { … }

html:lang(fr-be)

tr:nth-child(2n+1) /* represents every odd row of an HTML table */

img:nth-of-type(2n+1) { float: right; }
img:nth-of-type(2n) { float: left; }

body > h2:not(:first-of-type):not(:last-of-type)

html|*:not(:link):not(:visited)
*|*:not(:hover)
p::first-line { text-transform: uppercase }

@namespace foo url(http://www.example.com);
foo|h1 { color: blue }  /* first rule */

span[hello="Ocean"][goodbye="Land"]

E[foo]{
  padding:65px;
}

input[type="search"]::-webkit-search-decoration,
input[type="search"]::-webkit-search-cancel-button {
  -webkit-appearance: none; // Inner-padding issues in Chrome OSX, Safari 5
}
button::-moz-focus-inner,
input::-moz-focus-inner { // Inner padding and border oddities in FF3/4
  padding: 0;
  border: 0;
}
.btn {
  // reset here as of 2.0.3 due to Recess property order
  border-color: #ccc;
  border-color: rgba(0,0,0,.1) rgba(0,0,0,.1) rgba(0,0,0,.25);
}
fieldset span button, fieldset span input[type="file"] {
  font-size:12px;
	font-family:Arial, Helvetica, sans-serif;
}

.rounded-corners (@radius: 5px) {
  border-radius: @radius;
  -webkit-border-radius: @radius;
  -moz-border-radius: @radius;
}

@import url("something.css");

@light-blue:   hsl(190, 50%, 65%);

#menu {
  position: absolute;
  width: 100%;
  z-index: 3;
  clear: both;
  display: block;
  background-color: @blue;
  height: 42px;
  border-top: 2px solid lighten(@alpha-blue, 20%);
  border-bottom: 2px solid darken(@alpha-blue, 25%);
  .box-shadow(0, 1px, 8px, 0.6);
  -moz-box-shadow: 0 0 0 #000; // Because firefox sucks.

  &.docked {
    background-color: hsla(210, 60%, 40%, 0.4);
  }
  &:hover {
    background-color: @blue;
  }

  #dropdown {
    margin: 0 0 0 117px;
    padding: 0;
    padding-top: 5px;
    display: none;
    width: 190px;
    border-top: 2px solid @medium;
    color: @highlight;
    border: 2px solid darken(@medium, 25%);
    border-left-color: darken(@medium, 15%);
    border-right-color: darken(@medium, 15%);
    border-top-width: 0;
    background-color: darken(@medium, 10%);
    ul {
      padding: 0px;  
    }
    li {
      font-size: 14px;
      display: block;
      text-align: left;
      padding: 0;
      border: 0;
      a {
        display: block;
        padding: 0px 15px;  
        text-decoration: none;
        color: white;  
        &:hover {
          background-color: darken(@medium, 15%);
          text-decoration: none;
        }
      }
    }
    .border-radius(5px, bottom);
    .box-shadow(0, 6px, 8px, 0.5);
  }
}
```

**Example `html`**

```html
<html style="color: green">
  <!-- this is a comment -->
  <head>
    <title>HTML Example</title>
  </head>
  <body>
    The indentation tries to be <em>somewhat &quot;do what
    I mean&quot;</em>... but might not match your style.
  </body>
</html>
```

**Example `text/x-vue`**

```text/x-vue
<template>
  <div class="sass">I'm a {{mustache-like}} template</div>
</template>

<script lang="coffee">
  module.exports =
    props: ['one', 'two', 'three']
</script>

<style lang="sass">
.sass
  font-size: 18px
</style>
```

**Example `twig`**

```twig
{% extends "layout.twig" %}
{% block title %}CodeMirror: Twig mode{% endblock %}
{# this is a comment #}
{% block content %}
  {% for foo in bar if foo.baz is divisible by(3) %}
    Hello {{ foo.world }}
  {% else %}
    {% set msg = "Result not found" %}
    {% include "empty.twig" with { message: msg } %}
  {% endfor %}
{% endblock %}
```

**Example `php`**

```php
<?php
$a = array('a' => 1, 'b' => 2, 3 => 'c');

echo "$a[a] ${a[3] /* } comment */} {$a[b]} \$a[a]";

function hello($who) {
	return "Hello $who!";
}
?>
<p>The program says <?= hello("World") ?>.</p>
<script>
	alert("And here is some JS code"); // also colored
</script>
```

**Example `js`**

```js
// Demo code (the actual new parser character stream implementation)

function StringStream(string) {
  this.pos = 0;
  this.string = string;
}

StringStream.prototype = {
  done: function() {return this.pos >= this.string.length;},
  peek: function() {return this.string.charAt(this.pos);},
  next: function() {
    if (this.pos < this.string.length)
      return this.string.charAt(this.pos++);
  },
  eat: function(match) {
    var ch = this.string.charAt(this.pos);
    if (typeof match == "string") var ok = ch == match;
    else var ok = ch && match.test ? match.test(ch) : match(ch);
    if (ok) {this.pos++; return ch;}
  },
  eatWhile: function(match) {
    var start = this.pos;
    while (this.eat(match));
    if (this.pos > start) return this.string.slice(start, this.pos);
  },
  backUp: function(n) {this.pos -= n;},
  column: function() {return this.pos;},
  eatSpace: function() {
    var start = this.pos;
    while (/\s/.test(this.string.charAt(this.pos))) this.pos++;
    return this.pos - start;
  },
  match: function(pattern, consume, caseInsensitive) {
    if (typeof pattern == "string") {
      function cased(str) {return caseInsensitive ? str.toLowerCase() : str;}
      if (cased(this.string).indexOf(cased(pattern), this.pos) == this.pos) {
        if (consume !== false) this.pos += str.length;
        return true;
      }
    }
    else {
      var match = this.string.slice(this.pos).match(pattern);
      if (match && consume !== false) this.pos += match[0].length;
      return match;
    }
  }
};
```

**Example `jsx`**

```jsx
// Code snippets from http://facebook.github.io/react/docs/jsx-in-depth.html

// Rendering HTML tags
var myDivElement = <div className="foo" />;
ReactDOM.render(myDivElement, document.getElementById('example'));

// Rendering React components
var MyComponent = React.createClass({/*...*/});
var myElement = <MyComponent someProperty={true} />;
ReactDOM.render(myElement, document.getElementById('example'));

// Namespaced components
var Form = MyFormComponent;

var App = (
  <Form>
    <Form.Row>
      <Form.Label />
      <Form.Input />
    </Form.Row>
  </Form>
);

// Attribute JavaScript expressions
var person = <Person name={window.isLoggedIn ? window.name : ''} />;

// Boolean attributes
<input type="button" disabled />;
<input type="button" disabled={true} />;

// Child JavaScript expressions
var content = <Container>{window.isLoggedIn ? <Nav /> : <Login />}</Container>;

// Comments
var content = (
  <Nav>
    {/* child comment, put {} around */}
    <Person
      /* multi
         line
         comment */
      name={window.isLoggedIn ? window.name : ''} // end of line comment
    />
  </Nav>
);
```

