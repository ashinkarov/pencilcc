# @configure_input@

ACLOCAL_AMFLAGS = -I m4

# Require clang to be installed on the system
#AM_DISTCHECK_CONFIGURE_FLAGS = --with-clang=system


SUBDIRS =
if BUNDLED_PPCG
SUBDIRS += ppcg
endif
if BUNDLED_PENCIL_UTIL
SUBDIRS += pencil-headers
endif
if BUNDLED_PRL
SUBDIRS += prl
endif

pkgdata_SCRIPTS = pencilcc.py penciltool.py

CLEANFILES = pencilcc.py pencilcc penciltool.py penciltool

TESTS = \
	tests/polybench_test.sh \
	tests/check-simple.sh \
	tests/check-simple-keep.sh \
	tests/check-prl_alloc.sh \
	tests/check-prl_manage.sh \
	tests/check-prl_manage_host.sh

EXTRA_DIST = \
	autogen.sh \
	README \
	examples \
	get-polybench.sh \
	polybench \
	tests/simple.c \
	tests/check-simple.sh \
	tests/check-simple-keep.sh \
	tests/prl_alloc.c \
	tests/check-prl_alloc.sh \
	tests/prl_manage.c \
	tests/check-prl_manage.sh \
	tests/prl_manage_host.c \
	tests/check-prl_manage_host.sh

AM_TESTS_ENVIRONMENT = \
	export PENCILCC="$(abs_builddir)/pencilcc"; \
	export TESTSRCDIR="`dirname "$(abs_srcdir)/$${b}"`"; \
	export TESTBUILDDIR="`dirname "$(abs_builddir)/$${b}"`/check/`basename $${b}`"; \
	$(MKDIR_P) "$${TESTBUILDDIR}";

pencilcc: pencilcc.py
	ln -sf $< $@

penciltool: penciltool.py
	ln -sf $< $@

all-local: pencilcc penciltool

install-exec-local:
	$(LN_S) -f $(DESTDIR)$(pkgdatadir)/pencilcc.py $(DESTDIR)$(bindir)/pencilcc
	$(LN_S) -f $(DESTDIR)$(pkgdatadir)/penciltool.py $(DESTDIR)$(bindir)/penciltool

uninstall-local:
	rm -f $(DESTDIR)$(bindir)/pencilcc
	rm -f $(DESTDIR)$(bindir)/penciltool

# Dowload from the internet
polybench: get-polybench.sh
	${srcdir}/get-polybench.sh

# Get the packages so they can be used by "make check"
dist-local: polybench

installcheck-local:
	@for test in ${TESTS}; do \
		export PENCILCC="${bindir}/pencilcc"; \
		export TESTSRCDIR="`dirname "$(abs_srcdir)/$${test}"`"; \
		export TESTBUILDDIR="`dirname "$(abs_builddir)/$${test}"`/installcheck/`basename $${test}`"; \
		$(MKDIR_P) "$${TESTBUILDDIR}"; \
		echo "Checking `basename $${test}` ..."; \
		( cd "$${TESTBUILDDIR}" && if test -f "$(abs_builddir)/$${test}"; then "$(abs_builddir)/$${test}"; else "$(abs_srcdir)/$${test}"; fi) || exit 1; \
	done

clean-local:
	@for test in ${TESTS}; do \
		TESTBUILDDIR="`dirname "$(abs_builddir)/$${test}"`"; \
		rm -rf "$${TESTBUILDDIR}/check"; \
		rm -rf "$${TESTBUILDDIR}/installcheck"; \
	done
