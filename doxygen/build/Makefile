# NOTE: 1- A tab character must come at the beginning of every 
#	   command line to distinguish command lines from other lines
#	   in the makefile.
#	2- A line that starts at column 0 and contains a colon character(:) 
#	   is considered a rule. The name on the left side of the colon is created
#	   by the command.This is called a target. Any filenames specified after the colon
#	   are the files that the target depends on. They are called prerquisites(i.e. they are 
#	   required to exist before make decides to create the target.
#	3- The first target given in a makefile is the default target.

# Automatic variables: predefined set of variables inside make that can be used
#		       to aovid repetitive typing when writing out the command
#		       in a rule.
#	Here is a list of of the most useful ones:
#	  $@ : replaced by the target filename.
#	  $* : replaced by the target filename without the file extension.
#	  $< : replaced by the first prerequisite filename.
#	  $^ : replaced by the filenames of all the prerequisites, separated by spaces, discard duplicates.
#	  $+ : similar to $^, but includes duplicates.
#	  $? : the names of all prerequisites that are newer than the target, separated by spaces.

# Setting variables: To set a variable from the makefile, 
#                    write a line starting with the variable name
#                    followed by ‘=’, ‘:=’, or ‘::=’.
#                    Whatever follows the ‘=’, ‘:=’, or ‘::=’
#                    on the line becomes the value.
#
#    Variables defined with ‘=’ are recursively expanded variables.
#    Variables defined with ‘:=’ or ‘::=’ are simply expanded variables;
#      these definitions can contain variable references
#      which will be expanded before the definition is made.
#
# If you’d like a variable to be set to a value only if it’s not already set,
# then you can use the shorthand operator ‘?=’.


################################################################
# Compiler specifications
# VPATH : Is the list of directories to be searched for missing
#	  prerequisites. (Generally used for source files).
#	  The best approach is to seperate the list by spaces.
###############################################################
VPATH = ../src ../include

# Which compilers should be used
CXX = g++
FXX = gfortran

####################################################################
# What flag should be passed to the compiler
# CPPFLAGS : What flag should be passed to the C++ pre-processor
# FORFLAGS : What flag should be passed to the FORTRAN pre-processor
# LDFLAGS :  what libraries should be linked with, for example -lm 
#            links with the math library
####################################################################
CPPFLAGS = -O2 -g
FORFLAGS = -O2 -g  #flags for fortran
LDFLAGS = 

################################################################
# INCLUDE  : directory containing header files
# FOR_SRCS : is a list of all FORTRAN source code files that
#            must be linked to create the executable. 
# CPP_SRCS : is a list of all C++ source code files that must
#	     be linked to create the executable.
# SRCS_DIR : is where the source code files can be found
# All_SRCS : is a list of source code files both Fortran and C++
################################################################

INCLUDE := ../include
FOR_SRCS = # Fortran Source Files
CPP_SRCS = main.cpp SampleClass.cpp  # C++ Source Files
ALL_SRCS = $(CPP_SRCS) $(FOR_SRCS)

####################################################################
# All sources files have associated object files.
# CPP_OBJ : Associated object files for c++ source codes.
# FOR_OBJ : Associated object files for Fortran source codes.
#
# ==================================================================
# These lines set the OBJS to have the same values as the SRCS files
# but with all the .cpp's and .F95's changed to .o's.
#
# This uses Suffix Replacement within a macro:
#     $(name:string1=string2)
#   For each word in 'name' replace 'string1' with 'string2'.
#   Below we are replacing the suffix .cpp of all words in the macro
#   CPP_SRCS with the .o suffix.
####################################################################
CPP_OBJ = $(CPP_SRCS:.cpp=.o)
FOR_OBJ = $(FOR_SRCS:.F95=.o)
ALL_OBJ = $(CPP_OBJ) $(FOR_OBJ)

##############################################################
# EXEC : Name of the executable file
#      - all is the default rule since it is first in the file
##############################################################
EXEC = Sample
all: $(EXEC)  

############################################################
# Exec depends on the object files
# it is made automatically using the LDFLAGS variables
# $@ : the file name of the target
# $^ : The names of all the dependencies sperated by spaces,
#      but with duplicate names removed.
############################################################
$(EXEC): $(ALL_OBJ)
	$(CXX) $(CPPFLAGS) $(ALL_OBJ) -o $@

################################################################################
# The .o files are made automatically using the CXXFLAGS and FORFLAGS variables.
# $< : The name of the first dependency, any dependency that is more recent than
#      its corresponding target. It will recompile any F95 or cpp file if it is
#      newer than the corresponding .o file.
################################################################################
%.o : %.cpp
	$(CXX) -I$(INCLUDE) -c $(CPPFLAGS) $<

%.o : %.cxx
	$(CXX) -I$(INCLUDE) -c $(CPPFLAGS) $<

%.o : %.F95
	$(FXX) -I$(INCLUDE) -c $(FORFLAGS) $<

%.o : %.f95
	$(FXX) -I$(INCLUDE) -c $(FORFLAGS) $<

#######################################################################
# A target that does not represent a file is called a PHONY target.
# PHONY Targets are always out of date and its command will always run.
#######################################################################
.PHONY: clean

############
# Clean up #
############

# Maintanace Targets
# ----------------------------------------------
clean:
	@echo "*** Clean : removing object files    "
	@( $(RM) *.o *.mod *~)
	@( $(RM) $(EXEC))
