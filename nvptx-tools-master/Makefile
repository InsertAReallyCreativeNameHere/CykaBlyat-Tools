CXX = g++
CPPFLAGS = 
CPPFLAGS_EXTRA = -DPKGVERSION="\"(nvptx-tools) \"" \
		 -DREPORT_BUGS_TO="\"<https://github.com/MentorEmbedded/nvptx-tools/issues>\""
DEFS = -DPACKAGE_NAME=\"\" -DPACKAGE_TARNAME=\"\" -DPACKAGE_VERSION=\"\" -DPACKAGE_STRING=\"\" -DPACKAGE_BUGREPORT=\"\" -DPACKAGE_URL=\"\" -DHAVE_DECL_CUGETERRORNAME=0 -DHAVE_DECL_CUGETERRORSTRING=0 -DHAVE_SYS_TYPES_H=1 -DHAVE_SYS_STAT_H=1 -DHAVE_STDLIB_H=1 -DHAVE_STRING_H=1 -DHAVE_MEMORY_H=1 -DHAVE_STRINGS_H=1 -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DHAVE_UNISTD_H=1 -DHAVE_UNISTD_H=1 -DHAVE_SYS_STAT_H=1 -DHAVE_DECL_GETOPT=1
CPPFLAGS_ALL = $(DEFS) $(CPPFLAGS) $(CPPFLAGS_EXTRA)
CXXFLAGS = -g -O2
LDFLAGS = 
LIBS = 

CUDA_DRIVER_CPPFLAGS = 
CUDA_DRIVER_LDFLAGS = 
CUDA_DRIVER_LDFLAGS_ALLOW_SHLIB_UNDEFINED = -Wl,--allow-shlib-undefined

AR = /c/Users/Administrator/Documents/GitHub/CykaBlyat-Tools/mingw32/bin/ar
RANLIB = /c/Users/Administrator/Documents/GitHub/CykaBlyat-Tools/mingw32/bin/ranlib

LN_S = ln -s
INSTALL = /usr/bin/install -c
INSTALL_DATA = ${INSTALL} -m 644
INSTALL_PROGRAM = ${INSTALL}

prefix = /usr/local
exec_prefix = ${prefix}
bindir = ${exec_prefix}/bin
libdir = ${exec_prefix}/lib
srcdir = .

mkinstalldirs = $(SHELL) $(srcdir)/mkinstalldirs

PROGRAMS = nvptx-none-as nvptx-none-ld 

all: $(PROGRAMS)

libiberty.stmp:
	cd libiberty/ && $(MAKE)
	: > $@

nvptx-none-as: $(srcdir)/nvptx-as.c libiberty.stmp $(srcdir)/version.h
	$(CXX) $(CPPFLAGS_ALL) $(CXXFLAGS) -I$(srcdir)/include \
		$< -o $@ \
		-Llibiberty -liberty $(LDFLAGS) $(LIBS)

nvptx-none-ld: $(srcdir)/nvptx-ld.c libiberty.stmp $(srcdir)/version.h
	$(CXX) $(CPPFLAGS_ALL) $(CXXFLAGS) -I$(srcdir)/include \
		$< -o $@ \
		-Llibiberty -liberty $(LDFLAGS) $(LIBS)

nvptx-none-run: $(srcdir)/nvptx-run.c libiberty.stmp $(srcdir)/version.h
	$(CXX) $(CUDA_DRIVER_CPPFLAGS) \
		$(CPPFLAGS_ALL) $(CXXFLAGS) -I$(srcdir)/include \
		$< -o $@ \
		$(CUDA_DRIVER_LDFLAGS) \
		-Llibiberty -liberty $(LDFLAGS) $(LIBS) \
		-lcuda $(CUDA_DRIVER_LDFLAGS_ALLOW_SHLIB_UNDEFINED)

.PHONY: install
install: all
	$(mkinstalldirs) $(DESTDIR)$(bindir)
	for x in $(PROGRAMS); do \
		$(INSTALL_PROGRAM) -m 755 $$x $(DESTDIR)$(bindir)/$$x; \
	done
	[ -z "" ] || $(INSTALL_PROGRAM) -m 755 $(srcdir)/nvptx-run-single $(DESTDIR)$(bindir)/nvptx-none-run-single
	rm -f $(DESTDIR)$(bindir)/nvptx-none-ar
	rm -f $(DESTDIR)$(bindir)/nvptx-none-ranlib
	$(LN_S) $(AR) $(DESTDIR)$(bindir)/nvptx-none-ar
	$(LN_S) $(RANLIB) $(DESTDIR)$(bindir)/nvptx-none-ranlib
	$(mkinstalldirs) $(DESTDIR)$(prefix)/nvptx-none
	$(mkinstalldirs) $(DESTDIR)$(prefix)/nvptx-none/bin
	rel=$$(perl -MFile::Spec -e 'print File::Spec->abs2rel(@ARGV)' \
	  $(DESTDIR)$(bindir) $(DESTDIR)$(prefix)/nvptx-none/bin) && \
	for p in ar as ld ranlib; do \
	  rm -f $(DESTDIR)$(prefix)/nvptx-none/bin/"$$p" && \
	  $(LN_S) "$$rel"/nvptx-none-"$$p" $(DESTDIR)$(prefix)/nvptx-none/bin/"$$p"; \
	done
