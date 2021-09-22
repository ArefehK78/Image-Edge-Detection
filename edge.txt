				PRESERVE8
                THUMB

                AREA    RESET, DATA, READONLY
                EXPORT  __Vectors
__Vectors
				DCD  0x20001000     ; stack pointer value when stack is empty
				DCD  Reset_Handler  ; reset vector

				AREA	myCode, CODE, READONLY
;**********************************************************************************
GPIOC_CRL   	EQU     0x40011000
GPIOC_CRH   	EQU     0x40011004
GPIOC_IDR   	EQU     0x40011008
GPIOC_ODR   	EQU     0x4001100C
GPIOC_BSRR  	EQU     0x40011010
GPIOC_BRR   	EQU     0x40011014
GPIOC_LCKR  	EQU     0x40011018
RCC_AHB_1		EQU		0x40023800
GPIOA_BASE		EQU		0x40020000
GPIOB_BASE		EQU		0x40020400
;**********************************************************************************
				ENTRY
Reset_Handler

**********************************************************************************


;            MOV32   r12, #GPIOC_CRH      ; Address for port c control register
;            MOV32   r11, #0x00033330     ; Value for control register
;            STR     r11, [r12]            ; Write to contorl register

;            MOV32   r12, #GPIOC_ODR      ; Address for port c output data register
;            MOV     r11, #0x0A00         ; Value for port c
;            STR     r11, [r12]            ; Write value
			
			
			
;			GPIO s start:
;			MOV32	r12, #RCC_AHB_1				; clock enable
;			LDR		r0, [r12, 0x30]			
;			ORR		r0, r0, #0x00000003
;			STR		r0, [r12]

