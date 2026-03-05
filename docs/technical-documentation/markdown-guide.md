# Markdown guide

!!! info "Introduction"
    Markdown is a lightweight markup language used to format text using a plain-text syntax. It is widely used in documentation, README files, forums, and static websites. Some site generators, such as MkDocs (the framework used to create this website), are designed to create sites from Markdown.
  
    This guide covers the most common Markdown features and includes examples of their syntax and rendered output. Please note this is not a comprehensive guide.

!!! tip "Short on time? [Click here for a quick Markdown cheat sheet](#13-cheat-sheet)."

## 1. Headings

To create a heading, type number signs (`#`) in front of a word or phrase. The number of signs you use corresponds to the heading level.

!!! warning "Always put a space between the number signs and the heading name."

```markdown
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6
```

---

## 2. Text formatting

### 2.1. Bold

To make text bold, add two asterisks (`**`) before and after a word or phrase.

| **Input** | **Output** |
| ----------- | ----------- |
| `**Bold**`| **Bold**|

### 2.2. Italic

To italicize text, add one asterisk (`*`) before and after a word or phrase.

| **Input** | **Output** |
| ----------- | ----------- |
| `*Italic*`| *Italic*|

### 2.3. Bold and italic

To emphasize text with bold and italics at the same time, add three asterisks (`***`) before and after a word or phrase.

| **Input** | **Output** |
| ----------- | ----------- |
| `***Bold***` | ***Bold and italic***|

### 2.4. Strikethrough

To cross text out, add two tildes (`~~`) before and after a word or phrase.

| **Input** | **Output** |
| ----------- | ----------- |
| `~~Strikethrough~~` | ~~Strikethrough~~|

### 2.5. Color

If your site supports HTML code, you can add color using the following HTML tags:

| **Input** | **Output** |
| ----------- | ----------- |
| `<span style="color:red">This text is red.</span>`| <span style="color:red">This text is red.</span>|

