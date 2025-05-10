# ece-251a-spring-2025-homework-8-Amanda6623

## 1 (textbook problem 4.16)
<§4.6> In this exercise, we examine how pipelining affects the clock cycle time of the processor. Problems in this exercise assume that individual stages of the datapath have the following latencies:\
IF=250ps; ID=350ps; EX=150ps; MEM=300ps; WB=200ps\
Also, assume that instructions executed by the processor are broken down as follows:\
ALU/Logic=45%; Jump/Branch=20%; Load=20%; Store=15%

#1.a <§4.6> What is the clock cycle time in a pipelined and non-pipelined processor?\
pipelined: 350ps\
non-pipelined: 250 + 350 + 150 + 300 + 200 = 1250ps

#1.b <§4.6> What is the total latency of an lw instruction in a pipelined and non-pipelined processor?\
pipelined: 350ps\
non-pipelined: 350 * 5 = 1750ps

#1.c <§4.6> If we can split one stage of the pipelined datapath into two new stages, each with half the latency of the original stage, which stage would you split and what is the new clock cycle time of the processor?\
I would split ID and the new clock cycle time of the processor is 300ps.

#1.d <§4.6> Assuming there are no stalls or hazards, what is the utilization of the data memory?\
35%

#1.e <§4.6> Assuming there are no stalls or hazards, what is the utilization of the write-register port of the “Registers” unit?\
65%


## 2 (textbook problem 4.18)
<§4.6> Assume that $s0 is initialized to 11 and $s1 is initialized to 22.\
Suppose you executed the code below on a version of the pipeline from Section 4.6 that does not handle data hazards (i.e., the programmer is responsible for addressing data hazards by inserting NOP instructions where necessary). What would the final values of registers $s2 and $s3 be?\
addi $s0, $s1, 5\
add $s2, $s0, $s1\
addi $s3, $s0, 15

$s2 = 49\
$s3 = 42


## 3 (textbook problem 4.20)
<§4.6> Add NOP instructions to the code below so that it will run correctly on a pipeline that does not handle data hazards.\
addi $s0, $s1, 5\
NOP \
NOP \
NOP \
add $s2, $s0, $s1\
addi $s3, $s0, 15\
NOP \
NOP \
NOP \
add $s4, $s2, $s1

## 4 (textbook problem 4.27)
<§4.8> Problems in this exercise refer to the following sequence of instructions, and assume that it is executed on a five-stage pipelined datapath:\
add $s3, $s1, $s0\
lw $s2, 4($s3)\
lw $s1, 0($s4)\
or $s2, $s3, $s2\
sw $s2, 0($s3)

#4.a <§4.8> If there is no forwarding or hazard detection, insert NOPs to ensure correct execution.\
add $s3, $s1, $s0\
NOP \
NOP \
NOP \
lw $s2, 4($s3)\
lw $s1, 0($s4)\
NOP \
NOP \
NOP \
or $s2, $s3, $s2\
NOP \
NOP \
NOP \
sw $s2, 0($s3)

#4.b <§4.8> Now, change and/or rearrange the code to minimize the number of NOPs needed. You can assume register $t0 can be used to hold temporary values in your modified code.\
add $s3, $s1, $s0\
lw $s2, 4($s3)\
lw $s1, 0($s4)\
or $t0, $s3, $s2\
sw $t0, 0($s3)

#4.c <§4> If the processor has forwarding, but we forgot to implement the hazard detection unit, what happens when the original code executes?\
The or $s2, $s3, $s2 operation won't work as it's supposed to because lw $s2, 4($s3) accesses memory at the same time as when the or decodes the instruction.

#4.d <§4.8> If there is forwarding, for the first seven cycles during the execution of this code, specify which signals are asserted in each cycle by hazard detection and forwarding units in Figure 4.59.\
The $s2 in the or instruction is asserted by hazard detection.

#4.e <§4.8> If there is no forwarding, what new input and output signals do we need for the hazard detection unit in Figure 4.59? Using this instruction sequence as an example, explain why each signal is needed.\
We would need a new input to determine if the destination of a previous instruction is a source register of a following instruction. A new output signal would be needed to stall instruction fetch and insert a NOP.

## 5 (textbook problem 4.30)
This exercise explores how exception handling affects pipeline design. Refer to the following two instructions:\
Instruction 1 = beqz $s0, LABEL\
Instruction 2 = ld $s0, 0($s1)\
#5.a <§4.10> Which exceptions can each of these instructions trigger? For each of these exceptions, specify the pipeline stage in which it is detected.\
beqz could trigger an illegal intruction which is detected in the ID stage.\
ld could trigger address misalignment, page fault, or access fault which would be detected in the MEM stage.

#5.b <§4.10> If the second instruction is fetched immediately after the first instruction, describe what happens in the pipeline when the first instruction causes the first exception you listed in Exercise 4.30.1. Show the pipeline execution diagram from the time the first instruction is fetched until the time the first instruction of the exception handler is completed.\

The pipeline flushes the next instruction and the processor sets up the exception handler. The first instruction of the handler begins execution by cycle 4.
