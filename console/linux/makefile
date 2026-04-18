LBITS := $(shell getconf LONG_BIT)

VPATH = ../../src ../../include
CXX = gcc -Wall -I ../../include

OBJ = bbmain.o bbexec.o bbeval.o bbasmb.o bbdata.o bbccos.o bbccon.o

all: bbcbasic

bbmain.o: bbmain.c BBC.h
	$(CXX) -c -O2 -ffast-math -fno-finite-math-only $< -o $@

bbexec.o: bbexec.c BBC.h
	$(CXX) -c -O2 -ffast-math -fno-finite-math-only $< -o $@

bbeval.o: bbeval.c BBC.h
	$(CXX) -Wno-array-bounds -c -O2 -ffast-math -fmath-errno -fno-finite-math-only $< -o $@

bbasmb.o: bbasmb_x86_64.c BBC.h
	$(CXX) -c -Os $< -o $@

bbccos.o: bbccos.c bbccon.h
	$(CXX) -Wno-array-bounds -Wno-unused-result -c -Os $< -o $@

bbccon.o: bbccon.c bbccon.h
	$(CXX) -Wno-array-bounds -Wno-unused-result -c -Os $< -o $@

ifeq ($(LBITS),64)
bbdata.o: ../../src/bbdata_x86_64.nas
	nasm -f elf64 -s ../../src/bbdata_x86_64.nas -o bbdata.o
else
bbdata.o: ../../src/bbdata_x86_32.nas
	nasm -f elf32 -s ../../src/bbdata_x86_32.nas -o bbdata.o
endif

bbcbasic: $(OBJ)
	$(CXX) $(OBJ) -L . -L/usr/include -ldl -lm -lrt -pthread \
	-o bbcbasic -Wl,-s -Wl,-R,'$$ORIGIN'
	cp bbcbasic ../../
