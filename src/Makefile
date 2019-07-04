include config.mk

EXT := cu 

OBJS := rinterface.o mi.o sort.o granger.o qrdecomp.o correlation.o hcluster.o distance.o matmult.o lsfit.o kendall.o cuseful.o

R_HOME := $(shell R RHOME)

#compiler/preprocessor options
INCS := -I. -I"$(CUDA_INC)" -I"$(R_INC)"
PARAMS := $(DEVICEOPTS) -Xcompiler $(CPICFLAGS)

#linker options
LD_PARAMS := $(DEVICEOPTS) -Xlinker '$(RPATH) $(R_FRAMEWORK)'
LIBS :=  -L"$(R_LIB)" -L"$(CUDA_LIB)" -lcublas $(shell R CMD config BLAS_LIBS)

TARGETS := gputools.so

NVCC := $(CUDA_HOME)/bin/nvcc -gencode arch=compute_53,code=sm_53 -gencode arch=compute_60,code=sm_60

all: $(TARGETS) 

$(TARGETS): $(OBJS)
	$(NVCC) -shared $(LD_PARAMS) $(LIBS) $(OBJS) -o $@

$(OBJS): %.o: %.$(EXT)
	$(NVCC) -c $(INCS) $(PARAMS) $^ -o $@

clean:
	rm -rf *o

.PHONY: all clean