- [Here is a list of colors you can use](https://htmlcolorcodes.com/color-names/)

---

## 3. Blockquotes

To create a blockquote, add a `>` in front of a paragraph. Use it to emphasize important information, like warnings, notes, tips...

`> Blockquote`

!!! warning "Always put blank lines before and after blockquotes."

If you want the blockquote to contain multiple paragraphs, add a `>` on the blank lines between the paragraphs.

```markdown
> Paragraph 1
>
> Paragraph 2
```

Example:
> Paragraph 1
>
> Paragraph 2

To nest a blockquote within a blockquote, use >>.

```markdown
> Main paragraph
>
>> Nested blockquote
```

Example:
> Main paragraph
>
>> Nested blockquote

---

## 4. Lists

You can organize items into ordered and unordered lists.

### 4.1. Ordered lists

To create an ordered list, add line items with numbers followed by periods.

!!! tip "The numbers don’t have to be in numerical order, as Markdown will automatically input them in the correct sequence, but the list should start with the number one."

```markdown
1. First item
1. Second item
1. Third item
```

To create a nested list, add two spaces before the sub-items.

```markdown
1. First item
  1.1. First sub-item
  1.2. Second sub-item
```

### 4.2. Unordered lists

To create an unordered list, add dashes (`-`), in front of the line items.

```markdown
- First item
- Second item
- Third item
```

To create a nested list, type two spaces before the dash (`-`). To create a nested list within a nested list, add two more spaces, and so on.

Example:

```markdown
  - Item 1
    - Item 1.1
        - Item 1.1.1
          - Item 1.1.1.1
    - Item 2
```

!!! tip "If you need to start an unordered list item with a number followed by a period, you can use a backslash (`\`) to avoid creating an ordered list."

### 4.3. Task lists

To create a task list, add `- [ ]` before incomplete tasks and `- [x]` before completed tasks.

Example:

```markdown
- [x] Completed task
- [ ] Incomplete task
```

### 4.4. Mixed lists

You can merge elements from the previous lists to create mixed lists.

Example:

```markdown
1. Item
2. Item
   - Sub item
   - Sub item
```

---

## 5. Links

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
| `<a href="Link" target="_blank">Text that will be displayed</a>`| <a href="Link" target="_blank">Text that will be displayed</a>|

To automatically convert a URL or email address into a link, enclose it in angle brackets.

| **Input** | **Output** |
| ----------- | ----------- |
| `<example@example.com>`| <example@example.com>|

!!! tip "If you are having trouble with spaces in the middle of a URL, try to URL encode any spaces with `%20`. For parenthesis, try to URL encode the opening parenthesis (`(`) with `%28` and the closing parenthesis (`)`) with `%29`."

---

## 6. Images

To add an image, add an exclamation mark (`!`), followed by alt text in brackets, and the path or URL to the image asset in parentheses.

`![Alt text](Image path)`

To **add a link** to an image, enclose the Markdown for the image in brackets, and then add the link in parentheses.

`[![Alt text](Image path)](Link)`

If you want to **resize an image**, you can use HTML code:

`<img src="image.png" width="200" height="100">`

---

## 7. Tables

To add a table, use three or more hyphens (`---`) to create each column’s header, and use pipes (`|`) to separate each column. For compatibility, you should also add a pipe on either end of the row.

**Input:**

  | Syntax | Description |
  | ----------- | ----------- |
  | Heading | Title |
  | Paragraph | Text |

**Output:**

| Syntax | Description |
| ----------- | ----------- |
| Heading | Title |
| Paragraph | Text |

You can align text in the columns to the left, right, or center by adding a colon (`:`) to the left, right, or on both side of the hyphens within the heading row.

**Input:**

  | Syntax | Description | Test Text |
  | :--- | :----: | ---: |
  | Heading | Title | Here's this |
  | Paragraph | Text | And more |

**Output:**

| Syntax | Description | Test Text |
| :--- | :----: | ---: |
| Heading | Title | Here's this |
| Paragraph | Text | And more |

---

## 8. Code blocks

### 8.1. Inline code

To highlight code within text, add backticks (``` ` ```) around the code.

```markdown
Use the `git commit` command.
```

Example:

Use the `git commit` command.

### 8.2. Fenced code blocks

To display lines of code within Markdown, add` ``` `on the lines before and after the code.

!!! tip "Syntax highlighting"
    If you want to highlight the syntax specific to the displayed code, add the name after the /``` on the first line.
    Example: ```java

```html
<html>
  <head>
    <title>Test</title>
  </head>
</html>
```

### 8.3. Indented code blocks

You can get the same result as fenced code blocks adding four spaces before each line of code.

```markdown
    Code example
```

---

## 9. Escaping characters

To add a character that Markdown interprets as code, add a backslash (`\`) in front of the character.

`\# This is not a heading`

Example:

\# This is not a heading

If you wish to display Markdown code as plain text, add backticks (``` ` ```) around the code.

`# This **text** is unaffected by ~~coding~~.`

---

## 10. Separators

To create a separator, type three dashes (`-`) on a separate line. Use it to separate differentiated sections.

`---`

!!! warning "Always enter blank lines before and after separators."

---

## 11. Collapsible content (HTML)

If your site supports HTML code, you can add a collapsible section using HTML code.

```html
    <details>
      <summary>Click to expand</summary>
    
      You can add any text here.
    </details>
```

Example:

<details>
<summary>Click to expand</summary>

You can add any text here.
</details>

---

## 12. GitHub features

Markdown is often used on GitHub. These are some features you can use in this framework.

### 12.1. Footnotes

To add a footnote, follow this example:

```markdown
Here is a sentence with a footnote.[^1]

[^1]: Footnote text.
```

### 12.2 Blockquotes

Use the following examples for the type of content they describe:

```markdown
[!NOTE]
Information users should keep in mind, even when skimming.

[!TIP]
Optional guidance to help users work more effectively.

[!IMPORTANT]
Key information users need to complete a task successfully.

[!WARNING]
Alerts users to serious risks that require immediate attention.

[!CAUTION]
Warns about possible negative results of an action.
```

## 13. Cheat sheet

Quick reference for the most common Markdown features:

## 13.1. Headings

| Element   | Markdown           |
| --------- | ------------------ |
| Heading 1 | `# Heading 1`      |
| Heading 2 | `## Heading 2`     |
| Heading 3 | `### Heading 3`    |
| Heading 4 | `#### Heading 4`   |
| Heading 5 | `##### Heading 5`  |
| Heading 6 | `###### Heading 6` |

## 13.2. Text formatting

| Element       | Markdown     | Example    |
| ------------- | ------------ | ---------- |
| Bold          | `**text**`   | **text**   |
| Italic        | `*text*`     | *text*     |
| Bold + Italic | `***text***` | ***text*** |
| Strikethrough | `~~text~~`   | ~~text~~   |
| Inline code   | `` `code` `` | `code`     |

## 13.3. Blockquotes

| Element           | Markdown           |
| ----------------- | ------------------ |
| Blockquote        | `> Quote`          |
| Nested blockquote | `> > Nested quote` |

## 13.4. Lists

## 13.4.1. Ordered lists

| Markdown         |
| ---------------- |
| `1. First item`  |
| `2. Second item` |
| `3. Third item`  |

## 13.4.2. Unordered lists

| Markdown |
| -------- |
| `- Item` |
| `- Item` |
| `- Item` |

## 13.4.3. Task lists

| Markdown |
| -------- |
| `- [ ] Item` |
| `- [x] Item` |
| `- [ ] Item` |

## 13.5. Links

| Element           | Markdown                |
| ----------------- | ----------------------- |
| Basic link        | `[Text](URL)`           |
| Link with tooltip | `[Text](URL "Tooltip")` |
| Automatic link    | `<https://example.com>` |
| Email link        | `<example@example.com>` |

## 13.6. Images

| Element      | Markdown                        |
| ------------ | ------------------------------- |
| Image        | `![Alt text](image.png)`        |
| Linked image | `[![Alt text](image.png)](URL)` |

## 13.7. Tables

```markdown
| Syntax | Description |
| ----------- | ----------- |
| Heading | Title |
| Paragraph | Text |
```

## 13.8. Code blocks

## 13.8.1. Inline code

`code`

## 13.8.2. Fenced code block

```html
<html>
  <head>
    <title>Example</title>
  </head>
</html>
```

## 13.8.3. Indented code block

  Code example

## 13.9. Escaping characters

| Markdown     | Result    |
| ------------ | --------- |
| `\# Heading` | # Heading |
| `\*text\*`   | *text*    |

## 13.10. Separators

| Element         | Markdown |
| --------------- | -------- |
| Horizontal rule | `---`    |

## 13.11. Collapsible content (HTML)

<details>
  <summary>Click to expand</summary>

  You can add any text here.
</details>

## 13.12. HTML inside Markdown

| Feature              | Example                                  |
| -------------------- | ---------------------------------------- |
| Text color           | `<span style="color:red">Text</span>`    |
| Image resizing       | `<img src="image.png" width="200">`      |
| Open link in new tab | `<a href="URL" target="_blank">Text</a>` |
