# Makefile for job-specific resume builds
# Put this file in: data/jobs/<JobName>/

# LaTeX engine: change to pdflatex/latexmk if you prefer
ENGINE ?= pdflatex

# Pick the Timothe-Roma-Resume-*.tex file in this folder
JOB_TEX := $(wildcard Timothe-Roma-Resume-*.tex)
JOB_PDF := $(JOB_TEX:.tex=.pdf)

.PHONY: all resume clean

all: resume

resume: $(JOB_PDF)

%.pdf: %.tex
	$(ENGINE) -interaction=nonstopmode -halt-on-error $<

clean:
	rm -f *.aux *.log *.out *.toc *.synctex.gz