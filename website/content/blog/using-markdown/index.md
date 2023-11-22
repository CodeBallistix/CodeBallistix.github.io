---
title: "Markdown - Basic Usage"
date: 2023-11-21
draft: false
summary: "Learn about creating documents in the markdown markup language."
description: "Learn about creating documents in the markdown language."
slug: "using-markdown"
tags: [markdown, md, document, headings, paragraph, blockquote, image, table, code, block, list, html, rule, horizontal, escape, escaping, characters,  cheatsheet, examples]
series: ["Markdown"]
series_order: 2
showAuthor: true
authors:
  - mrinank
showAuthorBadges: true
---
{{< lead >}}
**Markdown** is a simple way to make text look nice without fancy editors or HTML. It adds headings, lists, and links to plain text, and it works the same on different devices.
{{< /lead >}}

# Cheatsheet and Examples

This offers a sample of basic Markdown formatting that can be used freely.

## Headings

The following HTML elements represent six levels of section headings. `h1` is the highest section level while `h4` is the lowest.

{{< highlight md >}}
# H1
{{< /highlight >}}

# H1

{{< highlight md >}}
## H2
{{< /highlight >}}

## H2

{{< highlight md >}}
### H3
{{< /highlight >}}
### H3

{{< highlight md >}}
#### H4
{{< /highlight >}}
#### H4

## Paragraph

{{< highlight md >}}
Xerum, quo qui aut unt expliquam qui dolut labo. Aque venitatiusda cum, voluptionse latur sitiae dolessi aut parist aut dollo enim qui voluptate ma dolestendit peritin re plis aut quas inctum laceat est volestemque commosa as cus endigna tectur, offic to cor sequas etum rerum idem sintibus eiur? Quianimin porecus evelectur, cum que nis nust voloribus ratem aut omnimi, sitatur? Quiatem. Nam, omnis sum am facea corem alique molestrunt et eos evelece arcillit ut aut eos eos nus, sin conecerem erum fuga. Ri oditatquam, ad quibus unda veliamenimin cusam et facea ipsamus es exerum sitate dolores editium rerore eost, temped molorro ratiae volorro te reribus dolorer sperchicium faceata tiustia prat.

Itatur? Quiatae cullecum rem ent aut odis in re eossequodi nonsequ idebis ne sapicia is sinveli squiatum, core et que aut hariosam ex eat.
{{< /highlight >}}

Xerum, quo qui aut unt expliquam qui dolut labo. Aque venitatiusda cum, voluptionse latur sitiae dolessi aut parist aut dollo enim qui voluptate ma dolestendit peritin re plis aut quas inctum laceat est volestemque commosa as cus endigna tectur, offic to cor sequas etum rerum idem sintibus eiur? Quianimin porecus evelectur, cum que nis nust voloribus ratem aut omnimi, sitatur? Quiatem. Nam, omnis sum am facea corem alique molestrunt et eos evelece arcillit ut aut eos eos nus, sin conecerem erum fuga. Ri oditatquam, ad quibus unda veliamenimin cusam et facea ipsamus es exerum sitate dolores editium rerore eost, temped molorro ratiae volorro te reribus dolorer sperchicium faceata tiustia prat.

Itatur? Quiatae cullecum rem ent aut odis in re eossequodi nonsequ idebis ne sapicia is sinveli squiatum, core et que aut hariosam ex eat.

## Blockquotes

The blockquote element represents content that is quoted.

{{< highlight md >}}
> Tiam, ad mint andaepu dandae nostion secatur sequo quae.
> **Note** that you can use _Markdown syntax_ within a blockquote.
{{< /highlight >}}

> Tiam, ad mint andaepu dandae nostion secatur sequo quae.
> **Note** that you can use _Markdown syntax_ within a blockquote.

### Multiple Paragraphs

Blockquotes can contain multiple paragraphs. Add a `>` on the blank lines between the paragraphs.

{{< highlight md >}}
> Dorothy followed her through many of the beautiful rooms in her castle.
>
> The Witch bade her clean the pots and kettles and sweep the floor and keep the fire fed with wood.
{{< /highlight >}}

> Dorothy followed her through many of the beautiful rooms in her castle.
>
> The Witch bade her clean the pots and kettles and sweep the floor and keep the fire fed with wood.

### Nested Blockquotes

Blockquotes can be nested. Add a `>>` in front of the paragraph you want to nest.

