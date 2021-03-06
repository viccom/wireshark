#
# Make the "Wireshark User Guide" and "Wireshark Developer Guide"
# in several formats.
# See the Readme.txt file for instructions.
#

# formatting objects processor
# Additional options to fop.
# This needs to contain at least the argument '-Xmx256m'
# fop executable is found in configure.ac.
FOP_OPTS=-Xmx256m

# Asciidoc converter
A2X_HTML_OPTS="--stylesheet=ws.css"
A2X_TEXT_OPTS=

############### YOU SHOULDN'T HAVE TO EDIT ANYTHING BELOW THIS LINE! ################

include Makefile.common

# Automatically generate .pdf files from .fo files
# Suse needs FOP_OPTS, Debian needs JAVA_OPTS
.fo.pdf:
	FOP_OPTS=$(FOP_OPTS) JAVA_OPTS=$(FOP_OPTS) $(FOP) $< $@

all: wsug wsdg release-notes

all-pdf: wsug-pdf wsdg-pdf release-notes-pdf

# Wireshark User Guide
wsug: wsug_html_chunked wsug_html

wsug-pdf: wsug user-guide-a4.pdf user-guide-us.pdf

user-guide.xml: user-guide.asciidoc $(WSUG_FILES)
	$(A2X) --verbose \
		--attribute=docinfo \
		--asciidoc-opts="--conf-file=$(srcdir)/asciidoc.conf --conf-file=$(srcdir)/asciidoctor-asciidoc.conf" \
		--no-xmllint \
		--format=docbook \
		--destination-dir=$(builddir) \
		$<

# create html single page file
wsug_html: wsug_html/index.html

