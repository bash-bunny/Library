# Markdown

Set of personal notes on how to write in **Markdown**

**Note**: always be consistent between notes and how to write them, write the same way the same things, like lists, code, etc.

## Headers

It start with hashtag `#`. Include a blank line after the header to separate it from the next paragraph. It's possible to have up to 6 headers

```markdown
# Heading 1

Paragraph

## Heading 2

Another paragraph
```

## Paragraph

They are consecutive lines of text. It's better to create one line with all the text that we want to include than separate it in multiple lines, because it's easier to edit and better to visualize it.

## Formatting

- One asterisk `*` for *italic*
- Two asterisk `**` for **bold** (strong)
- Three asterisks `***` for ***bold and italic*** (strong em)
- For `monospace` use `\`` which is known as pre, preformatting or code
- For mathematical expressions use dollars `$\math expression$`: $\sqrt{3x-1}+(1+x)^2$

**Note**: be aware that in some places like pandoc, the dollar sign is taken as a mathematical expression, so if you want to use it, you have to scape it with \ like `\$5000`

In general it's possible to scape every character with \ 

## Lists

It's possible to use `-`, `+` or `*`, only be consistent between your notes, and that particular character has to be the first one of the line, and be followed by a space

- This is a
- List with `-`

* This is
* another list with `*`

For ordered lists you can use the number 1 followed by a dot `1.` for all points, and when it's printed it renders it, and have sub-lists below that

1. First point of an ordered list
1. Second point
  1. First point of the sub-list
  1. Second point of the sub-list

## Hard Breaks

It's possible to break a line to the half if it ends with 2 blank spaces  
Like this one

## Verbatim Blocks

It's useful to define blocks of code, and you can use 4 spaces at the beginning or 3 backticks, which is known as *Code Fence*


Four spaces

    violets are blue
    roses are red

Code Fence

```
violets are blue
roses are red
```

It's possible to add the language of the code at the end of the 3 backticks

```ruby
puts "Learning markdown
```

To take notes in markdown about markdown use `~~~markdown` or `~~~~markdown` and inside that take notes with markdown notation

~~~markdown
In markdown the ` allows you to put code blocks

```js
console.log("Hello World")
```
~~~

## Blockquotes

It uses `>` at the beginning of the line without any space. It's better to use them in one line

> This is a
>
> Multiline
>
> Quote

## Comments

It's possible to put comments in markdown with HTML notation `<!-- This is a comment -->`, and it would not render. ***Do not use HTML in Markdown*** except for the comments of the code.

~~~markdown
<!-- This comment does not appear when render -->

And this <!-- comment --> neither
~~~

## Links

This is an [inline link to DeadS3c Library](https://github.com/deads3c/Library)

It's possible to put raw URLs with angle brackets `<>` like <https://github.com/deads3c/Library>. Always use full URLs.

It's possible to put emails and phone numbers too
- <mailto:example@test.com>
- <tel:555-555-5555>

## Separators

There are lines to divide text, it uses 4 dashes `----`

----

This text is separate it from the one above

## Images

You can add it with `![name of the image](path/to/the/image)`

![Cyber Cat Girl](hyprlandgirl.png)

Also you can define images like links `[![name of the image](path/to/the/image)](url)`

[![Cyber Cat Girl](hyprlandgirl.png)](https://hyprland.org/)
