# Errors

<aside class="notice">
This error section is stored in a separate file in <code>includes/_errors.md</code>. Slate allows you to optionally separate out your docs into many files...just save them to the <code>includes</code> folder and add them to the top of your <code>index.md</code>'s frontmatter. Files are included in the order listed.
</aside>

LaTeXML has a sizeable collection of different errors, which have evolved over time from stress testing the application over the 1+ million articles in arXiv.org. The general convention is introduced in the manual [link here](https://dlmf.nist.gov/LaTeXML/manual/errorcodes/). In this cookbook, we try to enumerate the intuitions of each concrete error message.


Error Severity | Category   | What | Description
-------------- | -------    | ---- | ------
Warning        | not_parsed | <GRAMMAR_ROLES+> | A warning emitted when latexml's MathGrammar fails to construct a tree for a given TeX formula. The error includes a window of the grammar lexemes where the parse failed to proceed.
Error          | undefined  | `\command` | An error where a TeX `\command` was attempted to be digested without first being defined. While this could be an author error, it is also common where prerequisite `.sty` or `.cls` libraries were not present or failed to correctly load.
Fatal          | too_many_errors | 100 | When latexml encounters 100 errors, it considers the document too badly broken to continue, and terminates early without creating any output.