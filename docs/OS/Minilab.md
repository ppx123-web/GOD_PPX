# MiniLab
## M1 pstree
Attention: what is the output of pstree -V > /dev/null ? STDERR

## M2 coroutine
setjmp/longjmmp: setjmp save the information of the current including the stack!!! When using longjmp, the stack will also return and if the origin stack has been destroyed,...boom!

In fact, the switch among the coroutines is implemented by longjmp, at the beginning of the co_wait, call setjmp and the program will never use the stack of the previous coroutine so this is correct. 

Anothor problem: when a coroutine is called by co_yield and ends before the coroutine called by co_wait, the coroutine need another jmp_buf data structure to call longjmp to the loop in the co_wait.