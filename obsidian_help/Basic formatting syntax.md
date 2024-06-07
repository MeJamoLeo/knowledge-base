#markdown #tutorial #document
Learn how to apply basic formatting to your notes, using [Markdown](https://daringfireball.net/projects/markdown/). For more advanced formatting syntax, refer to [Advanced formatting syntax](https://help.obsidian.md/Editing+and+formatting/Advanced+formatting+syntax).

## Paragraphs
This is a paragraph.

This is another paragraph.

## Headings `#`
- # This is a heading 1
- ## This is a heading 2
- ### This is a heading 3
- #### This is a heading 4
- ##### This is a heading 5
- ###### This is a heading 6

## Bold, italics, highlights `*` `_` `==` `~~`
- **Bold text**
- _Italic text_ 
- ~~Striked out text~~
- ==Highlighted text==
- **Bold text and _nested italic_ text**
- **_Bold and italic text_**

Formatting can be forced to display in plain text by adding a backslash `\` in front of it.

- \*\*This line will not be bold\*\*
- \*_This line will be italic and show the asterisks_\*

## Internal links
-  [[Welcome]]

 [internal links](https://help.obsidian.md/Linking+notes+and+files/Internal+links)

## External links

- Other URL
	- [Obsidian Help](https://help.obsidian.md)
- Other Vault
	  - [Note](obsidian://open?vault=MainVault&file=Note.md)

### Escape blank spaces in links
- `%20` [My Note](obsidian://open?vault=MainVault&file=My%20Note.md)
- `< >` [My Note](<obsidian://open?vault=MainVault&file=My Note.md>)

## External images

- `!`
![Engelbart](https://history-computer.com/ModernComputer/Basis/images/Engelbart.jpg)
- `|100x145`
![Engelbart|100x145](https://history-computer.com/ModernComputer/Basis/images/Engelbart.jpg)
- `|100`
![Engelbart|100](https://history-computer.com/ModernComputer/Basis/images/Engelbart.jpg)

## Quotes `>`
> Human beings face ever more complex and urgent problems, and their effectiveness in dealing with these problems is a matter that is critical to the stability and continued progress of society.

\- Doug Engelbart, 1961

- [callout](https://help.obsidian.md/Editing+and+formatting/Callouts)
	> [!info] Human beings face ever more complex and urgent problems, and their effectiveness in dealing with these problems is a matter that is critical to the stability and continued progress of society.
	
	> [!todo] Human beings face ever more complex and urgent problems, and their effectiveness in dealing with these problems is a matter that is critical to the stability and continued progress of society.
	
	> [!tip] Human beings face ever more complex and urgent problems, and their effectiveness in dealing with these problems is a matter that is critical to the stability and continued progress of society.
	
	> [!done] Human beings face ever more complex and urgent problems, and their effectiveness in dealing with these problems is a matter that is critical to the stability and continued progress of society.
	
	> [!question] Human beings face ever more complex and urgent problems, and their effectiveness in dealing with these problems is a matter that is critical to the stability and continued progress of society.
	
	> [!warning] Human beings face ever more complex and urgent problems, and their effectiveness in dealing with these problems is a matter that is critical to the stability and continued progress of society.
	
	> [!fail] Human beings face ever more complex and urgent problems, and their effectiveness in dealing with these problems is a matter that is critical to the stability and continued progress of society.
	
	> [!error] Human beings face ever more complex and urgent problems, and their effectiveness in dealing with these problems is a matter that is critical to the stability and continued progress of society.
	
	> [!bug] Human beings face ever more complex and urgent problems, and their effectiveness in dealing with these problems is a matter that is critical to the stability and continued progress of society.
	
## Lists
-   First list item
-   Second list item
-   Third list item

1.  First list item
2.  Second list item
3.  Third list item

### Task lists `[ ]`
- [x] This is a completed task.
- [ ] This is an incomplete task.

You can use any character inside the brackets to mark it as complete.
- [x] Milk
- [?] Eggs
- [-] Eggs

### Nesting lists
1.  First list item
    1.  Ordered nested list item
2.  Second list item
    -   Unordered nested list item
- [ ] Task item 1
	- [ ] Subtask 1
- [ ] Task item 2
	- [ ] Subtask 1

## Horizontal rule
***
****
* * *
---
----
- - -
___
____
_ _ _

## Code
### Inline code
Text inside `backticks` on a line will be formatted like code.

### Code blocks
```js
function fancyAlert(arg) { if(arg) { $.facebox({div:'#foo'}) } }
```

## Footnotes

You can add footnotes<sup data-footnote-id="fnref-1-6aa591a0e107c200" id="fnref-1-6aa591a0e107c200"><a href="https://publish.obsidian.md/#fn-1-6aa591a0e107c200" target="_blank" rel="noopener">[1]</a></sup> to your notes using the following syntax:

```md
This is a simple footnote[^1]. [^1]: This is the referenced text. [^2]: Add 2 spaces at the start of each new line. This lets you write footnotes that span multiple lines. [^note]: Named footnotes still appear as numbers, but can make it easier to identify and link references.
```

You can also inline footnotes in a sentence. Note that the caret goes outside the brackets.

```md
You can also use inline footnotes. ^[This is an inline footnote.]
```

Inline footnotes only work in reading view, not in Live Preview.

## Comments `%%`

This is an %%inline%% comment.
%% This is a block comment. Block comments can span multiple lines. %%

## Learn more

To learn more advanced formatting syntax, such as tables, diagrams, and math expressions, refer to [Advanced formatting syntax](https://help.obsidian.md/Editing+and+formatting/Advanced+formatting+syntax).

___

1.  This is a footnote.[↩︎](https://publish.obsidian.md/#fnref-1-6aa591a0e107c200