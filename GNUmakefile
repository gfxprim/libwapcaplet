# Self-contained GNU makefile for libwapcaplet.
#
# GNU make prefers GNUmakefile over the upstream netsurf-buildsystem Makefile,
# so plain `make` here builds libwapcaplet without that buildsystem.  This file
# is deliberately standalone — it touches nothing else in the tree and writes
# all output under build-gnu/ — so the commit adding it can be rebased cleanly.
# `make -f Makefile` still drives the original buildsystem.
#
# It builds a static archive: build-gnu/libwapcaplet.a

CC       ?= cc
AR       ?= ar
BUILDDIR := build-gnu
LIB      := $(BUILDDIR)/libwapcaplet.a

all: $(LIB)

CFLAGS := -std=c99 -O2 -g -D_BSD_SOURCE -D_DEFAULT_SOURCE -Iinclude -Isrc

SRCS := $(shell find src -name '*.c')
OBJS := $(patsubst src/%.c,$(BUILDDIR)/%.o,$(SRCS))

$(LIB): $(OBJS)
	@mkdir -p $(dir $@)
	$(AR) cr $@ $^

$(BUILDDIR)/%.o: src/%.c
	@mkdir -p $(dir $@)
	$(CC) $(CFLAGS) -MMD -MP -MF $(@:.o=.d) -c -o $@ $<

clean:
	rm -rf $(BUILDDIR)

.PHONY: all clean

-include $(shell find $(BUILDDIR) -name '*.d' 2>/dev/null)