{{< highlight md >}}
> Dorothy followed her through many of the beautiful rooms in her castle.
>
>> The Witch bade her clean the pots and kettles and sweep the floor and keep the fire fed with wood.
{{< /highlight >}}

> Dorothy followed her through many of the beautiful rooms in her castle.
>
>> The Witch bade her clean the pots and kettles and sweep the floor and keep the fire fed with wood.

### Blockquotes with other elements

{{< highlight md >}}
> #### The quarterly results look great!
>
> - Revenue was off the chart.
> - Profits were higher than ever.
>
>  *Everything* is going according to **plan**.
{{< /highlight >}}

> #### The quarterly results look great!
>
> - Revenue was off the chart.
> - Profits were higher than ever.
>
>  *Everything* is going according to **plan**.

## Images

To add an image, add an exclamation mark (!), followed by alt text in brackets, and the path or URL to the image asset in parentheses. You can optionally add a title in quotation marks after the path or URL.

{{< highlight md >}}
![The CodeBallistix Logo is awesome!](./logo.jpg "CodeBallistix Logo")
{{< /highlight >}}

![The CodeBallistix Logo is awesome!](./logo.png "CodeBallistix Logo")

## Tables

Tables aren't part of the core Markdown spec, but many content platforms support it.

{{< highlight md >}}
| Name  | Age |
| ----- | --- |
| Bob   | 27  |
| Alice | 23  |
{{< /highlight  >}}

| Name  | Age |
| ----- | --- |
| Bob   | 27  |
| Alice | 23  |

### Inline Markdown within tables

{{< highlight md >}}
| Italics   | Bold     | Code   |
| --------- | -------- | ------ |
| _italics_ | **bold** | `code` |
{{< /highlight >}}

| Italics   | Bold     | Code   |
| --------- | -------- | ------ |
| _italics_ | **bold** | `code` |

## Code

