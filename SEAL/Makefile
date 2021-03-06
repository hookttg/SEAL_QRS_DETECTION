# Makefile for the SEAL library
UTIL_DIR=util
BIN_DIR=../bin
OBJ_DIR=obj
SRCS=$(wildcard *.cpp) $(wildcard $(UTIL_DIR)/*.cpp)
OBJS=$(SRCS:%.cpp=$(OBJ_DIR)/%.o)
DEPS=$(OBJS:.o=.d)
INSTALL_DIR=SEAL
SEALLIB=libseal.a
CXX=g++
CXXFLAGS=-g -O2 -mbmi2 -DHAVE_CONFIG_H -march=native -std=c++11
PREFIX=/usr/local

.PHONY : all clean install uninstall

all : $(BIN_DIR)/$(SEALLIB)

$(OBJ_DIR)/%.d : %.cpp
	@-mkdir -p $(dir $@)
	@set -e; \
	rm -f $@; \
	$(CXX) -MM $(CXXFLAGS) -I. -I$(UTIL_DIR) $< > $@.temp; \
	SUBDIR_NAME=`echo $@|sed -e 's,\.,,' -e 's,$(UTIL_DIR),$(UTIL_DIR)/,'`; \
	sed 's,\($(*F)\)\.o[ :]*,$(OBJ_DIR)/$(SUBDIR_NAME)\1.o $@ : ,g' < $@.temp > $@; \
	rm -f $@.temp

$(OBJ_DIR)/%.o : %.cpp
	@-mkdir -p $(dir $@)
	$(CXX) -c $(CXXFLAGS) -I. -I$(UTIL_DIR) -o $@ $<

$(BIN_DIR)/$(SEALLIB) : $(OBJS)
	@-mkdir -p $(dir $@)
	ar rcs $(BIN_DIR)/$(SEALLIB) $(OBJS)

clean :
	@-rm -f $(BIN_DIR)/$(SEALLIB)
	@-rm -rf $(OBJ_DIR)

install : $(BIN_DIR)/$(SEALLIB)
	@-mkdir -p $(PREFIX)/lib/$(INSTALL_DIR)
	@-mkdir -p $(PREFIX)/include/$(INSTALL_DIR)/util
	cp -f $(BIN_DIR)/$(SEALLIB) $(PREFIX)/lib/$(INSTALL_DIR)/$(SEALLIB)
	cp -f *.h $(PREFIX)/include/$(INSTALL_DIR)
	cp -f $(UTIL_DIR)/*.h $(PREFIX)/include/$(INSTALL_DIR)/$(UTIL_DIR)

uninstall :
	rm -rf $(PREFIX)/lib/$(INSTALL_DIR)
	rm -rf $(PREFIX)/include/$(INSTALL_DIR)

-include $(DEPS)
