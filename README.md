INCLUDE Irvine32.inc

.data

success byte "Successfully matched the letters",0
input byte 3 dup (?)
msg byte "Enter missing letter in the word",0 
string byte 'mizzly',0
position dword 3 dup(?)
loop_val dword ?
store_val byte 6 dup(?)
count dword ?
temp dword ?

.code
main proc

;call dumpregs
;mov eax,2h
;call writeint 
;call dumpregs

mov eax,lengthof string
dec eax

mov edx,0
mov ebx,2
div ebx
mov  ecx,eax
mov loop_val,ecx
call dumpregs
mov esi,0

call randomize

;generating random numbers for random spaces

l:
mov eax,lengthof string
dec eax
call randomrange
call writeint
mov position[esi],eax
add esi,4
loop l

;checking wether random numbers are equal

;case1:comparing first 2 if equal

checking:
mov esi,0
mov ebx,position[esi]
add esi,4
cmp ebx,position[esi]
je random_again
jmp checking2

;jmp terminate

random_again:
mov eax,lengthof string 
call randomrange
mov position[esi],eax
jmp checking

;case2:comparing last 2 if same

checking2:
mov ebx,position[esi]
add esi,4
cmp ebx,position[esi]
je random_again
jmp checking3


checking3:

mov esi,0
mov ebx,position[esi]
add esi,8
cmp ebx,position[esi]
je random_again
jmp terminate

terminate:

display:
mov ecx,lengthof position
mov esi,0
call crlf

l1:
mov eax,position[esi]
call writeint
add esi,4
loop l1

;inserting spaces at random positions
CALL CRLF
mov ecx,loop_val
mov bl,"_"
mov esi,0
mov ebx,0

ran_pos:
mov edi,position[esi]
;storing value at that position in edx

mov dl,string[edi]
;moving that value to array store_val
;movzx eax,dl

mov store_val[edi],dl
mov string[edi],"_"

add esi,4
add ebx,1
loop ran_pos

;checking which random letters are reaplaced by underscores


mov edx,offset store_val
call writestring
call crlf

;sorting position array
;//////////////////
mov ecx, LENGTHOF position
dec ecx
LL3:
mov esi, OFFSET position
mov edi, OFFSET position+4
mov count, ecx
mov ecx, LENGTHOF position
dec ecx
LL1:
mov eax, [esi]
mov ebx, [edi]
cmp eax, ebx
jle LESS
mov [esi], ebx
mov [edi], eax
LESS:
add esi, TYPE position
add edi, TYPE position
LOOP LL1
mov ecx, count
LOOP LL3

mov ecx, LENGTHOF position
mov esi, OFFSET position
LL2:
mov eax, [esi]
call WriteInt
call Crlf
add esi, TYPE position
LOOP LL2

;////////////////////////////

;displaying msg

 mov esi,0
 mov ecx,3


IINPUT:
mov edx,offset msg
call writestring
call crlf
call crlf


mov edx,offset string
call writestring
call crlf
call crlf

;taking input

mov temp,ecx ;to restore value of terminating condition

mov ecx,lengthof input
mov edx,offset input
call readstring

mov ecx,temp

;checking if stores the input or not
;mov edx,offset input 
;call writestring
;call crlf
 
 mov edi,position[esi]  ;MOVING POSITION IN EDI
 mov al,store_val[edi]  ;STORED VALUE IN AL
 cmp input,al
 je insert
 jmp IINPUT

 insert:
 add esi,4
 dec ecx
 mov string[edi],al
 mov edx,offset string
 call writestring
 call crlf
 mov ebx,0
 cmp ecx,ebx
 je go
 jmp IINPUT


go:

mov edx,offset success
call writestring

exit
main endp
end main





