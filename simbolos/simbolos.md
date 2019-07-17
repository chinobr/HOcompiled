al ejecutar con nm el objeto visibility.o creado a partir del código visibilirt.c obtengo


###########################
0000000000000000 t add_abs
000000000000002a T main
                 U printf
0000000000000000 r val1
0000000000000004 R val2
0000000000000000 d val3
0000000000000004 D val4
##########################


donde los símbolos significan:

"T" y "t": The symbol is in the text (code) section.

"U": The symbol is undefined.

"R" y "r": The symbol is in a read only data section.

"D" y "d" The symbol is in the initialized data section.
