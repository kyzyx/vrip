#  
#  Brian Curless
#  
#  Computer Graphics Laboratory
#  Stanford University
#  
#  ---------------------------------------------------------------------
#  
#  Copyright (1997-2001) The Board of Trustees of the Leland Stanford Junior
#  University. Except for commercial resale, lease, license or other
#  commercial transactions, permission is hereby given to use, copy,
#  modify this software for academic purposes only.  No part of this
#  software or any derivatives thereof may be used in the production of
#  computer models for resale or for use in a commercial
#  product. STANFORD MAKES NO REPRESENTATIONS OR WARRANTIES OF ANY KIND
#  CONCERNING THIS SOFTWARE.  No support is implied or provided.
#  

ifndef OPTIMIZER
	OPTIMIZER = -O
endif

CXXREGFILES = verify.cc

CXXLIBFILES = Vec3f.cc Matrix2f.cc Matrix3f.cc Matrix4f.cc \
		Quaternion.cc

CXXFILES = $(CXXREGFILES) $(CXXLIBFILES)

CFILES = 

TARGETS = liblinear.a verify

ROOT = ../..

# Get OS uname
UNAME := $(shell uname)

ifeq ($(UNAME), IRIX)

        # Set optimizer flags to allow it to run on Indys...
        ifeq ($(OPTIMIZER), -O) 
	     OPTIMIZER = -O -mips3
        endif  #OPTIMIZER

	CC = cc
	CXX = CC

	CFLAGS = -D_BOOL -woff3262 -woff1174
	CXXFLAGS = -D_BOOL -woff3262 -woff1174

	INCLS = -I. -I$(ROOT)/include

	LIBPATHS = -L. -L$(ROOT)/lib

else

	# Set special optimizer flags...
	ifeq ($(OPTIMIZER),-O) 
		OPTIMIZER = -O6 -march=i686 -malign-double -fno-for-scope \
			    -funroll-loops -fomit-frame-pointer -ffast-math
	endif

	CC = gcc
	CXX = g++

	CFLAGS = -DLINUX -Wall -D_BOOL -Wno-unused \
		-Wmissing-declarations
	CXXFLAGS = -DLINUX -Wall -D_BOOL -Wno-unused

	INCLS = -I. -I$(ROOT)/include

	LIBPATHS = -L. -L$(ROOT)/lib

endif

SRCS = $(CXXREGFILES) $(CXXLIBFILES) $(CXXFILES) $(CFILES)

# C++ object files
CXXOBJS = $(CXXFILES:.cc=.o)
CXXLIBOBJS = $(CXXLIBFILES:.cc=.o)

COBJS = $(CFILES:.c=.o)

CFLAGS := $(INCLS) -DFUNCPROTO -DHAVE_UNISTD_H $(CFLAGS)
CXXFLAGS := $(INCLS) -DFUNCPROTO -DHAVE_UNISTD_H $(CXXFLAGS)
CC := $(CC) $(OPTIMIZER)
CXX := $(CXX) $(OPTIMIZER)

CXXLLDLIBS =  -L. -L$(ROOT)/lib -llinear -lc -lm

default: all

all: $(TARGETS)

install: clobber depend all

liblinear.a: $(CXXLIBOBJS)
	 ar -rc liblinear.a $(CXXLIBOBJS)

verify: verify.o liblinear.a
	$(CXX) -o $@ verify.o $(CXXLLDLIBS)

clean:
	-/bin/rm -f *.o *~ core

clobber:
	-/bin/rm -f *.o *~ core makedep $(TARGETS)

depend:
	echo > makedep
	gccmakedep -fmakedep -- $(CFLAGS) -- $(SRCS)
	-rm makedep.bak

-include makedep

