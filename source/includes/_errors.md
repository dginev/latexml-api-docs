# Errors

<aside class="notice">
This error section is stored in a separate file in <code>includes/_errors.md</code>. Slate allows you to optionally separate out your docs into many files...just save them to the <code>includes</code> folder and add them to the top of your <code>index.md</code>'s frontmatter. Files are included in the order listed.
</aside>

LaTeXML has a sizeable collection of different errors, which have evolved over time from stress testing the application over the 1+ million articles in arXiv.org. The general convention is introduced in the manual [link here](https://dlmf.nist.gov/LaTeXML/manual/errorcodes/). In this cookbook, we try to enumerate the intuitions of each concrete error message.

## Status Codes

Each conversion ends with a given severity, or status, coded as:

Severity/Status | Code
----------------| ----
no_problem      | 0
warning         | 1
error           | 2
fatal           | 3

## No Problems

When no error-level messages are emitted during conversion and post-processing, the results is considered to have "No Obvious Problems" and terminates successfully.

## Warning

Severity | Category   | What | Description
-------------- | -------    | ---- | ------
Warning        | expected   | `<metadata>` | expected metadata was not found, such as the refered targets of labels, sources of graphics inclusions, or missing numeric arguments for some assignments (treated as zero)
Warning        | imageprocessing      | Crop | empty images can not be cropped, minor, ignored.
Warning        | latex      | \GenericWarning | Usually when a `.sty` or `.cls` file is interpreted raw, and latexml triggers a warning coded by the original package author.
Warning        | limitation      | `<filename>` | Usually a complex transform limitation in `Post::Graphics`, operation may be ignored.
Warning        | malformed      | labels | No open node can be assigned labels, so the labels in question will be dropped.
Warning        | misdefined      | `\command` | minor convention violations, e.g. a conditional macro being defined without starting with an `\if` prefix.
Warning        | missing_file | `<filename>` | A raw TeX dependency (usually `.sty`, `.cls` or `.def`) can not be loaded. Either the dependency is missing from the system entirely, or when `--includestyles` is **not** specified, a `.ltxml` binding is missing.
Warning        | not_parsed | `<GRAMMAR_ROLES+>` | A warning emitted when latexml's MathGrammar fails to construct a tree for a given TeX formula. The error includes a window of the grammar lexemes where the parse failed to proceed.
Warning        | perl      | warn | A native perl warning that did not have a dedicated handler, e.g. redefined subroutines; wrong offsets in indexing
Warning        | undefined      | `<name>` | an undefined construct with minor impact was encountered, commonly a TeX counter
Warning        | unexpected | `<construct>` | A macro, value, XML element, or inclusion was not expected in the current context, but a clear recovery path is known.
Warning        | uninitialized      | `<$value>`| Caught a native perl warning when encountering operations over uninitialized variables

## Error

Severity | Category   | What | Description
-------------- | -------    | ---- | ------
Error          | expected   | `<token>` | Cases where a mandatory argument was necessary, but no argument was available. Processing proceeds with an empty or default value.
Error          | I/O        | `<filename>` | Could not read auxiliary file, ignoring when caught.
Error          | imageprocessing | `Read|Scale|Crop` | image processing operation failed, potentially due to image file not being found, or drivers missing
Error          | latex      | \GenericError | Usually seen when a `.sty` or `.cls` file is interpreted raw, and latexml triggers an error coded by the original package author. Generally due to latexml mistakes in emulating pdflatex.
Error          | malformed  | `<ns:element>` | The document construction phase could not use the given element in the document fragment built so far, as enforced by the latexml XML schema. Usually a result of a previous processing error, sometimes a discrepancy between the TeX boxing model and the XML tree paradigm.
Error          | misdefined | `<token>` | A token was not meaningful in the current context (e.g. it reached the stomach without a definition, or was not part of a mini language for a package, as in an RGB color specification). Usually processing ignores the token and proceeds.
Error          | missing_file  | `<filename>` | can't read an auxiliary file for a binding macro, usually left ignored as empty.
Error          | pgfparse   | pgfparse | failure to parse a pgf expression, either due to bad syntax, or latexml binding deficiency
Error          | recursion  | `\command` | Macro may be defined as expanding into itself, which would be an infinite loop if unchecked. Expansion is ignored when caught.
Error          | undefined  | `\command` | An error where a TeX `\command` was attempted to be digested without first being defined. While this could be an author error, it is also common where prerequisite `.sty` or `.cls` libraries were not present or failed to correctly load.
Error          | unexpected | `\command` or `<token>` | Processing did not expect the content at this expansion point. Any of: ending a wrong mode or environment; using a construct in the wrong TeX mode; or expanding illegal content in restricted processing (such as inside `\csname`).

## Fatal

Severity | Category   | What | Description
-------------- | -------    | ---- | ------
Fatal          | die | `<filename>` | A native perl death, as in `Fatal:perl:die`, caught by a different handler (to be refactored)
Fatal          | expected | `<tokens>` | Expected specific tokens that were not found, usually as part of a matching the signature of a TeX macro's arguments.
Fatal          | I/O | unreadable | It was impossible to read a mandatory file
Fatal          | internal | `<recursion>` | Detected an infinite recursion  during TeX expansion, leading to a hard abort.
Fatal          | invalid  | binary        | main TeX source seems to have been a binary file, abort.
Fatal          | invalid  | archive       | did not detect a main TeX source when examining an input archive, abort.
Fatal          | misdefined | `\command` | The defined expansion of the macro in question was considered unusable (e.g. unbalanced {})
Fatal          | missing_file | `<filename>` | The main TeX file was missing from the file system
Fatal          | perl | deep_recursion | Perl exceeded its maximum allowed recursion depth in native subroutine calls. This could indicate an infinite loop, so is treated as a hard abort.
Fatal          | perl | die | critical error in native Perl, usually preceded by a cascade of lighter errors with TeX interpretation
Fatal          | timeout | timedout | Took longer than the alloted `--timeout` seconds, and aborted. |
Fatal          | too_many_errors | 100 | When latexml encounters 100 errors, it considers the document too badly broken to continue, and terminates early without creating any output.
Fatal          | unexpected | `<endgroup>` | Conversion ran into too many closing groups, the last of which was locked and should have never been ended