;			
;			MOV32	r12, #GPIOA_BASE			
;			LDR		r0, [r12]					
;			AND		r0, r0, #0xFFFFFFFC			;00: Input
;			STR		r0, [r12, #0x08]
;			
;			LDR		r0, [r12, #0x08]			
;			AND		r0, r0, #0xFFFFFFFC			;2 MHz
;			STR		r0, [r3, #0x08]
;			
;			LDR		r0, [r12, 0x0C]				
;			AND		r0, r0, #0xFFFFFFFC			;No pull-up/pull-down
;			STR		r0, [r12, #0x0C]
;			
;			
;			
;			MOV32	r3, #GPIOB_BASE					
;			ldr		r0, [r3]					 
;			AND 	r0,	r0, #0xFFFFFFFD 		;01: output 
;			STR     r0, [r3]
;			
;			
;			LDR		r0, [r12, #0x04]			
;			AND		r0, r0, #0xFFFFFFFC			;Push-Pull
;			STR		r0, [r12, #0x08]
;			
;			LDR		r0, [r3, #0x08]				; 2 MHz
;			AND		r0, r0, #0xFFFFFFFC
;			STR     r0, [r3, #0x08]
;			
;			
;			LDR		r0, [r3, #0x0C]				
;			AND		r0, r0, #0xFFFFFFFC			;No pull-up/pull-down
;			STR     r0, [r3, #0x0C]				
;			
;			
;	
;			MOV32	r12, #GPIOA_BASE			; PA0 is on	
;L3			LDR		r0, [r12, #0x10]
;			AND		r0, r0, #0x00000001
;			CMP		r0, #1
;			BNE		L3


edgeF		MOV		r11, #0x20000000
			MOV		r1, #17
			ADR		r2, edge_row
			LDR		r12, edgeF_row_pointer  
			LDR		r3, [r12]
			LDR		r12, edgeF_colmn_pointer
			LDR		r4, [r12]
			
			
LOOP		ADD		r4, r4, #1
			MUL		r5, r3, r1
			ADD		r5, r5, r4
			ADD		r5, r5, r2
			LDRB	r6, [r5]
			ADD		r7, r7, r6
			
			ADD		r4, r4, #-1
			ADD		r3, r3, #1
			MUL		r5, r3, r1
			ADD		r5, r5, r4
			ADD		r5, r5, r2
			LDRB	r6, [r5]
			ADD		r7, r7, r6
			
			
			ADD		r4, r4, #1
			MUL		r5, r3, r1
			ADD		r5, r5, r4
			ADD		r5, r5, r2
			LDRB	r6, [r5]
			MOV		r6, r6, LSL #2
			SUB		r7, r7, r6
			
			
			ADD		r4, r4, #1
			MUL		r5, r3, r1
			ADD		r5, r5, r4
			ADD		r5, r5, r2
			LDRB	r6, [r5]
			ADD		r7, r7, r6
			
			
			ADD		r4, r4, #-1
			ADD		r3, r3, #1
			MUL		r5, r3, r1
			ADD		r5, r5, r4
			ADD		r5, r5, r2
			LDRB	r6, [r5]
			ADD		r7, r7, r6
			
			
			MOVS	r7, r7, LSR	#2
			ADD		r7, r7, #64
			
			AND		r8, r7, #0xFF
			CMP		r8, #100
			MOVHI	r8, #0
			MOVLS	r8, #255
				
			STRB	r8, [r11]
			ADD		r11, r11, #1
				
			
			LDR		r12, edgeF_row_pointer
			LDR		r3, [r12]
			LDR		r12, edgeF_colmn_pointer
			LDR		r4, [r12]
			ADD		r4, r4, #1
			STR		r4, [r12]
			
			MOV		r7, #0
			ADD 	r10, r10, #1
			CMP		r10, #14
			BLS		LOOP
			
			
			MOV		r10, #0
			LDR		r12, edgeF_row_pointer 
			LDR		r3, [r12]
			ADD		r3, r3, #1
			STR		r3, [r12]
			LDR		r12,edgeF_colmn_pointer
			MOV		r4, #0
			STR		r4, [r12]
			ADD		r9, r9, #1
			CMP		r9, #14
			BLS		LOOP

		
;			MOV32	r3, #GPIOB_BASE
;			LDR		r0, [r3, #0x14]
;			ORR 	r0,r0, #0x00000002
;			STR     r0, [r3]
			
		

edge_row		DCB			129,129,124,130,126,127,122,129,14,128,118,125,128,130,138,125,125,129,129,124,130,126,127,122,129,14,128,118,125,128,130,138,125,125,112,112,99,145,131,99,117,128,29,118,93,111,119,133,158,145,145,105,105,97,104,111,134,96,127,11,125,114,98,109,129,114,129,129,107,107,109,117,92,81,105,129,6,126,111,93,78,121,105,118,118,101,101,99,75,101,100,108,122,0,125,76,79,90,94,122,118,118,120,120,71,68,112,116,125,114,1,125,90,75,115,103,79,99,99,129,129,126,127,126,130,128,118,2,115,115,111,119,129,127,128,128,16,16,25,15,18,4,1,4,35,6,7,7,18,20,14,21,21,128,128,126,128,128,127,115,118,8,125,120,87,90,108,96,122,122,129,129,93,91,104,76,97,129,6,121,96,80,89,109,116,113,113,129,129,117,102,91,108,90,128,14,115,108,111,105,90,109,100,100,125,125,94,117,78,124,124,124,29,113,117,115,106,80,100,100,100,120,120,95,81,119,87,103,127,31,109,111,111,87,86,86,114,114,120,120,103,113,125,109,124,121,9,101,86,118,104,100,78,117,117,128,128,128,130,145,127,123,123,0,114,95,93,112,84,105,122,122,128,128,128,130,145,127,123,123,0,114,95,93,112,84,105,122,122


									AREA	myCode, CODE, READWRITE
								

									AREA	myCode, CODE, READWRITE
								
edgeF_row_pointer		DCD			0x20001000
edgeF_colmn_pointer		DCD			0x20001004
	
loop
             B       loop
;**********************************************************************************
			END
