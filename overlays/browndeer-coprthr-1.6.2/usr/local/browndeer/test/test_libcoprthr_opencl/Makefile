
prefix=/usr/local/browndeer
exec_prefix=${prefix}

#OPENCL_INC_DIR = /home/parallella/coprthr/include
#OPENCL_LIB_DIR = /home/parallella/coprthr/src/libocl
#OPENCL_LIBNAME = ocl
OPENCL_INC_DIR = $(prefix)/include
OPENCL_LIB_DIR = $(prefix)/lib
OPENCL_LIBNAME = coprthr_opencl

USE_OPENCL_INCS=-I$(OPENCL_INC_DIR)
USE_OPENCL_LIBS=-L$(OPENCL_LIB_DIR) -l$(OPENCL_LIBNAME)

TESTS = test_arg_int test_arg_int2 test_arg_int4 \
	test_arg_uint test_arg_uint2 test_arg_uint4 \
	test_arg_float test_arg_float2 test_arg_float4 \
	test_arg_double test_arg_double2 \
	test_block_int test_block_float test_special 

#TESTS = test_faf

TOPDIR ?= ../../


#INCS = -I$(TOPDIR)/include
#INCS = -I/usr/local/browndeer/include
#LIBS = -lm -L/usr/local/browndeer/lib -locl
##LIBS = -lm -L/usr/local/atistream/beta4/lib/x86_64 -lOpenCL

### select OpenCL implementation to test
#INCS += $(USE_OPENCL_INCS)
#LIBS += $(USE_OPENCL_LIB) -lOpenCL
#INCS += -I$(INSTALL_INCLUDE_DIR)
#LIBS += -L$(INSTALL_LIB_DIR) -locl
INCS += $(USE_OPENCL_INCS)
LIBS += $(USE_OPENCL_LIBS)

LIBS += -lm

SRC_TESTS = $(addsuffix .c,$(TESTS))
CLSRC_TESTS = $(addsuffix .cl,$(TESTS))
PRG_TESTS = $(addsuffix .x,$(TESTS))

#all: $(addsuffix .x,$(TESTS))  
all: $(PRG_TESTS)

#.PHONEY: $(addsuffix .x,$(TESTS))
.PHONEY: $(PRG_TESTS)

.SUFFIXES:
.SUFFIXES: .c .o

#CFLAGS += -g

clinfo.x: clinfo.o
	$(CC) $(CFLAGS) $(INCS) -o clinfo.x clinfo.c $(OBJS) $(LIBS)

test.x: test.o
	$(CC) $(CFLAGS) $(INCS) -o test.x test.c $(OBJS) $(LIBS)

test_%.x: ./scripts/gen_c_test_%.pl ./scripts/gen_cl_test_%.pl
	./scripts/gen_c_test_$*.pl > test_$*.c
	./scripts/gen_cl_test_$*.pl > test_$*.cl
	$(CC) $(CFLAGS) $(INCS) -o $@ test_$*.c $(OBJS) $(LIBS)

.c.o:
	$(CC) $(CFLAGS) $(DEF) $(INCS) -c $<

test: clean clinfo.x $(addsuffix .x,$(TESTS))
	./clinfo.x
	./scripts/run_tests.sh $(PRG_TESTS)

quicktest: clean clinfo.x $(addsuffix .x,$(TESTS))
	./clinfo.x
	./scripts/run_quick_tests.sh $(PRG_TESTS)


clean:
	rm -f *.o *.x
	rm -f $(SRC_TESTS) $(CLSRC_TESTS)

distclean: clean
	rm -f ./scripts/run_tests.sh ./scripts/run_quick_tests.sh

