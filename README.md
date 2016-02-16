dcpu-floats
============

NOTE: New 3rd generation floating point library is just about to obsolete this.

    https://github.com/orlof/dcpu-fp32

THIS IS A NAIVE FLOATING POINT IMPLEMENTATION FOR DCPU 

FEATURES

Available functions (in c-like format):
```
extern uint16 atof(uint16 *s, float *f)
extern void ftoa(float *f, uint16 *buf)
extern void itof(int *i, float *f)
extern uint16 ftoi(float *f, int *i)
extern uint16 fcmp(float *f1, float *f2)
extern void fadd(float *sum, float *term1, float *term2)
extern void fsub(float *difference, float *minuend, float *subtrahend)
extern void fmul(float *product, float *factor1, float *factor2)
extern void fdiv(float *quotient, float *divident, float *divisor)
```

SHORTCOMINGS

 - Incorrect rounding (only support for truncate)
 - No denormalized numbers
 - No double
 - Not optimized
 - Bugs:
   - ftoa not implemented (quick impl for testing)
   - return values not yet implemented
   - many others

HOWEVER... 
This library should give reasonable answers to basic calculations.
Later releases will try to address the shortcomings.

USAGE

Based on the Pascal programming language's call convention, 
the parameters are pushed on the stack in left-to-right order, 
and the callee is responsible for balancing the stack before 
return.

 - All methods pollute x, other registers are preserved
 - Possible return value is stored in x
 - Z is used as Frame Pointer (fp)
 - Floats are stored in memory using IEEE754 single precision 
   format (32 bits = 2 memory locations)

This means that local variable space starts from z+1 and continues to
z+2, z+3 etc. Arguments are located in z-11 (rightmost), z-12 
(second from right), etc.

Example

```
; convert 1st string to float
set push, stc1              ; 1st arg for atof (input string)
set push, ftc1              ; 2nd arg for atof (target float = 32 bits)
jsr atof                    ; call to atof, no return value

; convert 2nd string to float
set push, stc2
set push, ftc2 
jsr atof 

; division of floats
set push, float_result      ; quotient address where fdiv stores the result
set push, ftc1              ; divident
set push, ftc2              ; divisor
jsr fdiv                    ; call fdiv, no return value

; convert result float to string
set push, float_result      ; input float (quotient)
set push, string_result     ; destination string buffer
jsr ftoa

hang: 
set pc, hang 

:stc1 dat "123.456", 0      ; divident as string
:ftc1 dat 0,0               ; space to store divident as float

stc2 dat "654.321", 0       ; divisor as string
:ftc2 dat 0,0               ; space to store divisor as float

float_result dat 0,0       ; space to store quotient as float
:string_result dat "this is just a place holder for ftoa"
```
