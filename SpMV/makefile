.SUFFIXES : .cu .f .c .cpp .o_s .o_d .o
# CUDA path
CUDA_INSTALL_PATH ?= /usr/local/cuda-8.0
# Compilers
NVCC := $(CUDA_INSTALL_PATH)/bin/nvcc 
CC   := gcc
CXX  := g++
FORT := gfortran
LINK := $(CXX)
# Includes
INCLUDES  := -I. -I$(CUDA_INSTALL_PATH)/include
# Common flags
FLAGS := -O3 -DUNIX
# NVCC flags
NVCCFLAGS := -gencode arch=compute_50,"code=sm_50"
# Libs
LIB := -L$(CUDA_INSTALL_PATH)/lib64 -lcudart -lcusparse -fopenmp
# Object files
OBJS1 = spmv.o_s io.o_s testspmv.o_s mmio.o format.o_s auxil.o_s skit_s.o
OBJS2 = spmv.o_d io.o_d testspmv.o_d mmio.o format.o_d auxil.o_d skit_d.o
# Rules
.f.o:
	$(FORT) $(FLAGS) $(INCLUDES) -o $@ -c $<
.c.o:
	$(CC) $(FLAGS) $(INCLUDES) -o $@ -c $<
.cpp.o_s:
	$(CXX) $(FLAGS) -fopenmp -DDOUBLEPRECISION=0 $(INCLUDES) -o $@ -c $<
.cpp.o_d:
	$(CXX) $(FLAGS) -fopenmp -DDOUBLEPRECISION=1 $(INCLUDES) -o $@ -c $<
.cu.o_s:
	$(NVCC) $(NVCCFLAGS) $(FLAGS) -DDOUBLEPRECISION=0 $(INCLUDES) -o $@ -c $<
.cu.o_d:
	$(NVCC) $(NVCCFLAGS) $(FLAGS) -DDOUBLEPRECISION=1 $(INCLUDES) -o $@ -c $<

default: spmvS.ex spmvD.ex
spmvS.ex: $(OBJS1)
	$(LINK) -o spmvS.ex $(OBJS1) $(LIB)
spmvD.ex: $(OBJS2)
	$(LINK) -o spmvD.ex $(OBJS2) $(LIB)
clean:
	find . | egrep "#" | xargs rm -f
	find . | egrep "\~" | xargs rm -f
	rm -f *.o *.o_s *.o_d
	rm -f *.ex

