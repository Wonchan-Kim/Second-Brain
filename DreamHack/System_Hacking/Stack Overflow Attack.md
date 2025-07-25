# Stack Overflow Attack

## Summary
A stack overflow is a type of buffer overflow vulnerability that occurs when a program writes more data to the stack than it can hold. This can overwrite adjacent memory, leading to program crashes, unexpected behavior, or, in the worst case, arbitrary code execution.

## How it Works
1.  **Function Call:** When a function is called, a new stack frame is created. This frame includes local variables, function parameters, and the return address (where execution should resume after the function completes).
2.  **Vulnerable Code:** A function that copies data into a buffer on the stack without checking the size of the data can be vulnerable. For example, using `strcpy()` in C with user-supplied input.
3.  **Overflow:** If the input data is larger than the buffer, it will overwrite other data on the stack.
4.  **Hijacking Control Flow:** A carefully crafted input can overwrite the return address on the stack. When the function returns, instead of going back to the caller, it will jump to the address provided by the attacker. This address can point to malicious code (shellcode) also included in the input.

## Identifying Vulnerabilities
*   **Static Analysis:** Reviewing source code for the use of unsafe functions like `strcpy()`, `gets()`, `sprintf()`, etc.
*   **Dynamic Analysis (Fuzzing):** Sending a large amount of random or semi-random data to the application to see if it crashes. Tools like AFL or Peach Fuzzer can be used.
*   **Debugging:** Using a debugger like GDB to step through the program and inspect the stack.

## Exploitation Techniques
*   **Overwriting the Return Address:** The most common technique. The goal is to point the return address to shellcode.
*   **Shellcode:** A small piece of code payload used as the malware. It typically gives the attacker a shell (command-line access) on the target machine.
*   **NOP Sled:** A sequence of No-Operation (NOP) instructions used to "slide" the execution flow to the shellcode. This is useful when the exact address of the shellcode is not known.

## Prevention and Mitigation
*   **Use Safe Functions:** Use functions that check buffer sizes, like `strncpy()` and `snprintf()`.
*   **Stack Canaries:** A random value placed on the stack before the return address. If the canary is overwritten, the program knows a buffer overflow has occurred and can terminate safely.
*   **Data Execution Prevention (DEP) / NX Bit:** Marks the stack as non-executable, so an attacker cannot execute shellcode placed on the stack.
*   **Address Space Layout Randomization (ASLR):** Randomizes the memory addresses of libraries and the stack, making it harder for an attacker to guess the return address.

## Tools
*   **GDB (GNU Debugger):** For inspecting the stack and registers.
*   **IDA Pro / Ghidra:** Disassemblers and decompilers for reverse engineering binaries.
*   **pwntools:** A Python library for writing exploits.

## Examples/Case Studies
*   (Link to specific examples or CTF challenges here)

## Prevention and Mitigation
[[Stack Canaries]]