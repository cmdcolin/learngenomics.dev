PANDOC_PATH?=pandoc

SRC := $(shell fd md ../docs)
COMMITSHA := $(shell git rev-parse --short HEAD)

PACKAGE_VERSION := $(shell node -p -e "require('../package.json').version")

all: learngenomics.pdf

pdfcombined.md: $(SRC)
	echo $(SRC)
	for f in $^; do cat $$f | node tex_parser.js; done > $@

titlerev: title_rev_$(COMMITSHA).md

title_rev_$(COMMITSHA).md: title.md
	echo $(PACKAGE_VERSION)
	cat $< | sed -e 's/git-commit-sha/git revision $(COMMITSHA)/' | sed -e 's/jbrowse-version/$(PACKAGE_VERSION)/'  > $@

learngenomics.pdf: pdfcombined.md disable_float.tex title_rev_$(COMMITSHA).md listings.tex
	$(PANDOC_PATH) $(PANDOC_OPTS) -H disable_float.tex title_rev_$(COMMITSHA).md pdfcombined.md --listings -H listings.tex  --toc -o $@

cli.md: ../../products/jbrowse-cli/README.md
	cp $^ $@

clean:
	git clean -fdx .

.PHONY: clean titlerev
