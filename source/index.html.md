---
title: LaTeXML Cookbook

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - perl
  - http

toc_footers:
  - <a href='https://latexml.mathweb.org/editor'>LaTeXML showcase</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# WORK IN PROGRESS

# Introduction

Welcome to a practical cookbook of LaTeXML-related invocations, and customization switches. This resource contains a variety of recipes for using LaTeXML and its ecosystem, as well as a full enumeration of the public API

# Basic use

The most common use pattern is to produce an HTML5 equivalent for a given TeX input, which we'll use to get started.

> We start with a simple hello world snippet

```tex
% file.tex
\documentclass{article}
\begin{document}
Hello World!
\end{document}
```

> And convert:

```perl
use LaTeXML;
use LaTeXML::Common::Config;

my $config = LaTeXML::Common::Config->new(format=>'html5');
my $converter = LaTeXML->get_converter($config);

$response = $converter->convert('file.tex');

my ($result, $log, $status, $status_code) = map {$$response{$_}} qw(result log status status_code);
```

```shell
latexml file.tex --dest=file.xml
latexmlpost file.xml --dest=file.html

# Alternatively, latexmlc = latexml+latexmlpost+(optional http client)
latexmlc file.tex --dest=file.html
```

```http
POST /convert HTTP/1.1
Host: latexml.mathweb.org
Content-Type: application/x-www-form-urlencoded
Content-Length: 30

tex=Hello%20World!&format=html
```

In a nutshell, LaTeXML will always start with an input in the TeX/LaTeX ecosystem, and map it into a target format of choice, with a wide range of choices for customizing individual aspects of the conversion. This documentation effort will try to enumerate as many as possible from the pragmatic uses of LaTeXML we know of, with brief explanations of the exact invocation.

<aside class="notice">
You can use the Perl API once you have a native installation of LaTeXML, e.g. via a package manager, cpanminus, or cpan. See <a href="https://dlmf.nist.gov/LaTeXML/get.html">the official installation docs</a>for details.
</aside>
<aside class="notice">
The HTTP request examples are based on an endpoint of the plugin <a href="https://github.com/dginev/LaTeXML-Plugin-ltxmojo">LaTeXML::Plugin::ltxmojo</a>, which also contains a showcase web editor.
</aside>

LaTeXML can be used with different degrees of complexity, proportionally to the complexity of the input text. Certain questions only become relevant with document size (e.g. cross-linking multiple web pages corresponding to book chapters), while others become relevant with input volume (e.g. converting millions of individual formulas from Wikipedia).

<aside class="notice">
The latexmlc, HTTP and Perl API examples all rely on the same concrete implementation, realized via the LaTeXML.pm interface.
</aside>

# Converting a single formula

On the command side, you have the choice between using the dedicated out-of-the-box `latexmlmath` executable, or the omni-executable `latexmlc`, for "formula-to-formula" conversions. We will provide examples using both `latexmlmath` and `latexmlc` where possible.

## To Presentation MathML

```shell
# The general form of latexmlmath is:
latexmlmath '\sqrt{x}'

# The equivalent formula-to-formula call of latexmlc is:
latexmlc --whatsin=math --whatsout=math --format=html 'literal:\sqrt{x}'
```

```perl
my $converter = LaTeXML->get_converter(LaTeXML::Common::Config->new(
  whatsin=>'math', whatsout=>'math', format=>'html' ));

$converter->convert('literal:\sqrt{x}');
```

```http
POST /convert HTTP/1.1
Host: latexml.mathweb.org
Content-Type: application/x-www-form-urlencoded
Content-Length: 57

tex=%5Csqrt%7Bx%7D&whatsin=math&whatsout=math&format=html
```

> Result:

```xml
<math xmlns="http://www.w3.org/1998/Math/MathML" alttext="\sqrt{x}" display="block">
  <msqrt>
    <mi>x</mi>
  </msqrt>
</math>
```

Presentation MathML is the default output of `latexmlmath`, and is also the default serialization for all of LaTeXML's post-processing formats (ePub, (X)HTML, JATS, ...). Instead, the defaults for `latexml` and `latexmlc` target LaTeXML's own XML schema, as they do not include post-processing in a default run.


## To full MathML (presentation+content)

```shell
# Cross-referenced presentation and content in a single formula output:
latexmlc --pmml --cmml --whatsin=math --whatsout=math --format=html 'literal:\sqrt{x}'

# Alternatively, separate presentation and content written in single-formula files:
latexmlmath --pmml=sqrt.pmml --cmml=sqrt.cmml '\sqrt{x}'
```

## To full MathML with annotations

## To SVG image

## To PNG image

# Short article

# Book-sized manuscript

# Advanced Uses for Mathematics

# Advanced Uses for Formats (EPUB, JATS)

# Batch processing


# API

## All Customization switches
...