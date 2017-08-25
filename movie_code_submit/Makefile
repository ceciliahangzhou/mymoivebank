#
# 'make depend' uses makedepend to automatically generate dependencies 
#               (dependencies are added to end of Makefile)
# 'make'        build executable file 'mycc'
# 'make clean'  removes all .o and executable files
#

# define the C compiler to use
CC = cc

JSON_C_DIR=/usr/local

# define any compile-time flags
CFLAGS = -Wall -g -c `mysql_config --cflags`

# define any directories containing header files other than /usr/include
#
#INCLUDES = -I/home/newhall/include  -I../include
INCLUDES = -I$(JSON_C_DIR)/include/json-c

# define library paths in addition to /usr/lib
#   if I wanted to include libraries not in /usr/lib I'd specify
#   their path using -Lpath, something like:
#LFLAGS = -L/home/newhall/lib  -L../lib
LFLAGS = -L$(JSON_C_DIR)/lib -ljson-c

# define any libraries to link into executable:
#   if I want to link in libraries (libx.so or libx.a) I use the -llibname 
#   option, something like (this will link in libmylib.so and libm.so:
#LIBS = -lmylib -lm
SLIBS = `mysql_config --libs`

# define the clinet source files
CSRCS = $(wildcard client/*.c)

# define the server source files
SSRCS = $(wildcard server/*.c)

# define the C object files 
#
# This uses Suffix Replacement within a macro:
#   $(name:string1=string2)
#         For each word in 'name' replace 'string1' with 'string2'
# Below we are replacing the suffix .c of all words in the macro SRCS
# with the .o suffix
#
COBJS = $(CSRCS:.c=.o)
SOBJS = $(SSRCS:.c=.o)

# define the executable file 
CMAIN = movClient
SMAIN = movSer

#
# The following part of the makefile is generic; it can be used to 
# build any executable just by changing the definitions above and by
# deleting dependencies appended to the file from 'make depend'
#

.PHONY: depend clean

all:    $(CMAIN) $(SMAIN)
	@echo  Simple compiler named $(CMAIN) and $(SMAIN) has been compiled

$(CMAIN): $(COBJS) 
	$(CC) -o $(CMAIN) $(COBJS) $(LFLAGS) $(LIBS)

$(SMAIN): $(SOBJS) 
	$(CC) -o $(SMAIN) $(SOBJS) $(LFLAGS) $(LFLAGS_S) $(SLIBS)

# this is a suffix replacement rule for building .o's from .c's
# it uses automatic variables $<: the name of the prerequisite of
# the rule(a .c file) and $@: the name of the target of the rule (a .o file) 
# (see the gnu make manual section about automatic variables)
.c.o:
	$(CC) $(CFLAGS) $(INCLUDES)  $<  -o $@

clean:
	$(RM) *.o */*.o *~ $(CMAIN) $(SMAIN)

depend: $(CSRCS) $(SSRCS)
	makedepend $(INCLUDES) $^

# DO NOT DELETE THIS LINE -- make depend needs it
