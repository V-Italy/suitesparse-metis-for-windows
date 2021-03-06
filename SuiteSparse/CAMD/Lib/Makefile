#-------------------------------------------------------------------------------
# CAMD Lib/Makefile
#-------------------------------------------------------------------------------

LIBRARY = libcamd
VERSION = 2.4.4
SO_VERSION = 2

default: library

include ../../SuiteSparse_config/SuiteSparse_config.mk

# CAMD depends on SuiteSparse_config
LDLIBS += -lsuitesparseconfig

# compile and install in SuiteSparse/lib
library:
	$(MAKE) install INSTALL=$(SUITESPARSE)

C = $(CC) $(CF) -I../Include -I../../SuiteSparse_config

#-------------------------------------------------------------------------------
# source files
#-------------------------------------------------------------------------------

CAMD = camd_aat camd_1 camd_2 camd_dump camd_postorder camd_defaults \
	camd_order camd_control camd_info camd_valid camd_preprocess

INC = ../Include/camd.h ../Include/camd_internal.h \
      ../../SuiteSparse_config/SuiteSparse_config.h

#-------------------------------------------------------------------------------
# object files for each version
#-------------------------------------------------------------------------------

CAMDI = $(addsuffix .o, $(subst camd_,camd_i_,$(CAMD)))
CAMDL = $(addsuffix .o, $(subst camd_,camd_l_,$(CAMD)))
OBJ = $(CAMDI) $(CAMDL)

#-------------------------------------------------------------------------------
# compile each int and long routine (with no real/complex version)
#-------------------------------------------------------------------------------

camd_i_%.o: ../Source/camd_%.c $(INC)
	$(C) -DDINT -c $< -o $@

camd_l_%.o: ../Source/camd_%.c $(INC)
	$(C) -DDLONG -c $< -o $@

#-------------------------------------------------------------------------------
# Create the static and shared libraries (C versions only)
#-------------------------------------------------------------------------------

$(AR_TARGET): $(OBJ)
	$(ARCHIVE) $@ $^
	- $(RANLIB) $@

#-------------------------------------------------------------------------------
# install (shared C version only)
#-------------------------------------------------------------------------------

# install CAMD
install: $(AR_TARGET) $(INSTALL_LIB)/$(SO_TARGET)

$(INSTALL_LIB)/$(SO_TARGET): $(OBJ)
	@mkdir -p $(INSTALL_LIB)
	@mkdir -p $(INSTALL_INCLUDE)
	@mkdir -p $(INSTALL_DOC)
	$(CC) $(SO_OPTS) $^ -o $@ $(LDLIBS)
	( cd $(INSTALL_LIB) ; ln -sf $(SO_TARGET) $(SO_PLAIN) )
	( cd $(INSTALL_LIB) ; ln -sf $(SO_TARGET) $(SO_MAIN) )
	$(CP) ../Include/camd.h $(INSTALL_INCLUDE)
	$(CP) ../Doc/CAMD_UserGuide.pdf $(INSTALL_DOC)
	$(CP) ../README.txt $(INSTALL_DOC)/CAMD_README.txt
	chmod 755 $(INSTALL_LIB)/$(SO_TARGET)
	chmod 644 $(INSTALL_INCLUDE)/camd.h
	chmod 644 $(INSTALL_DOC)/CAMD_UserGuide.pdf
	chmod 644 $(INSTALL_DOC)/CAMD_README.txt

# uninstall CAMD
uninstall:
	$(RM) $(INSTALL_LIB)/$(SO_TARGET)
	$(RM) $(INSTALL_LIB)/$(SO_PLAIN)
	$(RM) $(INSTALL_LIB)/$(SO_MAIN)
	$(RM) $(INSTALL_INCLUDE)/camd.h
	$(RM) $(INSTALL_DOC)/CAMD_UserGuide.pdf
	$(RM) $(INSTALL_DOC)/CAMD_README.txt

#-------------------------------------------------------------------------------
# Remove all but the files in the original distribution
#-------------------------------------------------------------------------------

clean:
	- $(RM) -r $(CLEAN)

purge: distclean

distclean: clean
	- $(RM) -r $(PURGE)

