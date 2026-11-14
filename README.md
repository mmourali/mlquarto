# mlquarto: Quarto Extension for *Marketing Letters*

A [Quarto](https://quarto.org/) extension that formats manuscripts for *Marketing Letters*, published by Springer. One `.qmd` file renders to three outputs:

| Format | Output | Use |
| --- | --- | --- |
| `mlquarto-docx` | Word file | An editable source file: the guidelines ask for manuscripts in Word |
| `mlquarto-latex` | `.tex` source of the PDF | For manuscripts with mathematical content, which may be submitted in LaTeX; compile with pdfLaTeX (two runs, or latexmk). Rendering the PDF also keeps this file. |
| `mlquarto-pdf` | PDF typeset with pdfLaTeX | For sharing and checking the manuscript |

Version 1.0.0. The formatting follows the journal's [submission guidelines](https://link.springer.com/journal/11002/submission-guidelines) as they stood on 13 November 2026, except where noted under [Choices the guidelines leave open](#choices-the-guidelines-leave-open). On that day the page carried a notice that the journal had moved to a new submission system and that the guidelines were being revised, so compare this README with the page before you submit.

## Installation

Start a new manuscript from the template:

```bash
quarto use template mmourali/mlquarto
```

Or add the extension to an existing project:

```bash
quarto add mmourali/mlquarto
```

## The files Marketing Letters asks for

*Marketing Letters* uses double-anonymous review. Its submission system asks for the author details and for several statements in its own fields, so that they stay out of the manuscript.

| File | Template | Mode |
| --- | --- | --- |
| Anonymous manuscript | `template.qmd` | `ml-mode: manuscript` (the default) |
| Online-only appendix (optional) | `online-appendix.qmd` | `ml-mode: online-appendix` |
| Author details and statements, to have at hand | `title-page.qmd`, with the details in `_title-page.yml` | `ml-mode: title-page` |

```bash
quarto render template.qmd          # manuscript: Word, PDF, and .tex
quarto render online-appendix.qmd   # online appendix
quarto render title-page.qmd        # author details and statements
```

**Manuscript.** US letter, 1-inch margins, Times New Roman 12 point, double spaced, with page numbers. The first page holds the title, the abstract, and the keywords, and the text begins on the next page. The limits are short: at most 5,000 words, including the abstract, the main text, and the references; an abstract of at most 150 words; 4 to 6 keywords; and up to four tables and figures in total. Each render reports the word count and warns when a limit is exceeded.

**What the manuscript for review leaves out.** Springer's [instructions for double-anonymous review](https://www.springernature.com/gp/snapp/submitting/how-to-submit/double-anonymous) ask authors to keep out of the manuscript their names, affiliations, and e-mail addresses, and also their acknowledgments, contribution statements, competing interest statement, ethics statement, and funding information. The submission system asks for these, and what is entered there goes into the published version. The manuscript for review therefore prints one statement, on data availability, under "Statements and Declarations." Refer to your own earlier work in the third person, and anonymize any data, code, or supplementary file that reviewers will see.

**Author details and statements.** `title-page.qmd` prints what the guidelines list for a title page: the title; the names of the authors; their affiliations (institution, department, city, state, country); the e-mail address of the corresponding author; ORCIDs, if available; acknowledgments; funding; and the statements on competing interests, ethics approval, consent, and author contributions. All of it is entered in `_title-page.yml`. The page gathers in one place what the submission system asks for.

**Online appendix.** Online-only appendixes accompany a submission as a separate file, and the guidelines limit them to 2,000 words; the render reports the word count of the file. Its tables, figures, and equations are numbered on their own, apart from those of the article. `ml-appendix-prefix: "S"` adds a prefix to the numbers (Table S1, Fig. S1).

**Replication Corner.** With `ml-article-type: replication`, the limits are those of a Replication Corner submission: 4,000 words and up to three tables and figures.

**Article mode** (`ml-mode: article`, PDF and `.tex` only) is a publication-style preview for sharing drafts. It is not a submission format.

## Options

| Field | What the extension does |
| --- | --- |
| `title` | Printed centered and bold |
| `abstract` | Printed under "Abstract." Warns above 150 words. |
| `keywords` | Printed after the abstract. Warns unless there are 4 to 6. |
| `ml-mode` | `manuscript` (default), `online-appendix`, `title-page`, or `article` |
| `ml-article-type` | `article` (default) or `replication` (a Replication Corner submission) |
| `declarations` | `data-availability` is printed under "Statements and Declarations" immediately before the references. `funding`, `competing-interests`, `ethics`, `consent`, and `author-contributions` are printed on the title page, and in the manuscript only with `anonymous: false`. |
| `ml-citation-style` | `apa` (default) or `springer`; see [Citations and references](#citations-and-references) |
| `ml-appendix-prefix` | A prefix for the numbers of an online appendix (none by default) |
| `ml-floats` | `inline` (default) leaves tables and figures in the text, where the guidelines ask for figures. `end` moves each to a separate page after the references and marks its place in the text. |
| `anonymous` | `true` (default). `false` prints the title and the authors on a first page, and the acknowledgments and every statement before the references. |

## Text

**Headings.** The guidelines ask for the decimal system of headings with no more than three levels. Sections are numbered 1, 1.1, 1.1.1, without a period after the number, as in the journal's articles, and each heading stands on its own line: `#` and `##` in bold, `###` in italics. The abstract, the statements, the appendixes, and the references are not numbered. Write the main sections with one `#`. (If a document has no `#` heading at all, its `##` headings are taken as the main sections and every heading moves up one level.) The render warns about a fourth level.

**Hypotheses.** Write each hypothesis in a `hypothesis` div. It is indented, and its label (the words up to the first colon) is set in bold.

**Equations.** Equations are centered, with the number in parentheses at the right margin. In the text, `@eq-model` prints "Equation 1."

**Footnotes.** Footnotes appear at the bottom of the page; the guidelines ask for footnotes, not endnotes. A footnote should not consist solely of a reference citation, and it should contain no bibliographic details, figures, or tables.

**Appendixes.** Online-only appendixes are a separate file (see above). For an appendix inside the manuscript, give its heading the `appendix` class. It begins on a new page; the headings inside it carry no numbers; and its tables and figures continue the numbers of the text, because the guidelines ask authors not to number appendix figures A1, A2, and so on.

```markdown
# Appendix: Survey items {.appendix}
```

## Tables and figures

Give each table and figure a label and type its title without a number and without a final period. Captions print as "**Table 1** Title" above a table and "**Fig. 1** Caption" below a figure, flush left, with no punctuation after the number or at the end, as the guidelines ask. In the text, `@tbl-descriptives` prints "Table 1" and `@fig-loyalty` prints "Fig. 1." A paper may have up to four tables and figures in total; the render counts them and warns about a fifth.

```markdown
| Variable         | M    | SD   |
|:-----------------|-----:|-----:|
| Brand loyalty^a^ | 4.21 | 1.02 |
| Influencer trust | 4.56 | 0.89 |

: Descriptive statistics for the study variables {#tbl-descriptives}

::: {.notes}
All variables were measured on seven-point scales.

^a^Average of four items.
:::
```

Tables and figures stand in the text, where the guidelines ask for figures. Tables are flush with the left margin and single spaced. Notes in a `notes` div are printed as typed, single spaced, under the table or figure; mark table footnotes with superscript lowercase letters, or with asterisks for significance values. The guidelines ask for Helvetica or Arial lettering of about 8 to 12 points inside figures and for no title or caption inside the picture. The template's figure is drawn with R (ggplot2); delete that chunk if you do not use R.

**Word output.** Each table is a native Word table, flush with the left margin, with column widths that fit its contents. The alignment is written into the table itself, so it does not depend on the program that opens the file. (A table that holds citations, footnotes, images, or web links is written by pandoc in the ordinary way and takes its alignment from the table style of the reference document.) A numbered equation and its number are placed in a table without borders, the equation in the middle cell and the number in the right cell, so that the number stands at the right margin. An equation without a number is centered in a paragraph of its own. A table or figure without a note is followed by 12 points of space, because Word otherwise sets the next paragraph directly against it.

**Labels and column widths.** A table or figure that has a title but no label is printed without a number, in every output, and the render warns; a table without a title takes no number either. In a pipe table whose lines are long, the number of dashes under each column head sets that column's share of the width. If a share is too small for the longest word of its column, the extension sets the widths from the contents instead, so that no word is broken in the middle.

**Figures and tables with panels.** Quarto's layout options set several pictures in one figure. Give the figure a label; give each panel a caption and, if the text cites single panels, a label of its own:

```markdown
::: {#fig-studies layout-ncol=2}
![Study 1](study1.png){#fig-study1}

![Study 2](study2.png){#fig-study2}

Purchase intention by condition
:::
```

All three outputs set the panels side by side, each with its caption, "(a) Study 1," centered below it, and `@fig-study1` prints "Fig. 1a." A figure with panels counts as one figure. In Word the panels stand in a table without borders. A table with several parts (a `#tbl-` div with a layout option that holds one table for each part) is set the same way, and so are the plots of a code chunk with `layout-ncol` and `fig-subcap`.

**Notes and tables from code.** A table that fits on a page is kept on one page with its title and its note, and a note stays on the page of its figure. Tables that a code chunk writes (with `knitr::kable()`, for example) are treated like tables typed in Markdown. If a chunk writes its table as a LaTeX `table` environment, as `kableExtra` and `modelsummary` can, the extension places it like the other tables, at the left margin, and formats its title and note; the layout inside the table is the package's own.

## Citations and references

The guidelines page and the journal's articles differ here. The page shows Springer's basic format: citations by name and year without a comma, (Thompson 1990), and a reference list with initials after the surnames and no punctuation around them. The articles that the journal publishes cite and list their sources in APA style, for example (Bagozzi & Yi, 1988); see Wang, Katz, Kan, and Sussman (2026), *Marketing Letters*, 37, article 49, <https://doi.org/10.1007/s11002-026-09844-3>.

The extension follows the published articles and uses APA style (7th edition) by default:

> Bagozzi, R. P., & Yi, Y. (1988). On the evaluation of structural equation models. *Journal of the Academy of Marketing Science*, *16*(1), 74–94. https://doi.org/10.1007/BF02723327

`ml-citation-style: springer` gives the format of the guidelines page, (Bagozzi and Yi 1988) in the text and in the list:

> Bagozzi RP, Yi Y (1988) On the evaluation of structural equation models. Journal of the Academy of Marketing Science 16:74–94. https://doi.org/10.1007/BF02723327

For that format, the guidelines ask for journal names abbreviated according to the ISSN List of Title Word Abbreviations and accept the full name when the abbreviation is uncertain. The style prints the full name unless the entry in the `.bib` file gives the abbreviation, for example `shortjournal = {J Consum Res}`.

The statements are printed immediately before the references, and the reference list begins on a new page. If the document has no reference list, the extension adds one.

```markdown
# References

::: {#refs}
:::
```

In the `.bib` file, give an article that is in press `year = {in press}` and no volume (with `year = {forthcoming}`, the word is printed twice); enter a working paper as `@techreport` with `type = {Working paper}` and `institution`; and wrap a word in braces to protect its capital letters in a title (`{Facebook}`).

## Choices the guidelines leave open

- **Page and type.** The guidelines ask for a plain font (they name 10-point Times Roman as an example), italics for emphasis, and page numbers. They give no paper size, margins, or line spacing. The extension uses US letter paper, 1-inch margins, Times New Roman 12 point, and double spacing, like the other extensions of this family. In the PDF and `.tex` outputs, `fontsize: 10pt` and `linestretch: 1` change the type size and the spacing; in the Word file, change the "Normal" style.
- **First page.** The title, the abstract, and the keywords stand on the first page, and the text begins on the next one.
- **Headings.** The guidelines prescribe the decimal numbers and the limit of three levels, not the look of the headings.
- **Citations and references.** The default follows the journal's articles, not the examples on the guidelines page (see above).
- **Statements.** The guidelines list the statements for a title page and for a section headed "Statements and Declarations"; Springer's instructions for double-anonymous review keep most of them out of the manuscript. The manuscript for review prints the data availability statement only, which those instructions do not exclude and which the guidelines require of original research.
- **Word count.** The limit of 5,000 words takes in the abstract, the main text, and the references. The extension counts the footnotes with the main text and leaves out tables, figures, and their notes.
- **Online appendix.** The guidelines say only that figures in online appendixes are numbered separately. The extension numbers the tables, figures, and equations of the online appendix from 1 and offers a prefix.
- **Use of AI tools.** The guidelines ask authors to document the use of a large language model in the Methods section, so the template has no separate statement for it.

## Checks during rendering

The render reports the number of words and warns above 5,000 (4,000 for a Replication Corner submission; 2,000 for an online appendix). It also warns when the abstract exceeds 150 words; when there are fewer than 4 or more than 6 keywords; when the manuscript has more than four tables and figures (three for a Replication Corner submission); and when there are more than three levels of headings. It cannot check the anonymity of the manuscript.

## Files

| File | Role |
| --- | --- |
| `_extensions/mlquarto/_extension.yml` | The three formats and their settings |
| `_extensions/mlquarto/mlquarto.lua` | Front matter, author details, statements, the online appendix, the word count, page breaks, and the checks |
| `_extensions/mlquarto/mlquarto-post.lua` | Captions, section numbers in Word, the numbers of an online appendix in Word, Word tables, and figures with panels (it runs after Quarto has numbered tables and figures) |
| `_extensions/mlquarto/ml-header.tex` | LaTeX preamble: fonts, spacing, headings, captions |
| `_extensions/mlquarto/ml-reference.docx` | Word styles |
| `_extensions/mlquarto/apa.csl` | APA style (7th edition) |
| `_extensions/mlquarto/springer-basic-author-date.csl` | Springer's basic format, for `ml-citation-style: springer` |
| `_extensions/mlquarto/tools/build_reference_docx.py` | Rebuilds the Word reference document (requires Python and python-docx) |
| `template.qmd`, `bibliography.bib` | Manuscript template and its references |
| `online-appendix.qmd` | Online appendix template |
| `title-page.qmd`, `_title-page.yml` | Author details and statements |

## Requirements

Quarto 1.4 or later (tested with Quarto 1.4.557, 1.5.57, and 1.11.9; the Word output was checked in Microsoft Word for Mac) and a TeX distribution with pdfLaTeX. The PDF and `.tex` outputs use the `mathptmx` (Times) and `helvet` packages, which come with TeX Live and TinyTeX; the Word output takes Times New Roman from `ml-reference.docx`.

Greek letters and common mathematical signs can be typed as characters (α, χ², ≤, ≠, −, ×, →, ✓). In the PDF and `.tex` outputs, which pdfLaTeX typesets, Greek letters stand upright in the text, as they do in the Word file. pdfLaTeX has no type for other scripts (Cyrillic, Chinese, Arabic) or for a letter with a combining accent (a mean typed as an x followed by a combining macron); write such symbols as math, `$\bar{x}$`. The Word output takes any character.

## Resources

- [Marketing Letters submission guidelines](https://link.springer.com/journal/11002/submission-guidelines)
- [Springer Nature: preparing a manuscript for double-anonymous review](https://www.springernature.com/gp/snapp/submitting/how-to-submit/double-anonymous)

## License

The CSL files come from the [Citation Style Language](https://citationstyles.org/) project and are licensed under [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/). All other files are available under the [MIT License](LICENSE).
