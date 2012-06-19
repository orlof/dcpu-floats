dcpu-admiral
============
THIS IS A NAIVE FLOATING POINT IMPLEMENTATION FOR DCPU 

FEATURES

Available functions (in c-like format):

      extern uint16 atof(uint16 *s, float *f)

      extern void ftoa(float *f, uint16 *buf)

      extern void itof(int *i, float *f)

      extern uint16 ftoi(float *f, int *i)

      extern uint16 fcmp(float *f1, float *f2)

      extern void fadd(float *sum, float *term1, float *term2)

      extern void fsub(float *difference, float *minuend, float *subtrahend)

      extern void fmul(float *product, float *factor1, float *factor2)

      extern void fdiv(float *quotient, float *divident, float *divisor)

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
