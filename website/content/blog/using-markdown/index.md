---
title: "The md Document Format: Markdown 101"
date: 2023-11-21
draft: false
summary: "Learn about creating documents in the markdown language."
description: "Learn about creating documents in the markdown language."
slug: "using-markdown"
tags: ["markdown", "md", "document"]
---
{{< lead >}}
**Markdown** is a simple way to make text look nice without fancy editors or HTML. It adds headings, lists, and links to plain text, and it works the same on different devices.
{{< /lead >}}

# Brief Introduction to Markdown 
It was created by John Gruber and Aaron Swartz in 2004, aiming to provide a simple way for non-programmers to write content in a format that could be easily converted to HTML. Markdown allows users to add formatting elements like headings, lists, and links using plain text, making it accessible and user-friendly. Over time, Markdown has become widely adopted for various applications, including writing documentation, creating web content, blogging, instant messaging and collaborating on coding projects. It has gained popularity due to its simplicity, readability, and versatility.

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

## Code Blocks

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

