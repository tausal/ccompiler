i stink at programming but i am trying

use this gcc command to translate the c code into assembly

you need gcc and gdb btw

$ gcc -S -O -fno-asynchronous-unwind-tables -fcf-protection=none [file-name]

<ul>
<li>don't run the assembler or linker</li>
<li>optimize the code</li>
<li>we are not generating the unwind table which is useful for debugging</li>
<li>disables control-flow protection</li>
</ul>
To verify that the assembly works, assemble and link it, run it, and check the exit code with the $? operator:
$ gcc file.s -o file
$ ./file
$ echo %?

Before starting on the compiler, we shall write a compiler driver. It should convert a source file to an executable in 
three steps:

<ol>
<li>Run this command to preprocess the source file:
gcc -E -P INPUT_FILE -o PREPROCESSED_FILE
-E tells GCC to run only the preprocessor
-P tells the preprocessor to NOT emit the linemarkers
PREPROCESSED_FILE should have a .i file extension
</li>
<li>
Compile the proprocessed source file and output an assembly file with a .s extension. Delete the preprocessed file when you're done with it.
</li>
<li>
Assembly and link the assembly file to produce an executable, using this command
<code>gcc ASSEMBLY_FILE -o OUTPUT_FILE</code>
Delete the assembly file when you're done with it.
</li>

</ol>

The compiler driver must have a specific command line interface so the test script can run it. It must be a command line program that accepts a path to a C source file as its only argument. If this command succeeds it must produce an executable in the same directory as the input file with the same name. If the compiler files it should return a nonzero exit code and shouldn't write any assembly or executable files.

The compiler driver should support the following options:

<ul>
<li><code>--lex </code> Directs it to run the lexer, but stop before parsing</li>
<li><code>--parse </code> Directs it to run the lexer and the parser, but stop before assembly generation </li>
<li><code>--codegen</code> Directs it to perform lexing, parsing and assembly generation, but stop before code emission
</ul>

None of these options should produce any output files, and all should terminate with an exit code of 0 if they don't hit any errors.

<h2> The Lexer </h2>

The lexer should read in a source file and produce a list of tokens. You need to know what tokens you might encounter.

An identifier is an ASCII letter of underscore followed by a mix of letters, underscores, and digits. Identifiers are case sensitive. An integer constant consists of one or more digits. Each identifier token produced by the lexer must retain its specific name. Each constant needs to hold an integer value. By contrast, there's only one possible return keyword, so a return keyword token doesn't need to store any extra information. WHITESPACE TOKENS.
You can recognize each token type with a regular expression.
