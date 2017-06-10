# Assembly using NASM
# nasm -f win32 hello.asm
# ld -e _start hello.obj -lkernel32 -o hello.exe

### Doesn't work:
~~~
res = x + y
~~~

### Works:
~~~
mov eax, x
add eax, y
mov res, eax
~~~

## 32-bit Template
~~~~
global _start

extern _GetStdHandle@4
extern _WriteConsoleA@20
extern _ExitProcess@4

section .data
  str:     db 'hello, world',0x0D,0x0A
  strLen:  equ $-str

section .bss
  numCharsWritten:        resd 1

section .text
  _start:

  ;
  ; HANDLE WINAPI GetStdHandle( _In_  DWORD nStdHandle ) ;
  ;
  push    dword -11       ; Arg1: request handle for standard output
  call    _GetStdHandle@4 ; Result: in eax

  ;
  ; BOOL WINAPI WriteConsole(
  ;       _In_        HANDLE hConsoleOutput,
  ;       _In_        const VOID *lpBuffer,
  ;       _In_        DWORD nNumberOfCharsToWrite,
  ;       _Out_       LPDWORD lpNumberOfCharsWritten,
  ;       _Reserved_  LPVOID lpReserved ) ;
  ;
  push    dword 0         ; Arg5: Unused so just use zero
  push    numCharsWritten ; Arg4: push pointer to numCharsWritten
  push    dword strLen    ; Arg3: push length of output string
  push    str             ; Arg2: push pointer to output string
  push    eax             ; Arg1: push handle returned from _GetStdHandle
  call    _WriteConsoleA@20


  ;
  ; VOID WINAPI ExitProcess( _In_  UINT uExitCode ) ;
  ;
  push    dword 0         ; Arg1: push exit code
  call    _ExitProcess@4
~~~~
