## Process this file with automake to produce Makefile.in

AUTOMAKE_OPTIONS = 1.6 foreign
ACLOCAL_AMFLAGS = -I m4

SUBDIRS = include src doc win32
if HAVE_EXAMPLES
SUBDIRS += examples
endif
if HAVE_TOOLS
SUBDIRS += tools
endif

EXTRA_DIST = INSTALL m4/shout.m4 m4/acx_pthread.m4 \
	m4/ogg.m4 m4/vorbis.m4 m4/xiph_compiler.m4 m4/xiph_net.m4 \
	m4/xiph_types.m4 libshout.ckport

docdir = $(datadir)/doc/$(PACKAGE)
doc_DATA = COPYING NEWS README examples/example.c examples/nonblocking.c

m4datadir = $(datadir)/aclocal
m4data_DATA = m4/shout.m4

ckportdir = $(libdir)/ckport/db
ckport_DATA = libshout.ckport

if HAVE_PKGCONFIG
  pkgconfigdir = $(libdir)/pkgconfig
  pkgconfig_DATA = shout.pc
endif

debug:
	$(MAKE) all CFLAGS="@DEBUG@"

profile:
	$(MAKE) all CFLAGS="@PROFILE@"
