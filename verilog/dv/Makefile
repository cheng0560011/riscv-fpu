# //////////////////////////////////////////////////////////////////////////////
# // SPDX-FileCopyrightText: 2021, Dinesh Annayya
# // 
# // Licensed under the Apache License, Version 2.0 (the "License");
# // you may not use this file except in compliance with the License.
# // You may obtain a copy of the License at
# //
# //      http://www.apache.org/licenses/LICENSE-2.0
# //
# // Unless required by applicable law or agreed to in writing, software
# // distributed under the License is distributed on an "AS IS" BASIS,
# // WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# // See the License for the specific language governing permissions and
# // limitations under the License.
# // SPDX-License-Identifier: Apache-2.0
# // SPDX-FileContributor: Dinesh Annayya <dinesha@opencores.org>
# // //////////////////////////////////////////////////////////////////////////
#------------------------------------------------------------------------------
# Makefile for Synthesis
#------------------------------------------------------------------------------

# Paths
export ROOT_DIR := $(shell pwd)

### To Enable IVERILOG FST DUMP
export IVERILOG_DUMPER = fst
VPI_PATH:=$(VCS_HOME)/include


# Targets
.PHONY: clean rtl gate help

default: clean rtl

vcs_sim: clean
	gcc -shared -o fpu_sp.so fpu_sp.c -I$(VPI_PATH) -fPIC
	gcc -shared -o fpu_sp_tc.so fpu_sp_tc.c -I$(VPI_PATH) -fPIC
	vcs -full64 -sverilog -timescale=1ns/1ps -debug_all -l vcs.log -f filelist_rtl.f -top tb_top -CFLAGS "-fPIC" -load ./fpu_sp.so -load ./fpu_sp_tc.so -o simv
	./simv

rtl: clean  
	iverilog-vpi fpu_sp.c
	iverilog-vpi fpu_sp_tc.c
	iverilog -g2012  -D SPEEDSIM  -D WFDUMP -DFUNCTIONAL  tb_top.sv -f filelist_rtl.f -o tb_top.vvp
	vvp -M. -m fpu_sp -m fpu_sp_tc tb_top.vvp

gate: clean  
	iverilog-vpi fpu_sp.c
	iverilog-vpi fpu_sp_tc.c
	iverilog -g2012  -D GL -D SPEEDSIM  -DFUNCTIONAL  tb_top.sv -f filelist_gate.f  -o tb_top.vvp
	vvp -M. -m fpu_sp -m fpu_sp_tc tb_top.vvp

help:
	@echo "To run RTL  simulation: make rtl"
	@echo "To run Gate simulation: make gate"



clean:
	$(RM) -R *.vpi
	$(RM) -R *.vvp
	$(RM) -R *.vcd
	$(RM) -R *.fst
	$(RM) -R *.o
	$(RM) -rf core csrc simv* vc_hdrs.h ucli.key urg* *.log *.fsdb novas.* verdiLog *.so
