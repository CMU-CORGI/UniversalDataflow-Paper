PDFS := perf.obuf-graph.pdf perf.ibuf-graph.pdf perf.all-graph.pdf bp.all-graph.pdf \
	bp.rate.all-graph.pdf mem.all-graph.pdf mem.all_legend-graph.pdf spill.prop.all-graph.pdf \
	ipc.all-graph.pdf bc.all-graph.pdf bp.inc.all-graph.pdf util.all-graph.pdf

PDFDIR := pdf
PNGDIR := png
CFGDIR := cfg
BLDDIR := bld
DATADIR := ../data
CROPPEDPNGS := $(addprefix $(PNGDIR)/, $(PDFS:%.pdf=%-crop.png))
CROPPEDPDFS := $(addprefix $(PDFDIR)/, $(PDFS:%.pdf=%-crop.pdf)) 

DATA := $(shell find $(DATADIR) -name "*.json")

.PHONY: all prelim clean

.SECONDARY:

all: prelim $(CROPPEDPDFS) $(CROPPEDPNGS)
	@echo "Finished building figures"

prelim:
	mkdir -p $(PDFDIR) $(PNGDIR) 

clean:
	rm -rf $(BLDDIR)
	rm -rf $(PDFDIR)
	rm -rf $(PNGDIR)
	rm -rf figures-crop.pdf

$(PDFDIR)/doc_data.txt: $(PDFDIR)/figures-crop.pdf
	pdftk figures-crop.pdf burst output $(PDFDIR)/figures_%02d.pdf

$(PNGDIR)/%-crop.png: $(PDFDIR)/%-crop.pdf
	mkdir -p $(PNGDIR)
	pdftoppm $< $(PNGDIR)/$*-crop -png
	mv $(PNGDIR)/$*-crop-1.png $(PNGDIR)/$*-crop.png

$(PDFDIR)/%-crop.pdf: $(PDFDIR)/%.pdf
	pdfcrop $< && touch $@

$(PDFDIR)/figures-crop.pdf: figures.pdf
	pdfcrop figures.pdf && touch $(PDFDIR)/figures-crop.pdf

$(PDFDIR)/%-graph.pdf $(PDFDIR)/%_legend-graph.pdf: $(DATA) bar.py \
	$(CFGDIR)/%.json $(AVG)
	mkdir -p $(PDFDIR)
	python3 bar.py --cfg=$(CFGDIR)/$*.json --dest=$(PDFDIR)/$*-graph.pdf \
		--legend=$(PDFDIR)/$*_legend-graph.pdf
