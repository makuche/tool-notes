Embedding a PDF figure/graph in a TeX document that automatically adapts/converts fonts can be achieved with the following steps:

1. Create file in inkscape. Once finished, change the document width in "Document Properties" to `Width: 16.5 cm`. This avoids any re-scaling when embedding the document into the latex document.
2. Save the document with `Save as...` and choose the file option `figure.pdf`. In the next pop up window, choose the Text output option `Omit text in PDF and create LaTeX file`. Keep the resolution at `300 dpi`. Saving the file will produce two files, a `figure.pdf_tex` and a `figure.pdf` file.
3. Add both files to the latex project folder. Within the latex document, use the following to include the figure:
```
\begin{figure}[ht!]
    \centering
    \incfig{figure}
    \caption{TODO}    % optional
    \label{fig:TODO}  % optional
\end{figure}
```
4. (This has to be done only once) Import required packages and add the following command to the beginning of your document or to your `config.tex`:
```
\usepackage{import}
\usepackage{xifthen}
\usepackage{pdfpages}
\usepackage{transparent}

\newcommand{\incfig}[1]{%
    \def\svgwidth{\columnwidth}
    \import{./images/}{#1.pdf_tex}
}
```

That's it!
This has the following advantages:
- Fonts in figures change according to the document style (no need to update figures manually)
- Math environment can be used directly within inkscape
- Clean look
- Document requires less storage