wsug_html/index.html: $(WSUG_SOURCE)
	@ echo --- WSUG - HTML SINGLE PAGE ---
	mkdir -p wsug_html/wsug_graphics/toolbar
	cp $(srcdir)/wsug_graphics/*.* wsug_html/wsug_graphics
	cp $(srcdir)/common_graphics/*.* wsug_html/wsug_graphics
	cp $(srcdir)/wsug_graphics/toolbar/* wsug_html/wsug_graphics/toolbar
	cp $(srcdir)/ws.css wsug_html
	$(XSLTPROC) --stringparam base.dir wsug_html/ $(COMMON_XSLTPROC_ARGS) \
		$(WSUG_XSLTPROC_ARGS) $(SINGLE_XSLTPROC_ARGS) $< > $@
	-chmod -R og+rX wsug_html

# create html chunked page files
wsug_html_chunked: wsug_html_chunked/index.html

wsug_html_chunked/index.html: $(WSUG_SOURCE)
	@ echo --- WSUG - HTML CHUNKED ---
	mkdir -p wsug_html_chunked/wsug_graphics/toolbar
	cp $(srcdir)/wsug_graphics/*.* wsug_html_chunked/wsug_graphics
	cp $(srcdir)/common_graphics/*.* wsug_html_chunked/wsug_graphics
	cp $(srcdir)/wsug_graphics/toolbar/* wsug_html_chunked/wsug_graphics/toolbar
	cp $(srcdir)/ws.css wsug_html_chunked
	$(XSLTPROC) --stringparam base.dir wsug_html_chunked/ $(COMMON_XSLTPROC_ARGS) \
		    $(WSUG_XSLTPROC_ARGS) $(CHUNKED_XSLTPROC_ARGS) $<
	-chmod -R og+rX wsug_html_chunked

# create pdf file (through XSL-FO), portrait pages on US letter paper
# you will get lot's of errors, but that's ok
user-guide-us.fo: $(WSUG_SOURCE) custom_layer_pdf.xsl
	@ echo --- WSUG - PDF US PAPER ---
	$(XSLTPROC) --stringparam paper.type letter \
		$(COMMON_XSLTPROC_ARGS) $(WSUG_PDF_XSLTPROC_ARGS) \
		--nonet $(srcdir)/custom_layer_pdf.xsl $< > $@

# create pdf file (through XSL-FO), portrait pages on A4 paper
# you will get lot's of errors, but that's ok
user-guide-a4.fo: $(WSUG_SOURCE) custom_layer_pdf.xsl
	@ echo --- WSUG - PDF A4 PAPER ---
	$(XSLTPROC) --stringparam paper.type A4 \
		$(COMMON_XSLTPROC_ARGS) $(WSUG_PDF_XSLTPROC_ARGS) \
		--nonet $(srcdir)/custom_layer_pdf.xsl $< > $@

wsluarm: make-wsluarm.pl $(WSLUA_MODULES)
	mkdir -p wsluarm_src
	$(PERL) $(srcdir)/make-wsluarm.pl $(WSLUA_MODULES)
	touch wsluarm


# Wireshark Developer Guide
wsdg: wsdg_html_chunked wsdg_html

wsdg-pdf: wsdg developer-guide-a4.pdf developer-guide-us.pdf

developer-guide.xml: developer-guide.asciidoc $(WSDG_FILES) wsluarm
	$(A2X) --verbose \
		--attribute=docinfo \
		--attribute=build_dir=$(abs_builddir) \
		--asciidoc-opts="--conf-file=$(srcdir)/asciidoc.conf --conf-file=$(srcdir)/asciidoctor-asciidoc.conf" \
		--no-xmllint \
		--format=docbook \
		--destination-dir=$(builddir) \
		$<

# create html single page file
wsdg_html: wsdg_html/index.html

wsdg_html/index.html: $(WSDG_SOURCE)
	@ echo --- WSDG - HTML SINGLE PAGE ---
	mkdir -p wsdg_html/wsdg_graphics/toolbar
	cp $(srcdir)/wsdg_graphics/*.* wsdg_html/wsdg_graphics
	cp $(srcdir)/common_graphics/*.* wsdg_html/wsdg_graphics
	cp $(srcdir)/wsdg_graphics/toolbar/* wsdg_html/wsdg_graphics/toolbar/
	cp $(srcdir)/ws.css wsdg_html
	$(XSLTPROC) --stringparam base.dir wsdg_html/ $(COMMON_XSLTPROC_ARGS) \
		--path .:wsluarm_src \
		$(WSDG_XSLTPROC_ARGS) $(SINGLE_XSLTPROC_ARGS) $< > $@
	-chmod -R og+rX wsdg_html

# create html chunked page files
wsdg_html_chunked: wsdg_html_chunked/index.html

wsdg_html_chunked/index.html: $(WSDG_SOURCE)
	@ echo --- WSDG - HTML CHUNKED ---
	mkdir -p wsdg_html_chunked/wsdg_graphics/toolbar
	cp $(srcdir)/wsdg_graphics/*.* wsdg_html_chunked/wsdg_graphics
	cp $(srcdir)/common_graphics/*.* wsdg_html_chunked/wsdg_graphics
	cp $(srcdir)/wsdg_graphics/toolbar/* wsdg_html_chunked/wsdg_graphics/toolbar/
	cp $(srcdir)/ws.css wsdg_html_chunked
	$(XSLTPROC) --stringparam base.dir wsdg_html_chunked/ $(COMMON_XSLTPROC_ARGS) \
		--path .:wsluarm_src \
		$(WSDG_XSLTPROC_ARGS) $(CHUNKED_XSLTPROC_ARGS) $<
	-chmod -R og+rX wsdg_html_chunked

# create pdf file (through XSL-FO), portrait pages on US letter paper
# you will get lot's of errors, but that's ok
developer-guide-us.fo: $(WSDG_SOURCE) custom_layer_pdf.xsl
	@ echo --- WSDG - PDF US PAPER ---
	$(XSLTPROC) --stringparam paper.type letter \
		    $(COMMON_XSLTPROC_ARGS) $(WSDG_PDF_XSLTPROC_ARGS) \
		    --path .:wsluarm_src:$(srcdir) \
		    --nonet $(srcdir)/custom_layer_pdf.xsl $< > $@

# create pdf file (through XSL-FO), portrait pages on A4 paper
# you will get lot's of errors, but that's ok
developer-guide-a4.fo: $(WSDG_SOURCE) custom_layer_pdf.xsl
	@ echo --- WSDG - PDF A4 PAPER ---
	$(XSLTPROC) --stringparam paper.type A4 \
		    $(COMMON_XSLTPROC_ARGS) $(WSDG_PDF_XSLTPROC_ARGS) \
		    --path .:wsluarm_src:$(srcdir) \
		    --nonet $(srcdir)/custom_layer_pdf.xsl $< > $@

# Release notes
if HAVE_A2X_TEXT
release-notes: release-notes.html release-notes.txt
else
release-notes: release-notes.html
endif

release-notes-pdf: release-notes release-notes-a4.pdf release-notes-us.pdf

# create html single page file
release-notes.html: $(RELEASE_NOTES_SOURCE)
	@ echo --- RELEASE NOTES - HTML ---
	$(A2X) --format=xhtml $(A2X_HTML_OPTS) \
		--destination-dir=$(builddir) \
		$(srcdir)/release-notes.asciidoc

# create txt single page file (through HTML)
release-notes.txt: $(RELEASE_NOTES_SOURCE)
	@ echo --- RELEASE NOTES - TXT ---
	TZ=UTC $(A2X) --format=text @A2X_LYNX@ $(A2X_TEXT_OPTS) \
		--xsltproc-opts "--stringparam generate.toc \"article nop\"" \
		--destination-dir=$(builddir) \
		$(srcdir)/release-notes.asciidoc && \
		mv release-notes.text $@

news: release-notes.txt
	cp release-notes.txt ../NEWS
	-cp release-notes.txt $(top_srcdir)/NEWS

# Apparently we can only run one instance of a2x at a time.
.NOTPARALLEL: release-notes-us.pdf release-notes-a4.pdf
# create pdf file, portrait pages on US letter paper
release-notes-us.pdf: $(RELEASE_NOTES_SOURCE) custom_layer_pdf.xsl
	@ echo --- RELEASE NOTES - PDF US LETTER PAPER ---
	$(A2X) --format=pdf $(A2X_HTML_OPTS) --fop \
		--xsltproc-opts "--stringparam paper.type letter --nonet" \
		--xsl-file=$(srcdir)/custom_layer_pdf.xsl \
		--destination-dir=$(builddir) \
		$(srcdir)/release-notes.asciidoc

# create pdf file, portrait pages on A4 paper
release-notes-a4.pdf: $(RELEASE_NOTES_SOURCE) custom_layer_pdf.xsl
	@ echo --- RELEASE NOTES - PDF A4 PAPER ---
	$(A2X) --format=pdf $(A2X_HTML_OPTS) --fop \
		--xsltproc-opts "--stringparam paper.type A4 --nonet" \
		--xsl-file=$(srcdir)/custom_layer_pdf.xsl \
		--destination-dir=$(builddir) \
		$(srcdir)/release-notes.asciidoc

clean-local:
	-rm -rf $(CLEANDIRS)

EXTRA_DIST = \
	$(WSUG_DIST)		\
	$(WSDG_DIST)		\
	$(RELEASE_NOTES_SOURCE)	\
	dfilter2xml.pl		\
	make-wsluarm.pl		\
	README.txt		\
	CMakeLists.txt

MAINTAINERCLEANFILES = \
	Makefile.in
