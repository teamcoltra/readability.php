# Readability.php
[![Latest Stable Version](https://poser.pugx.org/andreskrey/readability.php/v/stable)](https://packagist.org/packages/andreskrey/readability.php) [![StyleCI](https://styleci.io/repos/71042668/shield?branch=master)](https://styleci.io/repos/71042668)

PHP port of *Mozilla's* **[Readability.js](https://github.com/mozilla/readability)**. Parses html text (usually news and other articles) and tries to return title, byline and text content. Analizes each text node, gives an score and orders them based on this calculation.

**Requires**: PHP 5.4+

**Lead Developer**: Andres Rey

## Status

Current status is *ultra-mega-alpha*. It is broken right now and it will change dramatically until the first 1.0 release. Expect wild changes. Submit pull requests. Argue with me.

## How to use it

First you have to require the library using composer:

`composer require andreskrey/readability.php`

Then, create and HTMLParser object with your preferences, feed the `parse()` function with your HTML and check the resulting array:

```php 
use andreskrey\Readability\HTMLParser;

$readability = new HTMLParser();

$html = file_get_contents('http://your.favorite.newspaper/article.html');

$result = $readability->parse($html);
```

The `$result` variable now will hold the following information:

```
$result = [
    'title' => 'Title of the article',
    'author' => 'Name of the author of the article',
    'image' => 'Main image of the article',
    'article' => 'DOMDocument with the full article text, scored and parsed'
]
```

## Options

## Limitations

Of course the main limitation is PHP. Websites that load the content through lazy loading, AJAX, or any type of javascript fueled call will be ignored (actually, *not ran*) and the resulting text will be incorrect, compared to the readability.js results. All the articles you want to parse with readability.php will need to be complete and all the content should be in the HTML already.  

## Known Issues

None so far.

## To-do

100% of the original readability code was ported, at least until the last commit when I started this project ([13 Aug 2016](https://github.com/mozilla/readability/commit/71aa562387fa507b0bac30ae7144e1df7ba8a356)). There are a lot of `TODO`s around the code, which are the part that need to be finished.

## Dependencies

Readability uses the Element interface and class from *The PHP League's* **[html-to-markdown](https://github.com/thephpleague/html-to-markdown/)**. The Readability object is an extension of the Element class. It overrides some methods but relies on it for basic DOMElement parsing.

## How it works

Readability parses all the text with DOMDocument, scans the text nodes and gives the a score, based on the amount of words, links and type of element. Then it selects the highest scoring element and creates a new DOMDocument with all its siblings. Each sibling is scored to discard useless elements, like nav bars, empty nodes, etc.