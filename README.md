# MASM-Tutorials

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
; Program template (Template.asm)
.386
.model flat,stdcall
.stack 4096
ExitProcess PROTO, dwExitCode:DWORD
.data
; declare variables here
.code
main PROC
; write your code here
INVOKE ExitProcess,0
main ENDP
END main
~~~~
