SPHINXBASE=sphinxbase-0.7
SPHINXBASE_DIR=$(switch_srcdir)/libs/$(SPHINXBASE)
SPHINXBASE_BUILDDIR=$(switch_builddir)/libs/$(SPHINXBASE)
POCKETSPHINX=pocketsphinx-0.7
POCKETSPHINX_DIR=$(switch_srcdir)/libs/$(POCKETSPHINX)
POCKETSPHINX_BUILDDIR=$(switch_builddir)/libs/$(POCKETSPHINX)
SPHINXBASE_LA=$(SPHINXBASE_BUILDDIR)/src/libsphinxbase/libsphinxbase.la
SPHINXBASE_LA2=$(SPHINXBASE_BUILDDIR)/src/libsphinxad/libsphinxad.la

POCKETSPHINX_LA=$(POCKETSPHINX_BUILDDIR)/src/libpocketsphinx/libpocketsphinx.la

SPHINXMODEL=communicator_semi_6000_20080321

LOCAL_CFLAGS=-I$(SPHINXBASE_DIR)/include -I$(POCKETSPHINX_DIR)/include
LOCAL_LIBADD=$(POCKETSPHINX_LA) $(SPHINXBASE_LA) $(SPHINXBASE_LA2) 
BASE=../../../..
include $(BASE)/build/modmake.rules

$(SPHINXBASE_DIR):
	$(GETLIB) $(SPHINXBASE).tar.gz

$(SPHINXBASE_BUILDDIR)/buildstamp: $(SPHINXBASE_DIR)
	mkdir -p $(SPHINXBASE_BUILDDIR)
	test -f $(SPHINXBASE_BUILDDIR)/Makefile || (cd $(SPHINXBASE_BUILDDIR) && $(DEFAULT_VARS) $(SPHINXBASE_DIR)/configure $(DEFAULT_ARGS) --srcdir=$(SPHINXBASE_DIR) --without-python CFLAGS=)
	test -f $(SPHINXBASE_BUILDDIR)/buildstamp || (touch $(SPHINXBASE_BUILDDIR)/buildstamp)

$(SPHINXBASE_LA): $(SPHINXBASE_BUILDDIR)/buildstamp
	cd $(SPHINXBASE_BUILDDIR) && $(MAKE)
	$(TOUCH_TARGET)

$(POCKETSPHINX_DIR):
	$(GETLIB) $(POCKETSPHINX).tar.gz

$(POCKETSPHINX_BUILDDIR)/Makefile: $(POCKETSPHINX_DIR) $(SPHINXBASE_LA)
	mkdir -p $(POCKETSPHINX_BUILDDIR)
	cd $(POCKETSPHINX_BUILDDIR) && $(DEFAULT_VARS) $(POCKETSPHINX_DIR)/configure $(DEFAULT_ARGS) --srcdir=$(POCKETSPHINX_DIR) --without-python --with-sphinxbase=$(SPHINXBASE_DIR) --with-sphinxbase-build=$(SPHINXBASE_BUILDDIR) CFLAGS=
	$(TOUCH_TARGET)

$(POCKETSPHINX_BUILDDIR)/buildstamp: $(POCKETSPHINX_BUILDDIR)/Makefile
	test -f $(POCKETSPHINX_BUILDDIR)/buildstamp || touch $(POCKETSPHINX_BUILDDIR)/buildstamp

$(POCKETSPHINX_LA): $(POCKETSPHINX_BUILDDIR)/Makefile 
	cd $(POCKETSPHINX_BUILDDIR) && $(MAKE)
	$(TOUCH_TARGET)

$(DESTDIR)$(grammardir)/model:
	mkdir -p $(DESTDIR)$(grammardir)/model

$(DESTDIR)$(grammardir)/model/communicator:
	$(GETLIB) $(SPHINXMODEL).tar.gz
	mkdir -p $(DESTDIR)$(grammardir)/model/communicator
	cp -rp $(switch_srcdir)/libs/Communicator_semi_40.cd_semi_6000/* $(DESTDIR)$(grammardir)/model/communicator

dictionary:
	@cp -f $(POCKETSPHINX_DIR)/model/lm/en_US/cmu07a.dic $(DESTDIR)$(grammardir)/default.dic

local_install: $(DESTDIR)$(grammardir)/model $(DESTDIR)$(grammardir)/model/communicator dictionary
