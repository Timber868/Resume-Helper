# Cover letter: build cover.pdf from cover.tex
# Run from this directory: make

LATEX    = pdflatex
LATEXOPT = -interaction=nonstopmode -file-line-error
MAIN     = cover

.PHONY: all clean

all: $(MAIN).pdf

$(MAIN).pdf: $(MAIN).tex info.tex body.tex
	$(LATEX) $(LATEXOPT) $(MAIN).tex
	$(LATEX) $(LATEXOPT) $(MAIN).tex

clean:
	rm -f $(MAIN).aux $(MAIN).log $(MAIN).out $(MAIN).pdf
