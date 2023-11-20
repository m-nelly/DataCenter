### Internal linking
```md
Link to a page: [[Internal link]].
```
Link to a page: [Internal link](https://help.obsidian.md/How+to/Internal+link).
### Embeds
Embed another file (read more about [Embed files](https://help.obsidian.md/How+to/Embed+files)). Here's an embedded section:
```md
![[Obsidian#What is Obsidian]]
```
### Headers
```md
# This is a heading 1
## This is a heading 2
### This is a heading 3 
#### This is a heading 4
##### This is a heading 5
###### This is a heading 6
```
# This is a heading 1
## This is a heading 2
### This is a heading 3
#### This is a heading 4
##### This is a heading 5
###### This is a heading 6
### Emphasis
```md
*This text will be italic*
_This will also be italic_
```
_This text will be italic_
*This will also be italic*
```md
**This text will be bold**
__This will also be bold__
```
**This text will be bold**
__This will also be bold__
```md
_You **can** combine them_
```
_You **can** combine them_
### Lists
```md
- Item 1
- Item 2
  - Item 2a
  - Item 2b
1. Item 1
1. Item 2
1. Item 3
   1. Item 3a
   1. Item 3b
```
-   Item 1
-   Item 2
    -   Item 2a
    -   Item 2b
1.  Item 1
2.  Item 2
3.  Item 3
    1.  Item 3a
    2.  Item 3b
### Images
```md
![Engelbart](https://history-computer.com/ModernComputer/Basis/images/Engelbart.jpg)
```
![Engelbart](https://history-computer.com/ModernComputer/Basis/images/Engelbart.jpg)
#### Resizing images
Example of this above image resized to 100 pixels wide:
```md
![Engelbart|100](https://history-computer.com/ModernComputer/Basis/images/Engelbart.jpg)
```
![Engelbart|100](https://history-computer.com/ModernComputer/Basis/images/Engelbart.jpg)
### Links
#### External links
Markdown style links can be used to refer to either external objects, such as web pages, or an internal page or image.
```md
http://obsidian.md
[Obsidian](http://obsidian.md)
```
http://obsidian.md
[Obsidian](http://obsidian.md)
#### Obsidian URI links
[Obsidian URI](https://help.obsidian.md/Advanced+topics/Using+obsidian+URI) links can be used to open notes in Obsidian either from another Obsidian vault or another program.
For example, you can link to a file in a vault like so (please note the [required encoding](https://help.obsidian.md/Advanced+topics/Using+obsidian+URI#Encoding)):
```md
[Link to note](obsidian://open?path=D:%2Fpath%2Fto%2Ffile.md)
```
[Link to note](obsidian://open?path=D:%2Fpath%2Fto%2Ffile.md)
You can link to a note by its vault name and file name instead of path as well:
```md
[Link to note](obsidian://open?vault=MainVault&file=MyNote.md)
```
[Link to note](obsidian://open?vault=MainVault&file=MyNote.md)
#### Escaping
If there are spaces in the url, they can be escaped by either using `%20` as a space, such as:
```md
[Export options](Pasted%20image)
```
[Export options](https://help.obsidian.md/Pasted+image)
Or you can enclose the target in `<>`, such as:
```md
[Slides Demo](<Slides Demo>)
```
[Slides Demo](https://help.obsidian.md/Attachments/Slides+demo)
### Blockquotes
```md
> Human beings face ever more complex and urgent problems, and their effectiveness in dealing with these problems is a matter that is critical to the stability and continued progress of society.

\- Doug Engelbart, 1961
```
> Human beings face ever more complex and urgent problems, and their effectiveness in dealing with these problems is a matter that is critical to the stability and continued progress of society.
- Doug Engelbart, 1961
### Inline code
```md
Text inside `backticks` on a line will be formatted like code.
```
Text inside `backticks` on a line will be formatted like code.
### Code blocks
Syntax highlight is supported with the language specified after the first set of backticks. We use prismjs for syntax highlighting, a list of supported languages can be found [at their site](https://prismjs.com/#supported-languages)
```js
function fancyAlert(arg) {
  if(arg) {
    $.facebox({div:'#foo'})
  }
}
```
### Task list
```md
- [x] #tags, [links](), **formatting** supported
- [x] list syntax required (any unordered or ordered list supported)
- [x] this is a complete item
- [?] this is also a complete item (works with every character)
- [ ] this is an incomplete item
- [ ] tasks can be clicked in Preview to be checked off
```
-   [\#tags](https://publish.obsidian.md/#tags), [links](https://publish.obsidian.md/), **formatting** supported
-   list syntax required (any unordered or ordered list supported)
-   this is a complete item
-   this is also a complete item (works with every character)
-   this is an incomplete item
-   tasks can be clicked in Preview to be checked off
### Tables
You can create tables by assembling a list of words and dividing them with hyphens `-` (for the first row), and then separating each column with a pipe `|`:
```md
First Header | Second Header
------------ | ------------
Content from cell 1 | Content from cell 2
Content in the first column | Content in the second column
```

| First Header                | Second Header                |
| --------------------------- | ---------------------------- |
| Content from cell 1         | Content from cell 2          |
| Content in the first column | Content in the second column |
```md
Tables can be justified with a colon | Another example with a long title
:----------------|-------------:
because of the `:` | these will be justified
```

Tables can be justified with a colon | Another example with a long title
:----------------|-------------:
because of the `:` | these will be justified

If you put links in tables, they will work, but if you use Piped Links, the pipe must be escaped with a `\` to prevent it being read as a table element.
```md
First Header | Second Header
------------ | ------------
[[Format your notes\|Formatting]]	|  [[Keyboard shortcuts\|hotkeys]]
```

First Header | Second Header
------------ | ------------
[[Format your notes\|Formatting]]	|  [[Keyboard shortcuts\|hotkeys]]
### Strikethrough
```md
Any word wrapped with two tildes (like ~~this~~) will appear crossed out.
```
Any word wrapped with two tildes (like ~~this~~) will appear crossed out.
### Highlighting
```md
Use two equal signs to ==highlight text==.
```
Use two equal signs to ==highlight== text.
### Horizontal Bar
```md
Use three stars ***, hyphens ---, or underscores ___ in a new line to produce an horizontal bar.

```
---
### Footnotes
```md
Here's a simple footnote,[^1] and here's a longer one.[^bignote]

[^1]: meaningful!

[^bignote]: Here's one with multiple paragraphs and code.

    Indent paragraphs to include them in the footnote.

    `{ my code }`

    Add as many paragraphs as you like.
```
Here's a simple footnote,[[1]](https://publish.obsidian.md/#fn-1-d8254699267d53c7) and here's a longer one.[[2]](https://publish.obsidian.md/#fn-2-d8254699267d53c7)
```md
You can also use inline footnotes. ^[notice that the carat goes outside of the brackets on this one.]
```
You can also use inline footnotes. [[3]](https://publish.obsidian.md/#fn-3-d8254699267d53c7)
### Math
```md
$$\begin{vmatrix}a & b\\
c & d
\end{vmatrix}=ad-bc$$
```
Obsidian uses [Mathjax](http://docs.mathjax.org/en/latest/basic/mathjax.html). You can check which packages are supported in Mathjax [here](http://docs.mathjax.org/en/latest/input/tex/extensions/index.html).
### Comments
Use `%%` to enclose comments, which will be parsed as Markdown, but will not show up in the preview.
```md
Here is some inline comments: %%You can't see this text%% (Can't see it)

Here is a block comment:
%%
It can span
multiple lines
%%
```
## Callouts
Use the following syntax to denote a callout block: `> [!INFO]`.
Learn more about callouts [here](https://help.obsidian.md/How+to/Use+callouts).
```markdown
> [!INFO]
> Here's a callout block.
> It supports **markdown** and [[Internal link|wikilinks]].
```

> [!INFO]
> Here's a callout block.
> It supports **markdown** and [[Internal link|wikilinks]].
### Diagram
Obsidian uses [Mermaid](https://mermaid-js.github.io/) to render diagrams and charts. Mermaid also provides [a helpful live editor](https://mermaid-js.github.io/mermaid-live-editor).

```mermaid
sequenceDiagram
    Alice->>+John: Hello John, how are you?
    Alice->>+John: John, can you hear me?
    John-->>-Alice: Hi Alice, I can hear you!
    John-->>-Alice: I feel great!
```
Obsidian supports linking to notes in Mermaid:
```mermaid
graph TD

Biology --> Chemistry

class Biology,Chemistry internal-link;
```
````
An easier way to do it is the following:
````
```mermaid
graph TD

A[Biology]
B[Chemistry]

A --> B

class A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z internal-link;
```

This way, all the note names (at least until `Z[note name]`) are all automatically assigned the class `internal-link` when you use this snippet.
If you use special characters in your note names, you need to put the note name in double quotes.  
`"⨳ special character"`  
It looks like this if you follow the [second option](https://help.obsidian.md/How+to/Format+your+notes#^376b9d):  
`A["⨳ special character"]`
## Developer notes
We strive for maximum capability without breaking any existing formats, therefore we use a slightly unorthodox combination of flavors of markdown. It is broadly CommonMark, with the addition of some functionality from GitHub Flavored Markdown (GFM), some LaTeX support, and our chosen embed syntax, which you can read more about at [Accepted file formats](https://help.obsidian.md/Advanced+topics/Accepted+file+formats).
We intentionally do not support parsing markdown syntax and blank lines within HTML blocks. This is the result of an optimization to handle very large files and to support synchronization between editing and reading mode.

1.  meaningful![↩︎](https://publish.obsidian.md/#fnref-1-d8254699267d53c7)
    
2.  Here's one with multiple paragraphs and code.
    
    Indent paragraphs to include them in the footnote.
    
    `{ my code }`
    
    Add as many paragraphs as you like.[↩︎](https://publish.obsidian.md/#fnref-2-d8254699267d53c7)
    
3.  notice that the carat goes outside of the brackets on this one. [↩︎](https://publish.obsidian.md/#fnref-3-d8254699267d53c7)