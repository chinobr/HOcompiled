1. Escriban qué esperan de cada uno de los pasos

#Paso 1: preprocesador
esperaría que compute el header calculator.h , el cual contiene el studio y la función add_numbers, la cual devuelve un entero a partir del input de dos números enteros a,b


#Paso 2: compilacion I
en este paso, donde se hace la compilación de C a assembler, se tiene que generar el archivo de "traducción" desde el primero a este último. En lenguaje assembler lo que voy a tener es una representación simbólica de los códigos de máquina binarios


#Paso 3: compilacion II
ahora acá vamos a pasar es de assembler a los binarios de la cpu. 
para visualizarlo tendré que usar `nm` (en gedit no puedo)


#Paso 4: linkeo
acá la onda sería linkear el objeto en un ejecutable para poder, justamente, ejecutar el código


2. ¿Qué agregó el preprocesador?

al hacer `make processing` me devuelve: 
`gcc -E calculator.c -o calculator.pp_c`

esta orden creó el archivo `calculator.pp_c`, el cual castea librerías como `features.h`, `cdefs.h`, etc



3. Identificar en la rutina de assembler las funciones

al correr `make assemblr` me devuelve:
`gcc -masm=intel -S calculator.c -o calculator.asm`

esta orden creó el archivo `calculator.asm`, el cual contiene:

######################################################################
######################################################################
######################################################################
	.file	"calculator.c"
	.intel_syntax noprefix
	.section	.rodata
	.align 8
[.LC0:]`<------------------funcion printf`
	.string	"I know how to add! 31 + 11 is %d\n"
	.text
	.globl	main
	.type	main, @function 

[main:] `<------------------funcion main`
.LFB0:
	.cfi_startproc
	push	rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	mov	rbp, rsp
	.cfi_def_cfa_register 6
	sub	rsp, 32
	mov	DWORD PTR [rbp-20], edi
	mov	QWORD PTR [rbp-32], rsi
	mov	esi, 11
	mov	edi, 31
	call	add_numbers
	mov	DWORD PTR [rbp-4], eax
	mov	eax, DWORD PTR [rbp-4]
	mov	esi, eax
	mov	edi, OFFSET FLAT:.LC0
	mov	eax, 0
	call	printf
	mov	eax, 0
	leave
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
[.LFE0:] `<-----------funcion add_numbers`
	.size	main, .-main
	.globl	add_numbers
	.type	add_numbers, @function
add_numbers:
.LFB1:
	.cfi_startproc
	push	rbp
	.cfi_def_cfa_offset 16
	.cfi_offset 6, -16
	mov	rbp, rsp
	.cfi_def_cfa_register 6
	mov	DWORD PTR [rbp-4], edi
	mov	DWORD PTR [rbp-8], esi
	mov	edx, DWORD PTR [rbp-4]
	mov	eax, DWORD PTR [rbp-8]
	add	eax, edx
	pop	rbp
	.cfi_def_cfa 7, 8
	ret
	.cfi_endproc
.LFE1:
	.size	add_numbers, .-add_numbers
	.ident	"GCC: (Ubuntu 5.4.0-6ubuntu1~16.04.10) 5.4.0 20160609"
	.section	.note.GNU-stack,"",@progbits

######################################################################
######################################################################
######################################################################

.LFXX son etiquetas locales 

--- push, mov, add, leave, ret, etc son instrucciones del CPU


4. Explicar qué quieren decir los símbolos que se crean en el objeto

al correr `make object` me devuelve `gcc -c calculator.c -o calculator.o` y el archivo calculator.o

ej ecutando con `nm` (la cual enlista simbolos desde un archivo objecto) el archivo calculator.o obtengo:

####################################
000000000000003c T add_numbers       [descriptor]
0000000000000000 T main              [descriptor]
                 U printf            [entrada]
####################################


"T"/"t" The symbol is in the text (code) section.

"U" The symbol is undefined.


5. ¿En qué se diferencian los símbolos del objeto y del ejecutable?

al correr `make executable` me devuelve `gcc calculator.o -o calculator.e` y el archico calculator.e

y con nm me devuelve:

################################
0000000000400562 T add_numbers
0000000000601038 B __bss_start
0000000000601038 b completed.7594
0000000000601028 D __data_start
0000000000601028 W data_start
0000000000400460 t deregister_tm_clones
00000000004004e0 t __do_global_dtors_aux
0000000000600e18 t __do_global_dtors_aux_fini_array_entry
0000000000601030 D __dso_handle
0000000000600e28 d _DYNAMIC
0000000000601038 D _edata
0000000000601040 B _end
00000000004005f4 T _fini
0000000000400500 t frame_dummy
0000000000600e10 t __frame_dummy_init_array_entry
0000000000400778 r __FRAME_END__
0000000000601000 d _GLOBAL_OFFSET_TABLE_
                 w __gmon_start__
000000000040062c r __GNU_EH_FRAME_HDR
00000000004003c8 T _init
0000000000600e18 t __init_array_end
0000000000600e10 t __init_array_start
0000000000400600 R _IO_stdin_used
                 w _ITM_deregisterTMCloneTable
                 w _ITM_registerTMCloneTable
0000000000600e20 d __JCR_END__
0000000000600e20 d __JCR_LIST__
                 w _Jv_RegisterClasses
00000000004005f0 T __libc_csu_fini
0000000000400580 T __libc_csu_init
                 U __libc_start_main@@GLIBC_2.2.5
0000000000400526 T main
                 U printf@@GLIBC_2.2.5
00000000004004a0 t register_tm_clones
0000000000400430 T _start
0000000000601038 D __TMC_END__

################

el objeto no puede ejcutarse, al contrario del ejecutable, con la instruccion ./ejecutable 



