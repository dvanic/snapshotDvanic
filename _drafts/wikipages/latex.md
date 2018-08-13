---
layout: default
---

## LaTeX notes

### Beamer change background for only a subset of frames

    {\setbeamercolor{background canvas}{bg=Apricot}{}}

### Horizontal line

    \begin{center}
    \line(1,0){250}
    \end{center}


### Insert pages from external pdf

    \usepackage{pdfpages}

Entire pdf
    
    \includepdf[pages={-}]{myfile.pdf}

For specific pages

    \includepdf[pages={1,3,5}]{myfile.pdf}

### Sizes

	1.	\tiny
	2.	\scriptsize
	3.	\footnotesize
	4.	\small
	5.	\normalsize
	6.	\large
	7.	\Large
	8.	\LARGE
	9.	\huge
	10.	\Huge

### Symbols

    « \guillemotleft
    » \guillemotright

Nice tilde

    {\raise.17ex\hbox{$\scriptstyle\sim$}}
