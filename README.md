i stink at programming but i am trying

use this gcc command to translate the c code into assembly

you need gcc and gdb btw

$ gcc -S -O -fno-asynchronous-unwind-tables -fcf-protection=none [file-name]

don't run the assembler or linker
optimize the code
we are not generating the unwind table which is useful for debugging
disables control-flow protection
