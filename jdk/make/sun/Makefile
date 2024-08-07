#
# Copyright (c) 1995, 2011, Oracle and/or its affiliates. All rights reserved.
# DO NOT ALTER OR REMOVE COPYRIGHT NOTICES OR THIS FILE HEADER.
#
# This code is free software; you can redistribute it and/or modify it
# under the terms of the GNU General Public License version 2 only, as
# published by the Free Software Foundation.  Oracle designates this
# particular file as subject to the "Classpath" exception as provided
# by Oracle in the LICENSE file that accompanied this code.
#
# This code is distributed in the hope that it will be useful, but WITHOUT
# ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
# FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
# version 2 for more details (a copy is included in the LICENSE file that
# accompanied this code).
#
# You should have received a copy of the GNU General Public License version
# 2 along with this work; if not, write to the Free Software Foundation,
# Inc., 51 Franklin St, Fifth Floor, Boston, MA 02110-1301 USA.
#
# Please contact Oracle, 500 Oracle Parkway, Redwood Shores, CA 94065 USA
# or visit www.oracle.com if you need additional information or have any
# questions.
#

#
# Makefile for building all of sun tools
#

BUILDDIR = ..
PRODUCT  = sun
include $(BUILDDIR)/common/Defs.gmk

# Rhino/Mozilla java sources
ORG_EXISTS := $(call DirExists,$(CLOSED_SRC)/share/classes/sun/org,,)
ifneq ("$(ORG_EXISTS)", "")
  ORG_SUBDIR = org
endif

# Non windows subdirs
ifneq ($(PLATFORM), windows)
  ifndef OPENJDK
    ifeq ($(PLATFORM), solaris)
      ifneq ($(ARCH), amd64)
        # Solaris 11 does not have support for new compilation of DGA code.
        OS_VERSION = $(shell uname -r)
        OS_MAJOR_VERSION := $(call MajorVersion,$(OS_VERSION))
        OS_MINOR_VERSION := $(call MinorVersion,$(OS_VERSION))
        ifeq ($(shell $(EXPR) $(OS_MAJOR_VERSION) == 5), 1)
          ifeq ($(shell $(EXPR) $(OS_MINOR_VERSION) \<= 10), 1)
            DGA_SUBDIR = jdga
          endif
        endif
      endif
    endif
  endif
  HEADLESS_SUBDIR = headless
  XAWT_SUBDIR     = xawt
endif

ifeq ($(PLATFORM), macosx)
  LWAWT_PRE_SUBDIR = osxapp
  LWAWT_SUBDIR = lwawt
endif

ifndef OPENJDK
ifneq ($(PLATFORM), macosx)
  JDBC_SUBDIR = jdbc
endif
endif

ifdef OPENJDK
  RENDER_SUBDIR = pisces
else
  RENDER_SUBDIR = dcpr
endif

# nio need to be compiled before awt to have all charsets ready
SUBDIRS            = jar security javazic misc net nio text launcher

ifdef BUILD_HEADLESS_ONLY
  DISPLAY_LIBS = awt $(HEADLESS_SUBDIR)
  DISPLAY_TOOLS = 
else
  DISPLAY_LIBS = awt splashscreen $(XAWT_SUBDIR) $(HEADLESS_SUBDIR)
  DISPLAY_TOOLS = applet
endif
SUBDIRS_desktop    = audio $(RENDER_SUBDIR) image \
                     $(LWAWT_PRE_SUBDIR) $(DISPLAY_LIBS) $(DGA_SUBDIR) $(LWAWT_SUBDIR) \
                     jawt font jpeg cmm $(DISPLAY_TOOLS) beans
SUBDIRS_management = management
SUBDIRS_misc       = $(ORG_SUBDIR) rmi $(JDBC_SUBDIR) tracing
SUBDIRS_tools      = native2ascii serialver tools jconsole

ifndef OPENJDK
  SUBDIRS += usagetracker
endif

include $(BUILDDIR)/common/Subdirs.gmk

all build clean clobber::
	$(SUBDIRS-loop)
	echo Done with jdk/make/sun
