
TESTS_ENVIRONMENT = PATH=$(VARNISH_BINDIR):$(VARNISH_SBINDIR):$$PATH

vmoddir = $(VMOD_DIR)
vmod_LTLIBRARIES = libvmod_utf8.la

libvmod_utf8_la_CFLAGS = $(VMOD_INCLUDES) -I$(top_srcdir)/utf8proc
libvmod_utf8_la_LDFLAGS = -module -export-dynamic -avoid-version -shared

libvmod_utf8_la_SOURCES = \
	vmod_utf8.c \
	$(top_srcdir)/utf8proc/utf8proc.c \
	$(top_srcdir)/utf8proc/utf8proc.h

nodist_libvmod_utf8_la_SOURCES = \
	vcc_if.c \
	vcc_if.h

dist_man_MANS = vmod_utf8.3

VTC_LOG_COMPILER = $(VARNISHTEST) -v \
	-Dvmod_topbuild=$(abs_top_builddir)
TEST_EXTENSIONS = .vtc
TESTS = @VMOD_TESTS@

$(libvmod_utf8_la_OBJECTS): vcc_if.h

vcc_if.h vmod_utf8.man.rst vmod_utf8.rst: vcc_if.c

vcc_if.c: $(VMODTOOL) $(top_srcdir)/src/vmod_utf8.vcc
	$(PYTHON) $(VMODTOOL) $(top_srcdir)/src/vmod_utf8.vcc

vmod_utf8.3: vmod_utf8.man.rst
	$(RST2MAN) $? $@

EXTRA_DIST = vmod_utf8.vcc \
	$(top_srcdir)/utf8proc/utf8proc_data.c \
	$(TESTS)

CLEANFILES = $(builddir)/vcc_if.c \
	$(builddir)/vcc_if.h \
	$(builddir)/vmod_utf8.man.rst \
	$(builddir)/vmod_utf8.rst \
	$(builddir)/$(dist_man_MANS)

if USE_LCOV
check-local:
	$(LCOV) -c -d .libs -o .libs/$(PACKAGE).info
	$(GENHTML) -o lcov .libs/$(PACKAGE).info
	$(GCOV) -o .libs/libvmod_utf8_la-vcc_if.gcno vmod_utf8.c

clean-local:
	rm -rf lcov
endif
