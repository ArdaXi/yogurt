# yogurt: pacman frontend

VERSION       = $(shell git describe --abbrev=4)
DESTDIR       =
PREFIX        = /usr/local
SHELL         = /bin/bash
INSTALL       = /usr/bin/env install 
MSGFMT        = /usr/bin/msgfmt
SED           = /bin/sed
bindir        = $(PREFIX)/bin
libdir        = $(PREFIX)/lib
sysconfdir    = $(PREFIX)/etc
completiondir = ${PREFIX}/share/bash-completion/completions
localedir     = $(PREFIX)/share/locale
localstatedir = $(PREFIX)/var
mandir        = $(PREFIX)/share/man
dbpath        = ${localstatedir}/lib/pacman/
cachedir      = ${localstatedir}/cache/pacman/pkg/
aururl        = https://aur.archlinux.org

SCRIPTS = yogurt.sh pacdiffviewer.sh \
		  yogurtrc \
		  lib/util.sh lib/pkgbuild.sh lib/pacman.sh lib/abs.sh

all: $(SCRIPTS)

$(SCRIPTS): ${SCRIPTS:=.in}
	@echo '	GEN' $@
	@$(SED) -e 's|@VERSION@|$(VERSION)|g' \
	        -e 's|@localedir@|$(localedir)|g' \
	        -e 's|@sysconfdir@|$(sysconfdir)|g' \
	        -e 's|@libdir@|$(libdir)|g' \
	        -e 's|@localstatedir@|$(localstatedir)|g' \
	        -e 's|@dbpath@|$(dbpath)|g' \
	        -e 's|@cachedir@|$(cachedir)|g' \
	        -e 's|@aururl@|$(aururl)|g' \
	        $@.in > $@

install: $(SCRIPTS)
	$(INSTALL) -d $(DESTDIR)$(bindir)
	$(INSTALL) -d $(DESTDIR)$(libdir)/yogurt
	$(INSTALL) -d $(DESTDIR)$(sysconfdir)
	$(INSTALL) -d $(DESTDIR)$(completiondir)
	$(INSTALL) -d $(DESTDIR)$(mandir)/man{5,8}
	# Scripts
	$(INSTALL) -m755 yogurt.sh $(DESTDIR)$(bindir)/yogurt
	$(INSTALL) -m755 pacdiffviewer.sh $(DESTDIR)$(bindir)/pacdiffviewer
	# Configuration
	$(INSTALL) -m644 yogurtrc $(DESTDIR)$(sysconfdir)/yogurtrc
	$(INSTALL) -m644 bashcompletion $(DESTDIR)$(completiondir)/yogurt
	# Libs
	$(INSTALL) -m644 lib/alpm_backup.sh $(DESTDIR)$(libdir)/yogurt
	$(INSTALL) -m644 lib/alpm_query.sh $(DESTDIR)$(libdir)/yogurt
	$(INSTALL) -m644 lib/alpm_stats.sh $(DESTDIR)$(libdir)/yogurt
	$(INSTALL) -m644 lib/abs.sh $(DESTDIR)$(libdir)/yogurt
	$(INSTALL) -m644 lib/aur.sh $(DESTDIR)$(libdir)/yogurt
	$(INSTALL) -m644 lib/util.sh $(DESTDIR)$(libdir)/yogurt
	$(INSTALL) -m644 lib/io.sh $(DESTDIR)$(libdir)/yogurt
	$(INSTALL) -m644 lib/pacman.sh $(DESTDIR)$(libdir)/yogurt
	$(INSTALL) -m644 lib/pkgbuild.sh $(DESTDIR)$(libdir)/yogurt
	$(INSTALL) -m644 lib/misc.sh $(DESTDIR)$(libdir)/yogurt
	# Man
	$(INSTALL) -m644 man/*.5 $(DESTDIR)$(mandir)/man5
	$(INSTALL) -m644 man/*.8 $(DESTDIR)$(mandir)/man8
	# Locales
	test -x $(MSGFMT) && for file in po/*/*.po; \
	do \
	  package=$$(echo $$file | $(SED) -e 's#po/\([^/]\+\).*#\1#'); \
	  lang=$$(echo $$file | $(SED) -e 's#.*/\([^/]\+\).po#\1#'); \
	  $(INSTALL) -d $(DESTDIR)$(localedir)/$$lang/LC_MESSAGES; \
	  $(MSGFMT) -o $(DESTDIR)$(localedir)/$$lang/LC_MESSAGES/$$package.mo $$file; \
	done

dist:
	mkdir yogurt-$(VERSION) 
	cp -a AUTHORS INSTALL COPYING Makefile pacdiffviewer.sh.in yogurt.sh.in yogurtrc.in \
	  bashcompletion lib man po yogurt-$(VERSION)
	$(SED) -i -e 's|^VERSION.*|VERSION=$(VERSION)|g' yogurt-$(VERSION)/Makefile
	tar zcvf yogurt-$(VERSION).tar.gz yogurt-$(VERSION)
	rm -rf yogurt-$(VERSION)

clean:
	rm -f $(SCRIPTS)
	rm -f yogurt-$(VERSION).tar.gz
	rm -rf yogurt-$(VERSION)


.PHONY: all install clean dist
