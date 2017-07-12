AUTOMAKE_OPTIONS = check-news
ACLOCAL_AMFLAGS = -I local_m4

MODEL_DIR := mni-models

# mess with the data install directory
pkgdatadir = $(prefix)/share/$(MODEL_DIR)

MODEL_BASE := average305_t1_tal_lin
MODELS := $(MODEL_BASE)

# datestamp
DATE_STAMP := `date +%D-%T`

# package data
noinst_DATA := \
	$(MODELS:=.mnc.gz) \
	$(MODEL_BASE)_mask.mnc.gz \
	$(MODEL_BASE)_headmask.mnc.gz

# installed data data
pkgdata_DATA := \
	$(MODELS:=.mnc) \
	$(MODELS:=_mask.mnc) \
	$(MODELS:=_headmask.mnc)

EXTRA_DIST = $(noinst_DATA) $(m4_files) \
	epm-header.in

m4_files = local_m4/ac_prog_gzip.m4

# be tidy
CLEANFILES = $(pkgdata_DATA)

# gunzip rule
%: %.gz
	$(gzip) -d -c $< > $@

all: $(noinst_DATA:.gz=) $(DATE)
	for i in $(pkgdata_DATA); \
   do \
      echo "Adding mni-model header to $$i"; \
      minc_modify_header \
			-sinsert mni-model:package=$(PACKAGE) \
			-sinsert mni-model:version=$(VERSION) \
			-sinsert mni-model:compile-date=$(DATE_STAMP) \
			-sinsert mni-model:compile-user=$$USER \
			$$i; \
   done
