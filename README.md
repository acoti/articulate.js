# Articulate.js

Articulate.js, a jQuery plugin, enables developers, with as little as one line of code, to create links that allow users to click, sit back, and listen to the browser read aloud the important content on a Web page. In some ways, it can turn a thoughtful essay or article into a mini podcast. And because it uses built-in JavaScript functionality, no browser extensions or other system software is needed.

Read the featured article on [CSS Tricks](https://css-tricks.com/using-the-speech-synthesis-api-to-create-articulate-js/).

Visit the [Articulate.js Web site](http://articulate.purefreedom.com) for more information.


## Most Recent Update

**v1.1.0:** Added methods that retrieve a list of available voices and sets the voice based on the name or language. See CodePen demo and documentation below.


## CodePen Demos

[Basic Functions](http://codepen.io/adamcoti/pen/zoybvQ/)

[Voice Parameters](http://codepen.io/adamcoti/pen/GNPPrY/)

[Get and Set Voices](http://codepen.io/adamcoti/pen/JEYmge/)

[Text Manipulation](http://codepen.io/adamcoti/pen/eBbXJx/)

[HTML Data Attributes](http://codepen.io/adamcoti/pen/bBzdqe/)


## Installation

Simply include the "articulate.min.js" file in your project.


## Usage

Leverage the powerful selector options of jQuery to specify which parts of the Web page that you want spoken. For example, depending on how you have the Web page organized, a single line of code, like the following, can direct Articulate.js to speak the entire contents of an article or blog post:

```javascript
$('article').articulate('speak');
```

Or how about only the primary headers and paragraphs:

```javascript
$('h1,h2,p').articulate('speak');
```

Internally, Articulate.js clones the matched set of elements and all their descendant elements and text nodes. It then parses this clone using a default set of rules, deciding what should be spoken and ignored, then adding the appropriate pauses to make everything sound more like a narrative.


## Documentation

### Basic Functions

| Method | Description |
| ------------- | ------------- |
|` $(selector).articulate('speak');` | Speaks aloud the specified DOM element(s) and their descendants |
|` $().articulate('pause');` | Pauses the speaking |
| `$().articulate('resume');` | Resumes the speaking after it has been paused |
| `$().articulate('stop');` | Stops the speaking permanently |


### Voice Parameters

| Method | Description |
| ------------- | ------------- |
| `$().articulate('rate', number);` | Sets the rate of the speaking voice; default = 1.1; range = [0.1 - 10] |
| `$().articulate('pitch', number);` | Sets the pitch of the speaking voice; default = 1.0; range = [0 - 2] |
| `$().articulate('volume', number);` | Sets the volume of the speaking voice; default = 1.0; range = [0 - 1] |

**Note:** Omitting number resets the parameter to its default value; changes take effect only when next speak call is made


### Read-Only Attributes

| Method | Description |
| ------------- | ------------- |
| `$().articulate('enabled');` | Returns (true / false) specifying whether the browser supports the Web Speech API |
| `$().articulate('isSpeaking');` | Returns (true / false) specifying whether speaking has not yet been completed or stopped |
| `$().articulate('isPaused');` | Returns (true / false) specifying whether speaking is paused |

**Note:** `$().articulate('isSpeaking');` returns the value of true even when paused


### Get and Set Voices

| Method | Description |
| ------------- | ------------- |
| `$().articulate('getVoices');` | Returns an array of voice objects; each object has two properties: name and language |
| `$().articulate('getVoices',selector,text);` | Populates the DOM element(s) *selector* with a dropdown menu for voice selection; optional *text* overwrites default dropdown menu instruction |
| `$().articulate('setVoice','name',voice);` | Sets voice; must match exactly one of the names returned when using `getVoices` |
| `$().articulate('setVoice','language',twoDigit);` | Sets voice by finding the first voice that matches the *twoDigit* language code; this is case-insensitive |
| `$().articulate('setVoice','language',code);` | Sets voice by finding the first voice that exactly matches the complete language code |

**Note:** Default text for `getVoices` dropdown menu is: 'Choose a Different Voice'.

**Note:** Language codes consist of two-characters that specify the language, followed by a hyphen, followed by additional characters that specify the particular country or regional dialect of that language. For example, the codes "en-US" and "en-GB" are both English language, but each represent a different country.

**Note:** Setting a voice by specifying only a two-digit language code is useful for when you have text on the page in another language, but don't want to bother checking to see if that language is available. For example, a page otherwise in English may have a paragraph in German that you want spoken. That paragraph can have a link like this:

`$('p').articulate('setVoice','language','de').articulate('speak');`

If the German language is available, it will be appropriately spoken. If not, the current voice will remain.


### Text Manipulation

| Method | Description |
| ------------- | ------------- |
| `$().articulate('ignore',tagName,tagName,...);` | Adds one or more HTML tags to the default array of ignored HTML tags; omitting *tagName* clears the array of user-specified ignored HTML tags; see Reference Information below |
| `$().articulate('recognize',tagName,tagName,...);` | Removes one or more HTML tags from the default array of ignored HTML tags; omitting *tagName* clears the array of user-specified recognized HTML tags; see Reference Information below |
| `$().articulate('replace',oldText,newText,...);` | Replaces *oldText* with *newText* when speaking; this is case-insensitive; multiple pairs of text can be specified; omitting parameters deletes previous replace commands |
| `$().articulate('customize',tagName,prepend);` | Replaces default text spoken prior to the description of the HTML tags `<img>`, `<table>`, and `<figure>`; omitting parameters reverts values to its defaults; see Reference Information below |
| `$().articulate('customize',tagName,prepend,append);` | Replaces default text spoken prior to and after the content of the HTML tags `<q>`, `<ol>`, `<ul>`, and `<blockquote>`; omitting parameters reverts values to its defaults; see Reference Information below |


### HTML Data Attributes

| Data Attribute | Description |
| ------------- | ------------- |
| `data-articulate-ignore` | Content from that DOM element and its descendents are ignored |
| `data-articulate-recognize` | Content from that DOM element is spoken, overriding the default |
| `data-articulate-spell` | Content from that DOM element is spelled out |
| `data-articulate-prepend=text` | Specified *text* is spoken prior to the content of its DOM element |
| `data-articulate-append=text` | Specified *text* is spoken after to the content of its DOM element |
| `data-articulate-swap=text` | Specified *text* is spoken in place of the content of its DOM element |


### Miscellaneous

```html
<!-- <articulate>text</articulate> -->
```

Specified *text* is spoken when encountered in the HTML; the syntax must match exactly — one space separating the opening and closing `<articulate>` tags and their neighboring comment markers


### Reference Information

Chaining calls is acceptable. For example, the following works just fine:

```javascript
$('article').articulate('rate',1.3).articulate('speak');
```

**Ignored Tags:** audio, button, canvas, code, del, dialog, dl, embed, form, head, iframe, meter, nav, noscript, object, s, script, select, style, textarea, video

| HTML Tag | Default Prepend Text | Default Append Text |
| ------------- | ------------- | ------------- |
| `<img>` | There's an embedded image with the description, | n/a |
| `<table>` | There's an embedded table with the caption, | n/a |
| `<figure>` | There's an embedded figure with the caption, | n/a |
| `<q>` and “ ” | Quote, | , Unquote, |
| `<ol>` | Start of list. | End of list. |
| `<ul>` | Start of list. | End of list. |
| `<blockquote>` | Blockquote start. | Blockquote end. |

**Note:** A comma followed by a space results in a pause when spoken; a period results in a slightly longer pause


### Examples

```javascript
$().articulate('ignore','h5','h6','em'); // Do not speak the content of <h5>, <h6>, and <em> tags
```

```javascript
$().articulate('recognize','button','code'); // Speak the content of <button> and <code> tags
```

```javascript
$().articulate('replace','i.e.','That is'); // When 'i.e.' is encountered, say 'That is'
```

```javascript
$().articulate('customize','img','Embedded'); // Change default intro text spoken when <img> is encountered
```

```javascript
$().articulate('customize','ol','Start of numbered List.','End of Numbered List'); // Change default intro and outro text spoken when <ol> is encountered
```

--------------

```html
<div class="speech">
  <h1 data-articulate-prepend="An analysis of">The Gettysburg Address</h1>
  
  <p>Despite the speech's prominent place in the history and popular culture 
  of the <span data-articulate-append="of America">United States</span>, the 
  exact wording and location of the speech are disputed. The five known manuscripts 
  of the Gettysburg Address in Lincoln's hand differ in a number of details, and 
  also differ from contemporary newspaper reprints of the speech.</p>
  
  <p><span data-articulate-swap="Experienced researchers locate">Modern scholarship locates</span> 
  the speakers' platform 40 yards <span data-articulate-ignore>(or more)</span> 
  away from the Traditional Site within Soldiers' National Cemetery at 
  the Soldiers' National Monument and entirely within private, adjacent 
  Evergreen Cemetery.</p>
  
  <!-- <articulate>This is the end of the article.</articulate> -->
</div>
```

Using `$('div.speech').articulate('speak');` The above will be spoken as:

> An analysis of The Gettysburg Address. Despite the speech's prominent place in the history and popular culture 
  of the United States of America, the exact wording and location of the speech are disputed. The five known manuscripts 
  of the Gettysburg Address in Lincoln's hand differ in a number of details, and also differ from contemporary newspaper 
  reprints of the speech. Experienced researchers locate the speakers' platform 40 yards away from the Traditional Site 
  within Soldiers' National Cemetery at the Soldiers' National Monument and entirely within private, adjacent 
  Evergreen Cemetery. This is the end of the article.


## Share Links

[Facebook](https://www.facebook.com/sharer.php?u=https%3A%2F%2Fgithub.com%2Facoti%2Farticulate.js)

[Twitter](https://twitter.com/intent/tweet?url=https%3A%2F%2Fgithub.com%2Facoti%2Farticulate.js&text=A%20jQuery%20plugin%20that%20lets%20the%20browser%20speak%20to%20you.)

[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https%3A%2F%2Fgithub.com%2Facoti%2Farticulate.js)


## About Adam Coti

I've been a freelance front-end Web developer for over eighteen years. My availability varies, but feel free to contact me if you're interested in the services I offer. My Web site with portfolio and client list is at [www.purefreedom.com](http://www.purefreedom.com). Also, take a look at some recent articles I've written for [CSS-Tricks](https://css-tricks.com/author/adamcoti/).

