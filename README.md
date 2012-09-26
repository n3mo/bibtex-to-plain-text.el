# bibtex-to-plain-text.el

Tools for quickly creating plain text bibliographic references from BibTeX entries

The initial purpose of this package was to enable easy conversion from BibTeX/LaTeX citations into a plain text format that can be easily pasted into other programs that are unfriendly to LaTeX (or for quickly sharing references with colleagues or friends through email, etc.). 

Currently, this package allows you to easily convert either BibTeX formatted text, or latex \cite{} commands in a buffer into plain text APA formatted references. Of course, formatting such as underlining and italics will be missing from the reference string. As of now, if you plan to paste the formatted string into another program (such as M$ Word), you will need to manually add such markup. Future versions will hopefully remove this necessity. 

bibtex-to-plain-text is agnostic to the referencing style used as output. The default is APA styling. Styling behavior is controlled by the variable <code>bibtex-to-plain-text-style</code>, which is set by default to an association list called <code>bibtex-to-plain-text-apa-formats</code> that contains the formatting styles for each BibTeX entry type (article, book, incollection, etc.). Additional styles (MLA, Chicago style, etc.) will be added in time. Users can also provide their own custom style rules by creating custom association lists according to those defined in bibtex-to-plain-text.el. By setting <code>bibtex-to-plain-text-style</code> to the custom list, the output behavior of bibtex-to-plain-text can be modified:

<pre>
(setq bibtex-to-plain-text-style bibtex-to-plain-text-my-custom-format-list)
</pre>

# Setup

This package requires that <code>bibtex</code> and <code>reftex-cite</code> are installed.

Provided bibtex-to-plain-text.el is on your load path, a simple require will get you up and running.

<pre>
(require 'bibtex-to-plain-text)
</pre>

To use the <code>latex-</code> commands below, you will need to ensure that your LaTeX/BibTeX installations are set up properly. Most importantly, your .bib file(s) specified in your LaTeX <code>\bibliography{}</code> statements should be on the correct path. This is not a requirement for the <code>bibtex-</code> commands.

# Usage

The package makes several functions available. <i>Expect these functions to be renamed soon.</i>

## bibtex-create-plain-text-reference

This converts a BibTeX entry under point (selecting a region is NOT required) to a plain text reference. The result is pushed to the kill ring. Example:

<pre>
@article{petrov2011,
  title={Dissociable perceptual-learning mechanisms revealed by diffusion-model analysis},
  author={Petrov, A.A. and Van Horn, N.M. and Ratcliff, R.},
  journal={Psychonomic bulletin \& review},
  volume={18},
  number={3},
  pages={490--497},
  year={2011},
  publisher={Springer}
}
</pre>

Running <code>bibtex-create-plain-text-reference</code> with point anywhere inside the above BibTeX entry will produce the following APA reference:

<pre>
Petrov, A.A. and Van Horn, N.M. and Ratcliff, R. (2011). Dissociable perceptual-learning mechanisms revealed by diffusion-model analysis. Psychonomic Bulletin & Review, 18(3), 490-497.
</pre>

## bibtex-convert-buffer-to-plain-text

This is a wrapper for <code>bibtex-create-plain-text-reference</code> described above. The current buffer is searched and all BibTeX entries are converted to plain text. The results are written to a buffer named /*references/*. The contents of the current buffer can contain a mixture of BibTeX markup and other text. Thus, <code>bibtex-convert-buffer-to-plain-text</code> will scrape your buffer of any BibTeX entries and convert them into a references list.

## latex-convert-buffer-to-plain-text

This function should be run inside of a LaTeX document containing one or more <code>\cite{}</code> commands. Provided you have specified a bibliography file in <code>\bibliography{}</code> statement, the function will create a plain text reference list formatted according to the standards associated with the reference type of each citation (e.g., article, book, etc. will be treated uniquely).

## latex-create-plain-text-reference

<b>Currently not working properly!!!</b>

This function creates a plain text formatted reference of the <code>\cite{}</code> entry under point. The reference is pushed to the kill ring.

# Future Goals

* Add exporting options with markup text (for pasting properly formatted references into .rtf, html, etc.). 
* Convert from plain text references back into BibTeX. This is a tough problem probably not worth the timeinvolved.

# Warning 

This is in early development. The initial functions seem to work, but there are known bugs. For instance, the final BibTeX entry in a buffer is converted to a plain text reference twice. 
