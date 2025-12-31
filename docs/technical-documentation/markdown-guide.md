# Markdown guide

## 1. Headers

To create a heading, type number signs (`#`) in front of a word or phrase. The number of signs you use corresponds to the heading level.

> <span style="color:orangered">**⚠ Warning:** Always put a space between the number signs and the heading name.

`# Header 1`<br>
`## Header 2`<br>
`### Header 3`<br>
`#### Header 4`<br>
`##### Header 5`<br>
`###### Header 6`<br>

---

## 2. Separators

To create a separator, type three dashes (`-`) on a separate line. Use it to separate differentiated sections.

`---`

> <span style="color:orangered">**⚠ Warning:** Always enter blank lines before and after separators.

---

## 3. Blockquotes

To create a blockquote, add a `>` in front of a paragraph. Use it to emphasize important information, like warnings, notes, tips...

`> Blockquote`

Examples:

> <span style="color:orangered">**⚠ Warning:** Always put blank lines before and after blockquotes.

If you want the blockquote to contain multiple paragraphs, add a `>` on the blank lines between the paragraphs.

`> Paragraph 1`<br>
`>`<br>
`> Paragraph 2`<br>

If you want to embed a blockquote within a blockquote, use >>.

`> Main paragraph`<br>
`>`<br>
`>> Embedded paragraph`<br>

---

## 4. Lists

You can organize items into ordered, unordered, and task lists.

### 4.1. Ordered lists

To create an ordered list, add line items with numbers followed by periods.

> <span style="color:orangered">**❗Note:** The numbers don’t have to be in numerical order, as Markdown will automatically input them in the correct sequence, but the list should start with the number one.

`1. First line`<br>
`1. Second line`<br>
`1. Third line`<br>

To create a nested list, type an additional number after the period.

`1. First line`<br>
`1.1. First sub-line`<br>
`1.2 Second sub-line`<br>

### 4.2. Unordered lists

To create an unordered list, add dashes (`-`), in front of the line items.

`- First line`<br>
`- Second line`<br>
`- Third line`<br>

To create a nested list, type two spaces before the dash (`-`). To create a nested list within a nested list, add two more spaces, and so on.

Example:

`- Item 1`<br>
`  - Item 1.1`<br>
`      - Item 1.1.1`<br>
`        - Item 1.1.1.1`<br>
`- Item 2`<br>

> <span style="color:darkorange">💡 **Tip:** If you need to start an unordered list item with a number followed by a period, you can use a backslash (`\`) to avoid creating an ordered list.

> <span style="color:darkgreen">** Info:** You can nest any type of list within another one.

---

### 5. Tables

To add a table, use three or more hyphens (`---`) to create each column’s header, and use pipes (`|`) to separate each column. For compatibility, you should also add a pipe on either end of the row.

**Input:**

    | Syntax      | Description |
    | ----------- | ----------- |
    | Header      | Title       |
    | Paragraph   | Text        |

**Output:**

| Syntax      | Description |
| ----------- | ----------- |
| Header      | Title       |
| Paragraph   | Text        |

You can align text in the columns to the left, right, or center by adding a colon (`:`) to the left, right, or on both side of the hyphens within the header row.

**Input:**

    | Syntax      | Description | Test Text     |
    | :---        |    :----:   |          ---: |
    | Header      | Title       | Here's this   |
    | Paragraph   | Text        | And more      |

**Output:**

| Syntax      | Description | Test Text     |
| :---        |    :----:   |          ---: |
| Header      | Title       | Here's this   |
| Paragraph   | Text        | And more      |

---

## 6. Links

To create a link, enclose the link text in brackets and then follow it immediately with the URL in parentheses.

| **Input** | **Output** |
| ----------- | ----------- |
| `[Text that will be displayed](Link)`| [Text that will be displayed](Link)|

If you want to add a **tooltip** that will appear when the user hovers over the text, add a space and quotation marks after the link:

| **Input** | **Output** |
| ----------- | ----------- |
| `[Text that will be displayed](Link "Tooltip")`| [Text that will be displayed](Link "Tooltip")|

If you want the link to **open in a different tab**, you can use HTML code:

| **Input** | **Output** |
| ----------- | ----------- |
| `<a href="Link" target="_blank">Text that will be displayed</a>`| <a href="Link" target="_blank">Text that will be displayed</a>|<a href="Link" target="_blank">Text that will be displayed</a>| <a href="Link" target="_blank">Text that will be displayed</a>|

To quickly turn a URL or email address into a link, enclose it in angle brackets.

| **Input** | **Output** |
| ----------- | ----------- |
| `<example@example.com>`| <example@example.com>|


> <span style="color:orangered">**❗Note:** If you are having trouble with spaces in the middle of a URL, try to URL encode any spaces with `%20`. For parenthesis, try to URL encode the opening parenthesis (`(`) with `%28` and the closing parenthesis (`)`) with `%29`.

---

## 7. Images

To add an image, add an exclamation mark (`!`), followed by alt text in brackets, and the path or URL to the image asset in parentheses.

`![Alt text](Image path)`

To **add a link** to an image, enclose the Markdown for the image in brackets, and then add the link in parentheses.

`[![Alt text](Image path)](Link)`

If you want to **resize an image**, you can use HTML code:

`<img src="image.png" width="200" height="100">`

---

## 8. Displaying Markdown characters

If there is a character you want to include, but Markdown interprets it as code, add a backslash (`\`) in front of the character.

`/# This is not a header`

If you wish to display text that Markdown considers as code, you can use **plain text** add an accent (``` ` ```) before and after it.

`# This **text** is unaffected by ~~coding~~.`

---

## 9. Code blocks

If you want to display lines of code within Markdown, type four spaces before them. Any spaces you introduce after that will count as normal spaces.

Example:

        <html>
          <head>
            <title>Test</title>
          </head>

---

## 10. Collapsible content

You can add a collapsible section using HTML code.

    <details>
      <summary>Click to expand</summary>
    
      You can add any kind of text here.
    </details>

Example:

<details>
<summary>Click to expand</summary>

You can add any kind of text here.
</details>

---

## 11. Formatting text

### 11.1. Bold

To bold text, add two asterisks (`**`) before and after a word or phrase.

| **Input** | **Output** |
| ----------- | ----------- |
| `**Bold**`| **Bold**|


### 11.2. Italic

To italicize text, add one asterisk (`*`) before and after a word or phrase.

| **Input** | **Output** |
| ----------- | ----------- |
| `*Italic*`| *Italic*|


### 11.3. Bold and italic

To emphasize text with bold and italics at the same time, add three asterisks (`***`) before and after a word or phrase.

| **Input** | **Output** |
| ----------- | ----------- |
| `***Bold***` | ***Bold and italic***|

### 11.4. Strikethrough

To cross text out, add two tildes (`~~`) before and after a word or phrase.

| **Input** | **Output** |
| ----------- | ----------- |
| `~~Strikethrough~~` | ~~Strikethrough~~|

### 11.5. Color

You can add color to a text by using HTML code.

| **Input** | **Output** |
| ----------- | ----------- |
| `<span style="color:red">This text is red.</span>`| <span style="color:red">This text is red.</span>|

- [Here is a list of colors you can use](https://htmlcolorcodes.com/color-names/)