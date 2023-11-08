# matrixmul8085
Matrix Multiplication using 8085 Assembly Language
Code for multiplication

; code for multiplication of 
; two numbers by repeated 
; addition 
; two numbers to be multiplied 
; are stored in 0002H and 
; 0003H, 
; output is stored in 0004H 
MOV B, 0002H  ; Load the first number into B register
MOV C, 0003H  ; Load the second number into C register
MVI A, 00H    ; Initialize accumulator A to 0

LOOP:
ADD B         ; Add the value in B to the accumulator A
DCR C         ; Decrement the value in C
JNZ LOOP      ; Jump back to LOOP if C is not zero
STA 0004H     ; Store the result in memory location 0004H

Multiplying row vector with column vector 

LXI H, 8500H  ; Load the address of the row vector
PUSH 8508H    ; Push the address of the column vector onto the stack

Method:
MOV M, A      ; Move the value in A to the memory location pointed by H
XCHG          ; Exchange H and D registers
MOV M, B      ; Move the value in B to the memory location pointed by H
CALL MUL      ; Call the MUL subroutine
STA 8516H     ; Store the result in memory location 8516H
INX H         ; Increment the address in H
XCHG          ; Exchange H and D registers
INX H         ; Increment the address in H
JMP Method    ; Jump back to the Method

Matrix Multiplication

	MVI C, 002H  ; Initialize C with 2 (number of rows)
MVI D, 002H  ; Initialize D with 2 (number of columns)

Method2:
DCR C
Method3: CALL MRC
DCR D
JNZ Method4

Method4:
ORI C, 00H
JNZ Method5

Method5:
LXI H, 8500H
JMP Method3

ORI C, 00H
JZ Method6

Method6:
LXI H, 8508H
JMP Method3

ORI D, 00H
JZ Method7

Method7:
INX H, 8508H
MVI D, 002H
ORI C, 00H
JNZ Method3

ORI C, 00H
JZ Method8

Method8:
HLT

Final Code
	
	MVI C, 00     ; Initialize C to 0
LXI H, 8500    ; Load the address of the first matrix

LOOP2:
LXI D, 8600    ; Load the address of the second matrix
CALL MUL      ; Call the MUL subroutine
MOV B, A       ; Move the result to B
INX H          ; Increment the address in H
INX D          ; Increment the address in D
INX D
CALL MUL      ; Call the MUL subroutine
ADD B          ; Add the result to B
CALL STORE    ; Call the STORE subroutine
DCX H          ; Decrement the address in H
DCX D          ; Decrement the address in D
CALL MUL      ; Call the MUL subroutine
MOV B, A       ; Move the result to B
INX H          ; Increment the address in H
INX D          ; Increment the address in D
INX D
ADD B          ; Add the result to B
CALL STORE    ; Call the STORE subroutine
MOV A, C       ; Move the value of C to A
CPI 04         ; Compare A with 4
JZ LOOP1       ; If A is 4, jump to LOOP1
INX H          ; Increment the address in H
JMP LOOP2      ; Jump back to LOOP2
LOOP1:
HLT            ; Halt the processor
MUL: LDAX D    ; Load the accumulator A with the value at the memory address pointed to by D
MOV D, A      ; Move the value in A to register D
MOV H, M      ; Move the value from the memory location pointed by H to register H
DCR H         ; Decrement the value in H
JZ LOOP3      ; Jump to LOOP3 if H becomes zero
LOOP4: ADD D   ; Add the value in D to A
DCR H         ; Decrement H
JNZ LOOP4     ; Jump back to LOOP4 if H is not zero
LOOP3: MVI H,85 ; Load H with 85 (assuming it points to the first matrix)
MVI D,86     ; Load D with 86 (assuming it points to the second matrix)
RET           ; Return from the subroutine
