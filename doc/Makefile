# Makefile for building documentation

default: html

# You can set these variables from the command line.
SRCDIR           := $(abspath $(dir $(lastword $(MAKEFILE_LIST))))
JULIAHOME        := $(abspath $(SRCDIR)/..)
include $(JULIAHOME)/Make.inc
JULIA_EXECUTABLE := $(build_bindir)/julia

.PHONY: help clean cleanall html pdf linkcheck doctest check

help:
	@echo "Please use 'make <target>' where <target> is one of"
	@echo "  html      to make standalone HTML files"
	@echo "  pdf       to make standalone PDF file"
	@echo "  linkcheck to check all external links for integrity"
	@echo "  doctest   to run all doctests embedded in the documentation"
	@echo "  check     to run linkcheck and doctests"

clean:
	-rm -rf _build/* deps/* docbuild.log

cleanall: clean

html:
	@echo "Building HTML documentation."
ifneq ($(OS),WINNT)
	$(JULIA_EXECUTABLE) make.jl -- deploy
else
# work around issue #11727, windows output redirection breaking on buildbot
	$(JULIA_EXECUTABLE) make.jl -- deploy > docbuild.log 2>&1
	@cat docbuild.log
endif
	@echo "Build finished. The HTML pages are in _build/html."

pdf:
	@echo "Building PDF documentation."
	$(JULIA_EXECUTABLE) make.jl -- pdf
	@echo "Build finished."

linkcheck:
	@echo "Checking external documentation links."
	$(JULIA_EXECUTABLE) make.jl -- linkcheck
	@echo "Checks finished."

doctest:
	@echo "Running all embedded 'doctests'."
	$(JULIA_EXECUTABLE) make.jl -- doctest
	@echo "Checks finished."

check:
	@echo "Running all embedded 'doctests' and checking external links."
	$(JULIA_EXECUTABLE) make.jl -- doctest linkcheck
	@echo "Checks finished."