To denote a word or phrase as code, enclose it in backticks (`).

{{< highlight md >}}
At the command prompt, type `nano`.
{{< /highlight >}}

At the command prompt, type `nano`.

### Escaping Backticks

If the word or phrase you want to denote as code includes one or more backticks, you can escape it by enclosing the word or phrase in double backticks (``).

{{< highlight md >}}
``Use `code` in your Markdown file.``
{{< /highlight >}}

``Use `code` in your Markdown file.``



## Code Blocks

To create code blocks, indent every line of the block by at least four spaces or one tab.

{{< highlight md >}}

    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="utf-8" />
        <title>Example HTML5 Document</title>
      </head>
      <body>
        <p>Test</p>
      </body>
    </html>

{{< /highlight >}}

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Example HTML5 Document</title>
  </head>
  <body>
    <p>Test</p>
  </body>
</html>
```

The basic Markdown syntax allows you to create code blocks by indenting lines by four spaces or one tab. If you find that inconvenient, try using fenced code blocks. Depending on your Markdown processor or editor, you’ll use three backticks (```) or three tildes (~~~) on the lines before and after the code block. The best part? You don’t have to indent any lines!


{{< highlight md >}}
```
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```
{{< /highlight >}}

```json
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```

## List Types

### Ordered List

{{< highlight md >}}
1. First item
2. Second item
3. Third item
3. The actual numbers don't really matter.
7. Don't worry if you miss or repeat numbers.
{{< /highlight >}}

1. First item
2. Second item
3. Third item
3. The actual numbers don't really matter.
7. Don't worry if you miss or repeat numbers.

### Unordered List

{{< highlight md >}}
- List item
- Another item
- And another item
{{< /highlight >}}
- List item
- Another item
- And another item

### Nested list
{{< highlight md >}}
- Fruit
  - Apple
  - Orange
  - Banana
- Dairy
  - Milk
  - Cheese
{{< /highlight >}}

- Fruit
  - Apple
  - Orange
  - Banana
- Dairy
  - Milk
  - Cheese

### Adding Elements in the Lists

To add another element in a list while preserving the continuity of the list, indent the element four spaces or one tab, as shown in the following examples.

> **Tip:** If things don't appear the way you expect, double check that you've indented the elements in the list four spaces or one tab.

#### Paragraphs

{{< highlight md >}}
* This is the first list item.
* Here's the second list item.

    I need to add another paragraph below the second list item.

* And here's the third list item.
{{< /highlight >}}

* This is the first list item.
* Here's the second list item.

    I need to add another paragraph below the second list item.

* And here's the third list item.

**For Ordered Lists:**

{{< highlight md >}}
1. This is the first list item.
1. Here's the second list item.

    I need to add another paragraph below the second list item.

1. And here's the third list item.
{{< /highlight >}}

1. This is the first list item.
1. Here's the second list item.

    I need to add another paragraph below the second list item.

1. And here's the third list item.

#### Blockquotes

{{< highlight md >}}
* This is the first list item.
* Here's the second list item.

    > A blockquote would look great below the second list item.

* And here's the third list item.
{{< /highlight >}}

* This is the first list item.
* Here's the second list item.

    > A blockquote would look great below the second list item.

* And here's the third list item.

### Images

{{< highlight md >}}
1. Open the file containing the Linux mascot.
2. Marvel at its beauty.

    ![CodeBallistix Logo](./logo.png)

3. Close the file.
{{< /highlight >}}

1. Open the file containing the CodeBallistix Logo.
2. Ponder on it.

    ![CodeBallistix Logo](./logo.png)

3. Close the file.

## Horizontal Rules

To create a horizontal rule, use three or more asterisks (***), dashes (---), or underscores (___) on a line by themselves.

{{< highlight md >}}
***

---

_________________
{{< /highlight >}}


***

---

_________________


## Links

To create a link, enclose the link text in brackets (e.g., [Duck Duck Go]) and then follow it immediately with the URL in parentheses (e.g., (https://duckduckgo.com)).

{{< highlight md >}}
My favorite search engine is [Duck Duck Go](https://duckduckgo.com).
{{< /highlight >}}

My favorite search engine is [Duck Duck Go](https://duckduckgo.com).

### Adding Titles

You can optionally add a title for a link. This will appear as a tooltip when the user hovers over the link. To add a title, enclose it in quotation marks after the URL.

{{< highlight md >}}
My favorite search engine is [Duck Duck Go](https://duckduckgo.com "The best search engine for privacy").
{{< /highlight >}}

My favorite search engine is [Duck Duck Go](https://duckduckgo.com "The best search engine for privacy").

### URLs and Email Addresses

To quickly turn a URL or email address into a link, enclose it in angle brackets.

{{< highlight md >}}
<https://www.markdownguide.org>

<fake@example.com>
{{< /highlight >}}

<https://www.markdownguide.org>

<fake@example.com>

### Formatiting Links

To emphasize links, add asterisks before and after the brackets and parentheses. To denote links as code, add backticks in the brackets.

{{< highlight md >}}
I love supporting the **[EFF](https://eff.org)**.

This is the *[Markdown Guide](https://www.markdownguide.org)*.

See the section on [`code`](#code).
{{< /highlight >}}

I love supporting the **[EFF](https://eff.org)**.

This is the *[Markdown Guide](https://www.markdownguide.org)*.

See the section on [`code`](#code).

### Reference Style Links

Reference-style links are a special kind of link that make URLs easier to display and read in Markdown. Reference-style links are constructed in two parts: the part you keep inline with your text and the part you store somewhere else in the file to keep the text easy to read.

#### Formatting the First Part of the Link

The first part of a reference-style link is formatted with two sets of brackets. The first set of brackets surrounds the text that should appear linked. The second set of brackets displays a label used to point to the link you’re storing elsewhere in your document.

Although not required, you can include a space between the first and second set of brackets. The label in the second set of brackets is not case sensitive and can include letters, numbers, spaces, or punctuation.

This means the following example formats are roughly equivalent for the first part of the link:

{{< highlight md >}}
[hobbit-hole][1]

[hobbit-hole] [1]
{{< /highlight >}}

#### Formatting the Second Part of the Link

The second part of a reference-style link is formatted with the following attributes:

1. The label, in brackets, followed immediately by a colon and at least one space (e.g., [label]: ).
2. The URL for the link, which you can optionally enclose in angle brackets.
3. The optional title for the link, which you can enclose in double quotes, single quotes, or parentheses.

This means the following example formats are all roughly equivalent for the second part of the link:

{{< highlight md >}}
[1]: https://en.wikipedia.org/wiki/Hobbit#Lifestyle
[1]: https://en.wikipedia.org/wiki/Hobbit#Lifestyle "Hobbit lifestyles"
[1]: https://en.wikipedia.org/wiki/Hobbit#Lifestyle 'Hobbit lifestyles'
[1]: https://en.wikipedia.org/wiki/Hobbit#Lifestyle (Hobbit lifestyles)
[1]: <https://en.wikipedia.org/wiki/Hobbit#Lifestyle> "Hobbit lifestyles"
[1]: <https://en.wikipedia.org/wiki/Hobbit#Lifestyle> 'Hobbit lifestyles'
[1]: <https://en.wikipedia.org/wiki/Hobbit#Lifestyle> (Hobbit lifestyles)
{{< /highlight >}}

You can place this second part of the link anywhere in your Markdown document. Some people place them immediately after the paragraph in which they appear while other people place them at the end of the document (like endnotes or footnotes).

#### Putting this together

Say you add a URL as a [standard URL link](#links) to a paragraph and it looks like this in Markdown:

{{< highlight md >}}
In a hole in the ground there lived a hobbit. Not a nasty, dirty, wet hole, filled with the ends
of worms and an oozy smell, nor yet a dry, bare, sandy hole with nothing in it to sit down on or to
eat: it was a [hobbit-hole](https://en.wikipedia.org/wiki/Hobbit#Lifestyle "Hobbit lifestyles"), and that means comfort.
{{< /highlight >}}

Though it may point to interesting additional information, the URL as displayed really doesn’t add much to the existing raw text other than making it harder to read. To fix that, you could format the URL like this instead:

{{< highlight md >}}
In a hole in the ground there lived a hobbit. Not a nasty, dirty, wet hole, filled with the ends
of worms and an oozy smell, nor yet a dry, bare, sandy hole with nothing in it to sit down on or to
eat: it was a [hobbit-hole][1], and that means comfort.

[1]: <https://en.wikipedia.org/wiki/Hobbit#Lifestyle> "Hobbit lifestyles"
{{< /highlight >}}

In both the instances above the rendered output would be identical:

In a hole in the ground there lived a hobbit. Not a nasty, dirty, wet hole, filled with the ends
of worms and an oozy smell, nor yet a dry, bare, sandy hole with nothing in it to sit down on or to
eat: it was a [hobbit-hole][1], and that means comfort.

[1]: <https://en.wikipedia.org/wiki/Hobbit#Lifestyle> "Hobbit lifestyles"


and the HTML for the link would be:

{{< highlight html >}}
<a href="https://en.wikipedia.org/wiki/Hobbit#Lifestyle" title="Hobbit lifestyles">hobbit-hole</a>
{{< /highlight >}}

## Escaping Characters

To display a literal character that would otherwise be used to format text in a Markdown document, add a backslash (\) in front of the character.

{{< highlight md >}}
\* Without the backslash, this would be a bullet in an unordered list.
{{< /highlight >}}

\* Without the backslash, this would be a bullet in an unordered list.

### Characters you can escape

You can use a backslash to escape the following characters.

| Character | Name |
|:--- |:--- |
| \\ | backslash |
| \` | backtick (see also [escaping backticks in code](#escaping-backticks)) |
| \* | asterisk |
| \_ | underscore |
| \{ \} | curly braces |
| \[ \] | brackets |
| \< \> | angle brackets |
| \( \) | parentheses |
| \# | pound sign |
| \+ | plus sign |
| \- | minus sign (hyphen) |
| \. | dot |
| \! | exclamation mark |
| \| | pipe |

## HTML

Many Markdown applications allow you to use HTML tags in Markdown-formatted text. This is helpful if you prefer certain HTML tags to Markdown syntax. For example, some people find it easier to use HTML tags for images. Using HTML is also helpful when you need to change the attributes of an element, like specifying the color of text or changing the width of an image.

To use HTML, place the tags in the text of your Markdown-formatted file.

{{< highlight md >}}
This **word** is bold. This <em>word</em> is italic.
{{< /highlight >}}

This **word** is bold. This <em>word</em> is italic.

For security reasons, not all Markdown applications support HTML in Markdown documents. When in doubt, check your Markdown application’s documentation. Some applications support only a subset of HTML tags.

Use blank lines to separate block-level HTML elements like `<div>`, `<table>`, `<pre>`, and `<p>` from the surrounding content. Try not to indent the tags with tabs or spaces — that can interfere with the formatting.

You can’t use Markdown syntax inside block-level HTML tags. For example, `<p>italic and **bold**</p>` won’t work.