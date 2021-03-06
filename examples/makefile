########################################################################
# Compiler and external dependences
########################################################################
CC        = mpicc
F77       = mpif77
CXX       = mpicxx
F90       = mpif90
HYPRE_DIR = /Users/calhoun/software/hypre/src/hypre

########################################################################
# Compiling and linking options
########################################################################
COPTS     = -g -Wall
CINCLUDES = -I$(HYPRE_DIR)/include
CDEFS     = -DHAVE_CONFIG_H -DHYPRE_TIMING
CFLAGS    = $(COPTS) $(CINCLUDES) $(CDEFS)
FOPTS     = -g
FINCLUDES = $(CINCLUDES)
FFLAGS    = $(FOPTS) $(FINCLUDES)
CXXOPTS   = $(COPTS) -Wno-deprecated
CXXINCLUDES = $(CINCLUDES) -I..
CXXDEFS   = $(CDEFS)
IFLAGS_BXX = -I../babel-runtime/sidl
CXXFLAGS  = $(CXXOPTS) $(CXXINCLUDES) $(CXXDEFS) $(IFLAGS_BXX)
IF90FLAGS = -I../babel/bHYPREClient-F90
F90FLAGS = $(FFLAGS) $(IF90FLAGS)


LINKOPTS  = $(COPTS)
LIBS      = -L$(HYPRE_DIR)/lib -lHYPRE -lm
LFLAGS    = $(LINKOPTS) $(LIBS) -lstdc++
LFLAGS_B =\
 -L${HYPRE_DIR}/lib\
 -lbHYPREClient-C\
 -lbHYPREClient-CX\
 -lbHYPREClient-F\
 -lbHYPRE\
 -lsidl -ldl -lxml2
LFLAGS77 = $(LFLAGS)
LFLAGS90 =

########################################################################
# Rules for compiling the source files
########################################################################
.SUFFIXES: .c .f .cxx .f90

.c.o:
	$(CC) $(CFLAGS) -c $<
.f.o:
	$(F77) $(FFLAGS) -c $<
.cxx.o:
	$(CXX) $(CXXFLAGS) -c $<

# This Fortran 90 build code is adapted from the sample in the Babel Users' Guide,
# which explains the separate preprocessing steps as a workaround to the 31-
# character name limit in Fortran 90.
.f90.o:
	$(CC) -E -traditional -P -o $*.tmp -x c $(IF90FLAGS) $<
	sed -e 's/^#pragma.*$$//' < $*.tmp > $*-pp.f90
	$(F90) -c -o $@ $(F90FLAGS) $*-pp.f90
	rm -f $*.tmp
# ... we can't delete the $*-pp.f90 files because they are the source code which
# debuggers need to look at

########################################################################
# List of all programs to be compiled
########################################################################
ALLPROGS = ex1

all: $(ALLPROGS)

default: all

########################################################################
# Example 1
########################################################################
ex1: ex1.o
	$(CC) -o $@ $^ $(LFLAGS)

########################################################################
# Example 2
########################################################################
ex2: ex2.o
	$(CC) -o $@ $^ $(LFLAGS)

##################a######################################################
# Example 2a
########################################################################
ex2a: ex2a.o
	$(CC) -o $@ $^ $(LFLAGS)


##################a######################################################
# Example 2b
########################################################################
ex2b: ex2b.o
	$(CC) -o $@ $^ $(LFLAGS)




########################################################################
# Example 3
########################################################################
ex3: ex3.o
	$(CC) -o $@ $^ $(LFLAGS)


########################################################################
# Example 4
########################################################################
ex4: ex4.o
	$(CC) -o $@ $^ $(LFLAGS)


########################################################################
# Clean up
########################################################################
clean:
	rm -f $(ALLPROGS:=.o)
	cd vis; make clean

distclean: clean
	rm -f $(ALLPROGS) $(ALLPROGS:=*~)
