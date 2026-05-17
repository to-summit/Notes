## Roadmap
1. What's a computer ? CPU + memory
2. What are registers ? Special memory units resides in CPU.
3. What is an ALU ? A multi-functional sequential logical circuits, can use control signal to select a specific functionality.
4. What is memory ? Where the instructions and data are stored. 
## What's a wire
Wire is intended to transmit bit. For validity, we hope there is only one device write the wire at a time and also there is no device read the wire when there is no device is writing. Instead of thinking it as a discrete units, thinking it as a pipe, electrons flow in it.

## What's a bus
Bus is a bunch of wires. How about connect each part of a computer directly ? We need $O(n^2)$ wires and we need every device have more inputs and outputs.

## What's a clock
Clock is not the usual time, it just like a metronome. We use clock to synchronize the events of CPU. Circuits consist of sequential circuit and combinational circuit. Only during a edge the sequential circuit can change its output. 
> Why we always want to make the circuit smaller ? Less time spent between two ends of a wire so that wo can have more high clock rate.

## What's a register
A sequential device. 
![[Pasted image 20260517204615.png]]
![[Pasted image 20260517204629.png]]

## How many bits to label N items
You want to identify one of N things , and so you need to use $ceil(lgN)$ bits to do so.
## What's is an instruction
A character string can be translated into a valid binary bitstring. In order to understand how CPU works, you must learn to read the translated machine code. 
