﻿HtmlSanitizer
=============

Vereyon's HtmlSanitizer is a rule based HTML sanitizer built on top of Microsofts HTML Agility pack.

```C#
var sanitizer = HtmlSanitizer.SimpleHtml5Sanitizer();
string cleanHtml = sanitizer.Sanitize(dirtyHtml);
```

Use cases
---------

HtmlSanitizer was designed with the following use cases in mind:

 * Prevent cross-site scripting by removing javascript.
 * Restrict HTML to simple markup in order to allow for easy transformation to other document types without having to deal with all possible HTML tags.
 * Enforce nofollow on links to discourage link spam.
 * Cleanup submitted HTML by removing empty tags for example.
 * Restrict HTML to a limited set of tags, for example in a comment system.

Features
--------

 * CSS class white listing
 * Empty tag removal
 * Tag white listing
 * Tag attribute and CSS class enforcement
 * Tag flattening to simplify document structure while maintaining content
 * Tag renaming
 * Attribute checks (e.g. URL validity)
 * A fluent style configuration interface
 
Usage
-----

### Basic usage

```C#
var sanitizer = HtmlSanitizer.SimpleHtml5Sanitizer();
string cleanHtml = sanitizer.Sanitize(dirtyHtml);
```

*Note: the SimpleHtml5Sanitizer returns a rule set which does not allow for a full document definition. Use SimpleHtml5DocumentSanitizer*

### Sanitize a document

```C#
var sanitizer = HtmlSanitizer.SimpleHtml5DocumentSanitizer();
string cleanHtml = sanitizer.Sanitize(dirtyHtml);
```

### Configuration

The code below demonstrates how to configure a rule set which only allows <strong>, <b>, <i> and <a> tags and which enforces the link tags to have a valid url, be no-follow and open in a new window.

```C#
var sanitizer = new HtmlSanitizer();
sanitizer.Tag("strong").RemoveEmpty();
sanitizer.Tag("b").Rename("strong").RemoveEmpty();
sanitizer.Tag("i").RemoveEmpty();
sanitizer.Tag("a").SetAttribute("target", "_blank")
	.SetAttribute("rel", "nofollow")
	.CheckAttribute("href", HtmlSanitizerCheckType.Url)
	.RemoveEmpty();

string cleanHtml = sanitizer.Sanitize(dirtyHtml);
```



Tests
-----

Got tests? Yes, see the tests project. It uses xUnit.