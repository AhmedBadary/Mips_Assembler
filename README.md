# Mips_Assembler
An assembler that translates a subset of the MIPS instruction set to machine code.

## Description
In this project, we will be writing an assembler that translates a subset of the MIPS instruction set to machine code. Our assembler is a two-pass assembler. However, we will only assemble the .text segment. At a high level, the functionality of our assembler can be divided as follows:  Pass 1: Reads the input (.s) file. Comments are stripped, pseudoinstructions are expanded, and the address of each label is recorded into the symbol table. Input validation of the labels and pseudoinstructions is performed here. The output is written to an intermediate (.int) file .  Pass 2: Reads the intermediate file and translates each instruction to machine code. Instruction syntax and arguments are validated at this step. The relocation table is generated, and the instructions, symbol table, and relocation table are written to an object (.out) file.
Now, we will continue where we left off by implementing a linker in MIPS. The linker processes object files (which in our project are .out files) and generates an executable file. In the rest of this document, "input" will be used interchangeably with "object file", and "output" with "executable file".

The linker has two main tasks, combining code and relocating symbols. Code from each input file's .text segment is merged together to create an executable. This also determines the absolute address of each symbol (recall that the assembler outputs a symbol table containing the relative address of each symbol). Since the absolute address is known, instructions that rely on absolute addressing can have the addresses filled in.


## Running the Assembler
First, make sure your assembler executable is up to date by running `make`.

By default, the assembler runs two passes. The first pass reads an input file and translates it into an intermediate file. The second pass reads the intermediate file and translates it into an output file. To run both passes, type:

```./assembler <input file> <intermediate file> <output file>```

Alternatively, you can run only a single pass. To run only the first pass, use the -p1 flag:

```./assembler <-p1> <input file> <intermediate file>```

To run only the second pass, use the '-p2' flag. Note that when running pass two only, your symbol and relocation table will be empty since labels were stripped in 'pass_one()', so it may affect your branch instructions.

```./assembler <-p2> <intermediate file> <output file>```
When testing cases that should produce error messages, you may want to use the -log flag to log error messages to a text file. The -log flag should be followed with the location of the output file (WARNING: old contents will be overwritten!), and it can be used with any of the three modes above.


## Running the Linker
The linker accepts an arbitrary number of input (object) file names followed by an output file name. The input files will be combined, and the result be placed in the output file. If the output file already exists, it will be overwritten. The files will be combined in the order that they were given (eg. input file 1 goes first, then input file 2, etc).

If you want to run from the command line, we have also give you an executable called linker You may first need to turn on the execute permission:

```chmod u+x linker```
Afterwards, you can pass in the input and output file names by supplying them as arguments:

```./linker <input 1 name> <input 2 name> ... <input N name> <output name>```
We have give some sample input-output pairs, which may be useful as a reference during step 4. The inputs are located in link-in and outputs are located in link-out. linker1.out (recall that we use .out to denote object files, which are inputs to the linker) and linker2.out are standalone test cases, but linker3A.out and linker3B.out should be linked together:
```
./linker link-in/linker1.out link-out/output1
./linker link-in/linker2.out link-out/output2
./linker link-in/linker3A.out link-in/linker3B.out link-out/output3
```
You can compare your output to the reference outputs in the link-out/ref directory. diff may be useful:
```
diff link-out/output1 link-out/ref/output1_ref
diff link-out/output2 link-out/ref/output2_ref
diff link-out/output3 link-out/ref/output3_ref
```