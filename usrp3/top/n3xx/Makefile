#
# Copyright 2016 Ettus Research LLC
#

# NOTE: All comments prefixed with a "##" will be displayed as a part of the "make help" target
##-------------------
##USRP N3XX FPGA Help
##-------------------
##Usage:
## make <Targets> <Options>
##
##Output:
## build/usrp_<product>_fpga_<image_type>.bit:    Configuration bitstream with header
## build/usrp_<product>_fpga_<image_type>.bin:    Configuration bitstream without header
## build/usrp_<product>_fpga_<image_type>.lvbitx: Configuration bitstream for PCIe (NI-RIO)
## build/usrp_<product>_fpga_<image_type>.rpt:    Build report (includes utilization and timing summary)

# Debug Options
# Uncomment the following line to add a debug UART on GPIO 10 & 11
#OPTIONS += DEBUG_UART=1

GIGE_DEFS=BUILD_1G=1 SFP0_1GBE=1 SFP1_1GBE=1 $(OPTIONS)
HG_DEFS=BUILD_1G=1 BUILD_10G=1 SFP0_1GBE=1 SFP1_10GBE=1 $(OPTIONS)
#HG_DEFS=BUILD_1G=1 SFP0_1GBE=1 $(OPTIONS)
XG_DEFS=BUILD_10G=1 SFP0_10GBE=1 SFP1_10GBE=1 $(OPTIONS)
HA_DEFS=BUILD_1G=1 BUILD_AURORA=1 SFP0_1GBE=1 SFP1_AURORA=1 $(OPTIONS)
XA_DEFS=BUILD_10G=1 BUILD_AURORA=1 SFP0_10GBE=1 SFP1_AURORA=1 $(OPTIONS)

# vivado_build($1=Target, $2=Device, $3=Definitions)
vivado_build = make -f Makefile.n3xx.inc $1 NAME=$@ ARCH=$(XIL_ARCH_$2) PART_ID=$(XIL_PART_ID_$2) $3 EXTRA_DEFS="$3"

# vivado_build($1=Device, $2=Option)
post_build = @\
	mkdir -p build; \
	echo "Exporting bitstream files..."; \
	cp build-$(1)_$(2)/n310.bin build/usrp_`echo $(1) | tr A-Z a-z`_fpga_$(2).bin; \
	cp build-$(1)_$(2)/n310.bit build/usrp_`echo $(1) | tr A-Z a-z`_fpga_$(2).bit; \
	echo "Exporting build report..."; \
	cp build-$(1)_$(2)/build.rpt build/usrp_`echo $(1) | tr A-Z a-z`_fpga_$(2).rpt; \
	echo "Build DONE ... $(1)_$(2)";

##
##Supported Targets
##-----------------

all:      N310_HG N310_XG ##(Default target)

N310_1G:  ##USRP N310. 1GigE on both SFP+ ports. DRAM TX FIFO (Experimental!).
	$(call vivado_build,bin,N310,$(GIGE_DEFS) N310=1)
	$(call post_build,N310,1G)

N310_HG:  ##USRP N310. 1GigE on SFP+ Port0, 10Gig on SFP+ Port1. DRAM TX FIFO.
	$(call vivado_build,bin,N310,$(HG_DEFS) N310=1)
	$(call post_build,N310,HG)

N310_XG:  ##USRP N310. 10GigE on both SFP+ ports. DRAM TX FIFO.
	$(call vivado_build,bin,N310,$(XG_DEFS) N310=1)
	$(call post_build,N310,XG)

N310_HA:  ##USRP N310. 1Gig on SFP+ Port0, Aurora on SFP+ Port1. DRAM TX FIFO (Experimental!).
	$(call vivado_build,bin,N310,$(HA_DEFS) N310=1)
	$(call post_build,N310,HA)

N310_XA:  ##USRP N310. 10Gig on SFP+ Port0, Aurora on SFP+ Port1. DRAM TX FIFO (Experimental!).
	$(call vivado_build,bin,N310,$(XA_DEFS) N310=1)
	$(call post_build,N310,XA)

clean:    ##Clean up all target build outputs.
	@echo "Cleaning targets..."
	@rm -rf build-N3*_*
	@rm -rf build

cleanall: ##Clean up all target and ip build outputs.
	@echo "Cleaning targets and IP..."
	@rm -rf build-ip
	@rm -rf build-N3*_*
	@rm -rf build

help:     ##Show this help message.
	@grep -h "##" Makefile | grep -v "\"##\"" | sed -e 's/\\$$//' | sed -e 's/##//'

##
##Supported Options
##-----------------
##GUI=1     Launch the build in the Vivado GUI.

.PHONY: all clean cleanall help