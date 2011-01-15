Q = @

QEMUDIR = ..
INSTDIR = /usr/local/bin

CXX  = g++
CC   = gcc

CWARNS	= -Wformat=2 -Wextra -Wmissing-noreturn -Wwrite-strings -Wshadow \
	   -Wno-unused-parameter -Wmissing-format-attribute -fno-builtin \
	   -Wswitch-default -Wmissing-declarations -Wno-format-nonliteral \
	   -Wno-deprecated -Wno-format-security

CFLAGS  = -Wall -Werror $(CWARNS) -I$(QEMUDIR) -g #-O2
LDFLAGS = -lsqlite3 -lz

HDRS	= $(QEMUDIR)/mtrace-magic.h syms.hh calltrace.hh util.h objinfo.h

all: mt2db mt2text

mt2db: mt2db.cc $(HDRS)
	@echo "CXX	$@"
	$(Q)$(CXX) -o $@ $< $(CFLAGS) $(LDFLAGS)

mt2text: mt2text.c $(HDRS)
	@echo "CC	$@"
	$(Q)$(CC) -o $@ $< $(CFLAGS) $(LDFLAGS)

objinfo: objinfo.cc $(HDRS)
	@echo "CXX	$@"
	$(Q)$(CXX) -DTEST -o $@ $< $(CFLAGS) $(LDFLAGS) -ldwarf -lelf

lockinfer: lockinfer.cc objinfo.cc $(HDRS)
	@echo "CXX	$@"
	$(Q)$(CXX) -o $@ lockinfer.cc objinfo.cc $(CFLAGS) $(LDFLAGS) -ldwarf -lelf

install: mt2db mt2text
	@echo "INSTALL  $(INSTDIR)"
	$(Q)cp mt2db $(INSTDIR)/mt2db
	$(Q)cp mt2text $(INSTDIR)/mt2text

clean:
	@echo "CLEAN"
	$(Q)rm -f mt2db mt2text