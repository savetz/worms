 \ WORMS? COPYRIGHT (C) 1983 BY DAVID S. MAYNARD                
 ( MASTER   OCT. 9, 1983.  COMMODORE 64 FINAL ) HEX             
  C000 CONSTANT HIMEM HIMEM 1000 - CONSTANT APMAREA             
 0 VARIABLE FNCUR 0 VARIABLE CHCUR                              
  CODE (?CH)                                                    
   C6 LDA, 0= NOT IF, 277 LDA, THEN, PUSH0A JMP, END-CODE       
HERE 0 C, 8 C, 4 C, 2 C, 1 C, 0 C, 84 - CONSTANT CV84-          
 : ?CH BEGIN (?CH)                                              
     DUP IF DUP 84 MAX 89 MIN CV84- + C@                        
      -DUP IF FNCUR ! ELSE DUP CHCUR ! THEN                     
  FLKBD THEN                                                    
 DROP (?CH) 0= UNTIL CHCUR @ ;                                  
 : KEY BEGIN CHCUR @ 0= WHILE ?CH DROP REPEAT                   
 CHCUR @ 0 CHCUR ! ;                                            
 : =KEY SWAP OVER = IF KEY = ELSE DROP 0 THEN ;                 
: ?TERMINAL ?CH DROP FNCUR @ 0 FNCUR ! DUP 8 = IF QUIT THEN ; ;S
 ( WORM GAME START )  HEX                                       
  : <-> CONSTANT ;  : < U< ;                                    
  : VAR VARIABLE ;                                              
 14 <-> #H  16 <-> #V  12 <-> #DH 12  <-> #DV 1 <-> H0 1 <-> V0 
 #DH 2 / <-> STX                                                
 #DV 2 / <-> STY                                                
 0 <-> W# 0 <-> 2*W#                                            
 0 VAR QFLG                                                     
 : STOP ?TERMINAL QFLG @ OR                                     
   DUP QFLG ! ;                                                 
 : CONT STOP 0= ;                                               
 400 <-> C64SCR                                                 
                                                                
                                                                
                                                                
                                                                
 ( WORM GAME CONSTANTS   )                                      
 4 <-> MAX#W #H 1 - <-> #H-1                                    
 #V 1 - <-> #V-1 FF <-> UNDFMOVE                                
 #DH 1 - <-> #DH-1                                              
 #DV 1 - <-> #DV-1                                              
 F1 <-> GETNEW# F2 <-> DOAI# F3 <-> DIE#                        
                                                                
  6 <-> NOMOVE                                                  
 3 VAR DEN ( DENSITY )                                          
 0 VAR #WORMS                                                   
 0 VAR WORMV  1 VAR GPR                                         
 C VAR MXLUM  07 VAR STO                                        
 1 <-> PWDTH                                                    
 0 VAR HCSHI                                                    
 0 VAR LOCK ( INTERLOCK FOR FORTH VBI ! IN UPDMAP )             
            ( LOCKED IN UPDMAP CHECKED IN CDEST2 )              
 ( WORM GAME CONSTANTS CONT. ATARI HDW )                        
 ( FIRST 1 + 800 / 800 * 800 - )                                
 ( 02E5 @ C1F - ) APMAREA \  DUP <-> PMA                        
  200 - DUP <-> WCHSET ( FITS! )                                
  #H #V * - <-> WDATA                                           
( D40A <-> WSYNC ) E4 <-> MAXPOT                                
\  022F <-> DMACTL D01D <-> GRACTL                              
\ D407 <-> PMBASE                                               
\  2C8 <-> COLBK 2C4 <-> COLPF0                                 
\  2C5 <-> COLPF1 2C6 <-> COLPF2                                
\  2C7 <-> COLPF3 2C0 <-> COLPM0                                
\  2C1 <-> COLPM1 2C2 <-> COLPM2                                
\  2C3 <-> COLPM3 26F <-> GPRIOR                                
\  D300 <-> PORTA 0270 <-> POT0                                 
\  D40E <-> NMIEN   ;S                                          
    ;S                                                          
    ( MOVE COUNT DOWN TIMERS )                                  
\ D201 <-> AUDC D200 <-> AUDF                                   
\ D208 <-> AUDCTL 1 VAR VBION                                   
\ HERE                                                          
\  0 , 0 , 0 , 0 ,                                              
\  <-> FRQT ( FREQ. TABLE )                                     
 HERE                                                           
   0 , 0 , 0 , 0 ,                                              
 <-> CDT  ( COUNT DOWN TIMERS )                                 
                                                                
 CODE  DECCDT ( DECREMENT CDT )                                 
  0 #  LDA,                                                     
  CDT ,X CMP,  0= NOT                                           
  IF, CDT ,X DEC, THEN,                                         
  RTS, END-CODE                                                 
   ;S                                                           
   ( MAP VBI TABLES AND CONTS )                                 
\ HERE                                                          
\   7 ,  7 , 7 ,  7 ,                                           
\ <-> CDT  ( MOVE CDTIMERS )                                    
 HERE                                                           
   0 , 0 , 0 , 0 ,                                              
 <-> MAPCDT ( MAP CD TIMER )                                    
 HERE                                                           
  0 , 0 , 0 , 0 ,                                               
 <-> MAPMOVE ( MOVE ARRY )                                      
 CODE DECMAP ( DECREMENT CDT )                                  
  0 # LDA,                                                      
  MAPCDT ,X CMP,  0= NOT                                        
  IF, MAPCDT ,X DEC, THEN,                                      
  RTS, END-CODE                                                 
  ;S                                                            
;S ( INTERRUPT ROUTINES UPDHS)                                  
    0 VAR HPSHAD 4 ALLOT                                        
   CODE UPDHS ( UPD HRZ POS  )                                  
  HPSHAD     LDA, D000 STA,                                     
  HPSHAD 1 + LDA, D001 STA,                                     
  HPSHAD 2 + LDA, D002 STA,                                     
  HPSHAD 3 + LDA, D003 STA,                                     
  RTS,  END-CODE                                                
( : CTABLE <BUILDS !CSP 3 HERE       )                          
(   0 , DOES> 2+ ;                   )                          
( : ENDTAB HERE OVER - 2 - SWAP !    )                          
(   3 ?PAIRS ?CSP ;                  )                          
( : TAB# 2 - @ ;                     )                          
( : WTABLE <BUILDS !CSP 3 HERE       )                          
(  0 , DOES> 2+ ;                    )                          
 ;S                                                             
;S    ( INTERRUPT ROUTINES SETVOL )                             
 HERE                                                           
 A5 C, A7 C, A6 C, A6 C, A5 C, A5 C, A5 C, A4 C,                
 A4 C, A3 C, A2 C, A1 C, A0 C, A0 C, A0 C, A0 C,                
                                                                
 A8 C, A7 C, A6 C, A6 C, A5 C, A4 C, A3 C, A3 C,                
 A4 C, A3 C, A2 C, A1 C, A0 C, A0 C, A0 C, A0 C,                
                                                                
 A6 C, A8 C, A7 C, A6 C, A5 C, A4 C, A3 C, A2 C,                
 A2 C, A1 C, A0 C, A0 C, A0 C, A0 C, A0 C, A0 C,                
                                                                
 A6 C, A8 C, A6 C, A4 C, A3 C, A2 C, A1 C, A0 C,                
 A0 C, A0 C, A0 C, A0 C, A0 C, A0 C, A0 C, A0 C,                
                                                                
 <-> ENV                                                        
;S                                                              
;S (  PMXTAB SETVOL             )                               
 CODE SETVOL ( SETVOLUME )                                      
  CDT ,X LDA, 0=   IF, 0 # LDA,                                 
  AUDC ,X STA, FRQT ,X STA,                                     
  ELSE, STO  LDA, SEC,                                          
  CDT ,X SBC, F # AND, F8 STA, TXA,                             
  .A ASL, .A ASL, .A ASL, CLC, F8 ADC, CLC,                     
  TAY, ENV ,Y LDA,                                              
  ( F # AND, A0 # ORA, )                                        
  AUDC ,X STA,                                                  
  ( FRQT ,X LDA, AUDF ,X STA, )                                 
  ( 0 # LDA, AUDCTL STA, )                                      
  THEN,  CLC, RTS, END-CODE ;S                                  
 CTABLE PMXTAB  3C C, 64 C, 8C C, B4 C,                         
  ENDTAB                                                        
 ;S                                                             
;S  ( INTERRUPT DAMPPOT  )                                      
 0 VAR CPOT 4 ALLOT                                             
 CODE DAMPPOT ( DAMP POT )                                      
  CPOT ,X LDA, SEC,                                             
  POT0 ,X CMP,                                                  
   CS IF, SEC, POT0 ,X SBC,                                     
   .A LSR, .A LSR, SEC,                                         
   FF # EOR, CLC, CPOT ,X ADC,                                  
     CPOT ,X STA, ELSE,                                         
    POT0 ,X LDA, SEC,                                           
    CPOT ,X SBC, .A LSR, .A LSR, .A LSR,                        
    CLC, CPOT ,X ADC,                                           
    CPOT ,X STA,                                                
  THEN,                                                         
  CLC, RTS, END-CODE                                            
 ;S                                                             
;S ( INTERRUPT ROUTINES RPOTS )                                 
 0 VAR WPOT 2 ALLOT 0 VAR PSCAN                                 
( CODE RPOT ( READ POT       )                                  
( ' DAMPPOT JSR,              )                                 
( E6 # LDA, SEC, CPOT ,X SBC, )                                 
( .A LSR, .A LSR, .A LSR,     )                                 
( 7 # AND, 6 # CMP, BCS,    )                                   
( IF, 5 # LDA, THEN,        )                                   
( WPOT ,X STA,  CLC, RTS, END-CODE  )                           
 CODE RPOTS ( READ POTS )                                       
  PSCAN LDA, 0= NOT IF,                                         
  0 # LDX, BEGIN,                                               
  ' DAMPPOT JSR,                                                
  INX, 4 # CPX, 0=                                              
  UNTIL, CLC, THEN, RTS, END-CODE                               
  ;S                                                            
 ( WORM PRIMITIVES .XPOS ETC )                                  
 CODE .XPOS ' C@ JMP, END-CODE                                  
 CODE SET1 1 # LDA, SETUP JSR, RTS, END-CODE                    
 CODE .YPOS ' SET1 JSR, 01 # LDY, N )Y LDA, PUSH0A JMP, END-CODE
 CODE N)Y@ N )Y LDA, PHA, INY, N )Y LDA, PUSH JMP, END-CODE     
 CODE .LOC    ' SET1 JSR, 02 # LDY, ' N)Y@ JMP, END-CODE        
 CODE .#MOVES ' SET1 JSR, 04 # LDY, ' N)Y@ JMP, END-CODE        
 CODE .STATUS ' SET1 JSR, 06 # LDY, ' .YPOS 5 + JMP, END-CODE   
 CODE .WRM#   ' SET1 JSR, 07 # LDY, ' .YPOS 5 + JMP, END-CODE   
 CODE .MOVE 0B # LDA, ' A0+ JSR, BOT LDA, BOT 1+ LDY, INX, INX, 
   ' AY+ JSR, ' C@ JMP, END-CODE                                
 CODE +C! BOT LDA, INX, INX, ' A0+ JSR, ' C! JMP, END-CODE      
 : !XPOS ( WORM XPOS ) SWAP C! ;                                
 : !YPOS SWAP 1+ C! ;                                           
 : !LOC SWAP 2+ ! ;                                             
 : !#MOVES SWAP 4 + ! ;     ;S                                  
 ( WORM PRIMITIVES CONT )                                       
 CODE .HPOS   ' SET1 JSR, 08 # LDY, ' .YPOS 5 + JMP, END-CODE   
                                                                
 : !HPOS ( WORM HPOS )                                          
   SWAP 8 +C! ;                                                 
 CODE .VPOS   ' SET1 JSR, 09 # LDY, ' .YPOS 5 + JMP, END-CODE   
 : !VPOS SWAP 9 +C! ;                                           
 CODE .CRIX   ' SET1 JSR, 0A # LDY, ' .YPOS 5 + JMP, END-CODE   
 : !CRIX SWAP A +C! ;                                           
 : .TMO 4C + @ ;                                                
 : !TMO SWAP 4C + ! ;                                           
                                                                
                                                                
                                                                
                                                                
 ;S                                                             
 ( #BITS WAIT               )                                   
\  : ENDIF [COMPILE] THEN ; IMMEDIATE                           
 : #BITS  (  BYTE -> #BITS )                                    
  0 OVER 1 AND IF 1+ ENDIF                                      
  OVER 2 AND IF 1+ ENDIF                                        
  OVER 4 AND IF 1+ ENDIF                                        
  OVER 8 AND IF 1+ ENDIF                                        
  OVER 10 AND IF 1+ ENDIF                                       
  OVER 20 AND IF 1+ ENDIF SWAP                                  
  DROP ;                                                        
 : WAIT ( TICKS -> )    JIFFYS ;                                
\ DECIMAL 89 92 HEX THRU DECIMAL 138 140 HEX THRU               
\ 0 220 ! FF 22E C! 1 MAX                                       
\ 220 ! BEGIN 22E C@ 0= UNTIL                                   
\ ;                                                             
 ;S                                                             
 ( WORM PRIMITIVES CNT WAITTAB )                                
 : !STATUS SWAP 6 + C! ;                                        
 : !WORM# SWAP 7 + C! ;                                         
 : !MOVE ( WORM N MOVE )                                        
   SWAP ROT B + + C! ;                                          
 : .VAL .LOC C@ ;                                               
 : !VAL ( W VAL ) 2DUP SWAP .LOC C! 64!VAL ;                    
 : [.VAL] .VAL 3F AND ; \ MASK OFF COLOR                        
( : CWAITTAB <BUILDS         )                                  
(  2 C, 2 C, 2 C, 2 C,  2 C, )                                  
(   2 C,  2 C,  DOES> ;      )                                  
( CWAITTAB WAITTAB           )                                  
                                                                
                                                                
                                                                
 ;S                                                             
 ( WORM PRIMITIVES CONT )                                       
 : .KIND 4B + C@ ;                                              
 : !KIND SWAP  4B + C! ;                                        
 : .SCORE 4C + @ ;                                              
 : !SCORE SWAP 4C + ! ;                                         
 : .NAME 4E + ;                                                 
\ : 2* DUP + ;      EAFORTH 2* IS FASTER                        
 : 3* DUP 2* + ;                                                
 : 4* 2* 2* ;                                                   
 : 8* 4* 2* ; ( : 28* DUP 4* + 8* ; )                           
 D3 <-> COLCRS  D6 <-> ROWCRS                                   
: X! COLCRS C! ; : X@ COLCRS @ ;                                
: Y! DUP ROWCRS C! 28* C64SCR + D1 ! ; : Y@ ROWCRS C@ ;         
: XY!  Y! X! ;   : ^ 1 0 XY! ;                                  
: P" 0 14    XY! 1 286 C! ( WHITE ) ; ;S                        
 ;S                                                             
 ( WORM TABLE BUILDERS )                                        
 HERE                                                           
  3 C, 4 C, 5 C, 0 C, 1 C, 2 C,                                 
   <-> NMOVE                                                    
 HERE                                                           
  1 , 0 , -1 , -1 , -1 , 0 ,                                    
   <-> XINCE                                                    
 HERE                                                           
  1 , 1 , 0 , -1 , 0 , 1 ,                                      
  <-> XINCO                                                     
 HERE                                                           
  0 , 1 , 1 , 0 , -1 , -1 ,                                     
 <-> YINC                                                       
 ;S                                                             
                                                                
                                                                
( ZERO PAGE DEFS )                                              
0A <-> ZF 0C <-> ZT 0E <-> ZM                                   
04 <-> ZA 06 <-> ZB 08 <-> ZC                                   
;S                                                              
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
HERE 0 C,  0 C, 00 C,  0 C,  0 C, 00 C,                         
  0 C,  0 C, 00 C,  3 C, C0 C, 00 C,                            
  3 C, C0 C, 00 C,  0 C,  0 C, 00 C,                            
  0 C,  0 C, 00 C,  0 C,  0 C, 00 C,                            
  0 C,  0 C, 00 C,  0 C,  0 C, 00 C,                            
  F C, F0 C, 00 C,  F C, F0 C, 00 C,                            
  F C, F0 C, 00 C,  F C, F0 C, 00 C,                            
  0 C,  0 C, 00 C,  0 C,  0 C, 00 C,                            
  0 C,  0 C, 00 C, 3F C, FC C, 00 C,                            
 3F C, FC C, 00 C, 3F C, FC C, 00 C,                            
 3F C, FC C, 00 C, 3F C, FC C, 00 C,                            
 3F C, FC C, 00 C,  0 C,  0 C, 00 C,                            
 FF C, FF C, 00 C, FF C, FF C, 00 C,                            
 FF C, FF C, 00 C, FF C, FF C, 00 C,                            
 FF C, FF C, 00 C, FF C, FF C, 00 C,                            
 FF C, FF C, 00 C, FF C, FF C, 00 C,                            
 FF C, FF C, 00 C, FF C, FF C, 00 C,                            
 FF C, FF C, 00 C, FC C, 3F C, 00 C,                            
 FC C, 3F C, 00 C, FF C, FF C, 00 C,                            
 FF C, FF C, 00 C, FF C, FF C, 00 C,                            
 FF C, FF C, 00 C, FF C, FF C, 00 C,                            
 F0 C,  F C, 00 C, F0 C,  F C, 00 C,                            
 F0 C,  F C, 00 C, F0 C,  F C, 00 C,                            
 FF C, FF C, 00 C, FF C, FF C, 00 C,                            
 FF C, FF C, 00 C, C0 C,  3 C, 00 C,                            
 C0 C,  3 C, 00 C, C0 C,  3 C, 00 C,                            
 C0 C,  3 C, 00 C, C0 C,  3 C, 00 C,                            
 C0 C,  3 C, 00 C, FF C, FF C, 00 C,                            
  0 C,  0 C, 00 C,  0 C,  0 C, 00 C,                            
  0 C,  0 C, 00 C,  0 C,  0 C, 00 C,                            
  0 C,  0 C, 00 C,  0 C,  0 C, 00 C,                            
  0 C,  0 C, 00 C,  0 C,  0 C, 00 C,  <-> MAPMASK               
( ANONB CR*24 )                                                 
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
 0 VAR ANONB ( ANIMATION ON )                                   
 : ANON 1 ANONB ! ;                                             
HERE CRMAP   00 + , CRMAP   18 + , CRMAP   30 + ,               
     CRMAP   48 + , CRMAP   60 + , CRMAP   78 + ,               
CRMAP   90 + , CRMAP A8 + ,                                     
     <-> CR*24                                                  
 ( INTERRUPT CRMAP ROUTINES )                                   
                                                                
                                                                
                                                                
 CRMAP VAR CRMADDR                                              
 CODE CSOURCE ( FIND ZF  ADDR )                                 
  MAPMOVE ,X LDA,                                               
  .A ASL, TAY,                                                  
  CR*24 ,Y  LDA, ZF STA, INY,                                   
  CR*24 ,Y  LDA, ZF 1+ STA, INY,                                
  RTS, END-CODE                                                 
 : GCMD WCHSET 3 + @ IF 0 WCHSET 3 + C! ELSE                    
                        8 WCHSET 3 + C! THEN ;                  
HERE MAPMASK 00 + , MAPMASK 18 + , MAPMASK 30 + ,               
     MAPMASK 48 + , MAPMASK 60 + , MAPMASK 78 + ,               
MAPMASK 90 + , MAPMASK A8 + , <-> MM*24 ;S                      
 ( CDEST CCANDM VBISMASK VBIMASKTAB )                           
 MAPMASK VAR MMADDR 0 VAR VBISMASK  1 VAR VBIMAPMASK            
        HERE  ( PAIRS: MASKVAL , STOVAL )                       
  0 C,  2 C, 0 C, 04 C, 0 C, 06 C,                              
  0 C,  8 C, 0 C, 0C C, 0 C, 10 C,                              
  1 C, 10 C, 3 C,  C C, 7 C, 10 C, F C, 10 C, <-> VBIMASKT      
 CODE CDEST2                                                    
  SPMEM  ,X LDA, ZT STA,                                        
  SPMEM  1+ ,X LDA, ZT 1+ STA,                                  
  RTS,  END-CODE                                                
 CODE CANDM ( AND MASK ADDR )                                   
  8 # LDA, SEC, MAPCDT ,X SBC,                                  
  .A ASL, TAY,                                                  
  MM*24 ,Y  LDA, ZM STA, INY,                                   
  MM*24 ,Y  LDA, ZM 1+ STA, INY,                                
  CLC, RTS, END-CODE ;S                                         
 ( INTRPT CODE CHAR ANIMATION  )                                
 CRMAP 7 8 * + <-> SRCBASE 14 <-> CLKLO                         
 WCHSET 3F 8 * + <-> DESTBAS                                    
 : ANOFF 0 ANONB ! ; ;S  8 0 DO                                 
   SRCBASE I + C@ DESTBAS I +                                   
   C! LOOP ;                                                    
;S CODE ANIM                                                    
  ANONB LDA, 0= NOT IF, CLKLO LDA, 3 # AND, 0= IF,              
  CLKLO LDA,  1C # AND,                                         
  .A ASL,  TAY,                                                 
  7 # LDX, BEGIN,                                               
  SRCBASE ,X LDA,                                               
  MAPMASK ,Y AND, DESTBAS ,X                                    
  STA, DEX, INY, FF # CPX,                                      
  0= UNTIL,  THEN,                                              
  THEN, CLC, RTS, END-CODE  ;S                                  
( WRTP )                                                        
 CODE WRTP ( WRITE PLAYERS ) LOCK LDA, 0= IF,                   
  17 # LDY, BEGIN,                                              
  ZF )Y LDA, ZM )Y AND,                                         
  ZT )Y STA,                                                    
  DEY, FF #  CPY, 0=                                            
  UNTIL, THEN,  CLC, RTS, END-CODE ;S                           
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
      ( INTERRUPT ROUTINES VBI     )                            
 CODE VBI ( VERTICAL BLANK )                                    
( CLKLO LDA, VBISMASK AND, 0= IF, ' VBISND JSR, THEN, )         
( ' ANIM JSR,  )                                                
( ' RPOTS JSR,  )  CLKLO LDA,                                   
  VBIMAPMASK AND, 0=   IF,                                      
  0 # LDX, BEGIN,                                               
  MAPCDT ,X LDA, 0= NOT IF,                                     
  ' CSOURCE JSR, ' CDEST2 JSR,                                  
  ' CANDM JSR, ' WRTP JSR,                                      
  ' DECMAP JSR, THEN,                                           
  ' DECCDT JSR, INX, INX,                                       
  8 # CPX, 0=   UNTIL,                                          
  THEN,  ( ' UPDHS JSR, )  CLC,                                 
  RTS,      END-CODE ( SYS VBI RETURN)                          
 ;S                                                             
 ( ?SPEED  ?SPEED2 CNFRM     )                                  
: RANGE 1+ >R OVER R> < >R 1- > R> AND ;                        
 : ACMD ANONB @ IF ANOFF ELSE ANON  THEN ;                      
                                                                
                                                                
 : (?SPEED2)                                                    
    KEY 39 SWAP - 2*  VBIMASKT + DUP C@ DUP  VBISMASK C!        
    1 MAX VBIMAPMASK C!  1+ C@ STO ! ;                          
                                                                
                                                                
 : ?SPEED                                                       
 ?CH -DUP IF                                                    
     30 39 RANGE                                                
    IF (?SPEED2) THEN THEN ;                                    
 : PROMPT2 P" C64SCR 14 28* + 28 20 FILL ;                      
 : CNFRM KEY D = DUP IF ELSE PROMPT2 THEN ; ;S                  
 ( TRIG AND FREQ TABLES CRMAP .GWADR MOVE8 )                    
 HERE                                                           
  04 C, 08 C, 40 C, 80 C,                                       
  <-> TRIGMASK                                                  
 : ?START \ 0 4 0 DO I TRIGMASK +                               
 \  C@ PORTA C@ AND 0= OR LOOP                                  
   ?TERMINAL 7 AND ( OR ) ;                                     
 : ?CLEAR BEGIN ?START 0= UNTIL                                 
   ;                                                            
 : MOVE18 ( FROM TO )                                           
       18 CMOVE ;                                               
                                                                
 ;S                                                             
                                                                
                                                                
                                                                
 ( UPDCRLOC            )                                        
                                                                
 : UPDCRLOC  ( WORM )                                           
  DUP .XPOS  8*  4 +                                            
   OVER                                                         
  .YPOS 1 AND IF 4 + ENDIF                                      
  2DUP !HPOS                                                    
  (  W# HPSHAD +  C! ) DROP                                     
  DUP .YPOS 8*  2E + !VPOS ;                                    
 ;S                                                             
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
    ( UPDMAP UPDMV ZRMAP ZMAP )                                 
 : ZRMAP ( 2*W# -> )                                            
   0 OVER MAPCDT + !                                            
   SPMEM + @ 18 0  FILL ;                                       
                                                                
 : ZMAP ( W )  DROP 2*W# ZRMAP ;                                
 : UPDMAP ( W ) 2*W#  ZRMAP                                     
   DUP .VPOS SWAP .HPOS 2* W#                                   
   YXSPRITE  7 MAPCDT 2*W# + !                                  
   STO C@ CDT 2*W# + ! ;                                        
                                                                
 : UPDMV ( W )                                                  
  2*W#                                                          
  ( DUP MAPCDT + 4 SWAP ! )                                     
  MAPMOVE +                                                     
  SWAP .CRIX SWAP ! ;  ;S                                       
 ( WORM BUILDERS CWORM GWORM )                                  
 HERE 0 , 0 , 0 , 0 , <-> WORMS                                 
 : GWORM ( N -> WORM ) DUP + WORMS + @ ;                        
 : INITWORM ( ADDR -->  )                                       
  >R R #WORMS @ DUP + WORMS + !                                 
  R STX !XPOS R STY !YPOS R 0 !LOC R 0 !#MOVES                  
  R 1 !STATUS R #WORMS @ !WORM# R 0 !HPOS R 0 !VPOS  R 6 !CRIX  
  R 40 0 DO DUP I GETNEW# !MOVE LOOP DROP                       
  R 0 !KIND R 0 !SCORE R> .NAME >R                              
  53 R C! 41 R 1+ C! 4D R 2+ C! 45 R 3 + C!                     
  R 8 4 DO DUP I + 20 SWAP C! LOOP DROP                         
  R> DROP 1 #WORMS +! ;                                         
                                                                
                                                                
                                                                
                                                                
 ( KILLW )     HEX 97 97 THRU                                   
  : KILLW ( WORM -> ) DROP TKILLW ; ;S                          
  ( 0 VBION ! )  A !CRIX                                        
  XSND \ .WRM# COLPF0 +                                         
  FF 50 DO   A 0 DO NOOP LOOP                                   
\  120 I - 10 / OVER C@ F0 AND OR                               
\   OVER C!                                                     
  I I 50 - 0 MAX DO                                             
    5 F FFFF I >< - ROVR SOUND 8 +LOOP                          
  STOP IF LEAVE ENDIF                                           
  8 +LOOP                                                       
\  5 OVER C@ F0 AND OR SWAP C!                                  
  XSND ( 1 VBION ! ) ;                                          
                                                                
                                                                
  ;S                                                            
  ( UPDSTATUS )                                                 
 : UPDSTATUS ( WORM )                                           
  DUP .STATUS                                                   
  IF DUP [.VAL] 3F =                                            
  IF DUP KILLW 0 !STATUS                                        
  ELSE 1 !STATUS ENDIF                                          
  ELSE DROP  ENDIF ;                                            
                                                                
                                                                
                                                                
                                                                
                                                                
 ;S                                                             
                                                                
                                                                
                                                                
 ( UPDLOC UPDCOL   TRIGCLR )                                    
 : UPDLOC  ( WORM )                                             
   DUP .XPOS OVER .YPOS #H * +                                  
   WDATA + !LOC ;                                               
 CODE 40* BOT LDA, CLC, .A ROR, .A ROR, .A ROR,                 
          BOT STA, CLC, NEXT JMP, END-CODE                      
 : UPDCOL ( WORM )                                              
   DUP [.VAL] W# 40* + !VAL ;                                   
 : INITAUTO ( WORM  )                                           
  40 0 DO                                                       
   DUP I DOAI#  !MOVE LOOP                                      
  DUP 3E 0 !MOVE ( FORCED )                                     
  DUP 3D 1 !MOVE DUP 3B 2 !MOVE DUP 37 3 !MOVE                  
  DUP 2F 4 !MOVE DUP 3F DIE# !MOVE                              
      1F 5 !MOVE ;  ;S                                          
                                                                
 ( WORM INITIALIZATION )                                        
 : INITPOS ( WORM )                                             
   DUP STX !XPOS DUP STY !YPOS                                  
   DUP NOMOVE !CRIX DUP UPDLOC                                  
   DUP UPDCRLOC (  DUP UPDMV )                                  
   ( DUP UPDMAP )                                               
   DUP 258 !TMO UPDSTATUS ;                                     
 : INITMOVES ( WORM  )                                          
  40 0 DO                                                       
   DUP I GETNEW#  !MOVE LOOP                                    
  DUP 3E 0 !MOVE ( FORCED )                                     
  DUP 3D 1 !MOVE                                                
  DUP 3B 2 !MOVE                                                
  DUP 37 3 !MOVE                                                
  DUP 2F 4 !MOVE DUP 3F DIE# !MOVE                              
      1F 5 !MOVE ;  ;S                                          
 C64SCR 14 28 * + <-> SCRMEM                                    
;S ( DLH SAVELIST BUILDER ) HEX                                 
 230 CONSTANT DLH                                               
 D404 CONSTANT HSCROL                                           
 DLH @ CONSTANT SAVDLH                                          
 : RESTORE SAVDLH DLH !                                         
  22 DMACTL C! 0 GRACTL C!                                      
  E0 2F4 C! 28 COLPF0 C! 0 COLPF1 C!                            
  C COLPF2 C! 46 COLPF3 C!                                      
\  5 0 DO ( CLEAR PM REGISTRS )                                 
\  0 D00D I + C! LOOP ;                                         
  ;                                                             
                                                                
                                                                
                                                                
 ;S                                                             
;S ( DISPLAY LIST BUILDER ) HEX                                 
 70 ALLOT    14 , HERE HERE 70 C, 70 C,                         
  56 C, WDATA  0 #H * + , 46 C, WDATA  1 #H * + ,               
  56 C, WDATA  2 #H * + , 46 C, WDATA  3 #H * + ,               
  56 C, WDATA  4 #H * + , 46 C, WDATA  5 #H * + ,               
  56 C, WDATA  6 #H * + , 46 C, WDATA  7 #H * + ,               
  56 C, WDATA  8 #H * + , 46 C, WDATA  9 #H * + ,               
  56 C, WDATA  A #H * + , 46 C, WDATA  B #H * + ,               
  56 C, WDATA  C #H * + , 46 C, WDATA  D #H * + ,               
  56 C, WDATA  E #H * + , 46 C, WDATA  F #H * + ,               
  56 C, WDATA 10 #H * + , 46 C, WDATA 11 #H * + ,               
  56 C, WDATA 12 #H * + , 46 C, WDATA 13 #H * + ,               
  80 C, 60 C, 46 C, SCRMEM 50 + , 00 C, 40 C,                   
  46 C, SCRMEM , 00 C, 00 C, 46 C, SCRMEM 28 + ,                
  41 C, , <-> WDL                                               
 ;S                                                             
 ( WDL SETHSCRL CHARACTER SET DEF. )                            
 HERE                                                           
 1 C, 2 C, 4 C, 8 C, 10 C, 20 C,                                
 <-> TMASK                                                      
;S                                                              
(  HEX #DV 2+ CDL WDL                                           
( : SETHSCRL                    )                               
(  DUP 2 - @  1 DO              )                               
(  DUP I 3 * + 1 - DUP C@ 10 OR )                               
(  SWAP C! 2 +LOOP DROP         )                               
(  C HSCROL C! ;                )                               
(  WDL SETHSCRL  )                                              
  2F4 <-> CHBAS D409 <-> CHBASE                                 
 WCHSET 100 / FF AND <-> WCHSETH                                
 WCHSETH VAR CHNOW C VAR SCRLVAL                                
                                                                
 ( CHSET BUILDING TVEC DEFS )                                   
 HERE                                                           
 00 C, 00 C, 00 C, 0F C, ( 0 )                                  
 00 C, 00 C, 00 C, 00 C,                                        
 00 C, 00 C, 00 C, 00 C, ( 1 )                                  
 08 C, 08 C, 04 C, 04 C,                                        
 00 C, 00 C, 00 C, 00 C, ( 2 )                                  
 10 C, 10 C, 20 C, 20 C,                                        
 00 C, 00 C, 00 C, F0 C, ( 3 )                                  
 00 C, 00 C, 00 C, 00 C,                                        
 20 C, 20 C, 10 C, 10 C, ( 4 )                                  
 00 C, 00 C, 00 C, 00 C,                                        
 04 C, 04 C, 08 C, 08 C, ( 5 )                                  
 00 C, 00 C, 00 C, 00 C,                                        
 <-> TVEC                                                       
 ;S                                                             
 ( CHSET BUILDING )                                             
 : DALLY FF 0 DO I DROP LOOP ;                                  
 37A7 VARIABLE SEED                                             
 \ : RAND SEED @ FB * 3D + DUP SEED ! 4* MSB ;                  
 : RAND D012 C@ SEED +! SEED C@ ;                               
 ;S  : ORCHARVEC ( CHARADDR VECADR )                            
  8 0 DO 2DUP I + C@                                            
   OVER I + C@ OR SWAP                                          
   I + C! LOOP DROP DROP ;                                      
 : ORCHAR ( CHB CINDX -> CHB )                                  
  6 0 DO 2DUP TMASK I + C@                                      
    AND  IF                                                     
   OVER 8 * +                                                   
   TVEC I 8 * + ORCHARVEC                                       
   ELSE DROP ENDIF LOOP                                         
   DROP ;                                                       
 ( CHSET BUILD WDISPLAY  )                                      
 : HI MSB ; : LO LSB ;                                          
 : CLEARF WDATA #H #V * 0 FILL                                  
  CLEAR64PF ;                                                   
 ;S : BUILDCHSET ( CHBASE ) DUP 200 0 FILL                      
 40 0 DO                                                        
  I ORCHAR   LOOP  DROP  8 WCHSET 3 + C! ;                      
 : IWDISPLAY ( 2A ) 22 DMACTL C!                                
 ( 02 ) 00 GRACTL C!   WDL DLH !                                
 C0 NMIEN C! \  GPR C@ GPRIOR C!                                
 \ PMA HI PMBASE C!                                             
 \ C 0 DO 0 I D000 + C! LOOP  ( PLAYER H POS )                  
  4 0 DO ( PWDTH D008 I + C! ) ( PLAYER WIDTHS )                
 COLPFT I + C@ COLPF0 I + C!                                    
 \ COLPMT I + C@ COLPM0 I + C!                                  
 LOOP WCHSETH CHBAS C! ;  ;S                                    
 ( [X] [Y] SCALE )                                              
 : [X]   ( X -> X  CLIPS )                                      
 DUP #DH-1 H0 +  > IF #DH - ELSE                                
 DUP H0 < IF #DH + ENDIF ENDIF ;                                
 : [Y]  ( Y -> Y CLIPS )                                        
 DUP #DV-1 V0 + > IF #DV - ELSE                                 
    DUP V0 < IF #DV + ENDIF                                     
  ENDIF ;                                                       
 HERE ( C D F G A ... )                                         
 ( NEED 18 ONLY )                                               
  892 ,  99F ,  B71 ,  CD8 ,  E6B ,                             
  1125 , 133F , 16E3 , 19B0 , 1CD6 ,                            
  224B , 267E , 2DC6 , 3361 , 39AC ,                            
  4495 , 4CFC , 5B8C , 66C2 , 7358 ,                            
  892B , 99F7 , B719 , CD85 , E6B0 ,                            
   <-> SCALE  ;S                                                
 ( SOUND BSND MSND PWORM )                                      
\ : BSND ( WORM --> ) DROP  ;                                   
\ 2*W# DUP FRQT + FF SWAP !                                     
\ CDT + 10 SWAP ! ;                                             
\ C VAR FV 4 VAR BV ( FOR,BK VOL)                               
                                                                
                                                                
                                                                
 : MSND ( W M )                                                 
  SWAP .STATUS                                                  
  IF W# 3* + 2* SCALE + @ >R WV SR AR R> ROVR C64SOUND          
  ELSE DROP THEN ;                                              
;S  : PWORM 1 VBION !                                           
  3F 0 DO DUP DUP I                                             
 .MOVE MSND 8 WAIT  LOOP                                        
 DROP XSND 0 VBION ! ;  ;S                                      
 ( [MOVE] DECTMO GXINC XY! X! Y! ^ )                            
 : [MOVE] ( WORM M -> WORM M )                                  
  DUP 5 > IF (  OVER BSND ) ELSE                                
   OVER .VAL OVER TMASK + C@                                    
   AND IF DROP GETNEW#                                          
   ENDIF ENDIF ;                                                
 : DECTMO ( W -> )                                              
  DUP .TMO 13 - 50 MAX !TMO ;                                   
 : GXINC ( WORM -> XINCV )                                      
  .YPOS 1 AND IF XINCO ELSE                                     
 XINCE ENDIF ;                                                  
                                                                
                                                                
                                                                
 ;S                                                             
                                                                
 (  ?SPACE ?CR MOVEOK DETRACT TIMEOUT )                         
: ?SPACE ?CH 20 =KEY ;                                          
                                                                
: ?NOTSP ?CH DUP 0= SWAP 20 = OR                                
           IF 0 ELSE KEY THEN ;                                 
: ?CR ?CH 0D =KEY ;                                             
 : MOVEOK ( WORM M -> FLAG   )                                  
   TMASK + C@ SWAP .VAL AND 0= ;                                
 : DETRACT ( 0 4D C! ) ;                                        
 : PAUSEP P" ." PAUSED: HIT RUN    " ;                          
 : ?PAUSE  ?CH 03 = IF  PAUSEP KEY DROP                         
     BEGIN ?CH IF 0 ELSE KEY 03 = THEN                          
     ?TERMINAL OR UNTIL PROMPT2 THEN ;                          
 : TIMEOUT ( MOVE --> MOVE F )  ?PAUSE                          
 ( W# ) ( DUP BLINK ) ?SPEED                                    
 ( TRIGMASK + C@ PORTA C@ AND 0= ) ?NOTSP ( OR) STOP OR ; ;S    
 ( SETTIME NXTMOVE CLRKBD   )                                   
 0 VAR KEYPOT                                                   
 : SETTIME ( WORM ) DROP ;                                      
\  0 KEYPOT !                                                   
\ 0 21E ! FF 22C C! DUP .TMO                                    
\ 21E !  DROP 2*W# >R                                           
\ R R + SCALE + C@                                              
\ R FRQT + !                                                    
\ STO @ R> CDT + ! ;                                            
 : NXTMOVE ( MOVE -> MOVE )                                     
  NMOVE + C@ 5 MIN 0 MAX ;                                      
 : CLRKBD BEGIN ?CH WHILE KEY DROP REPEAT ;                     
                                                                
                                                                
                                                                
   ;S                                                           
;S : =OR ROT >R R = SWAP R> = OR ; ;S  ( CASES   )              
 : CASES ?COMP CSP @ !CSP 4 ; IMMEDIATE                         
 : >OF 4 ?PAIRS COMPILE OVER COMPILE < COMPILE 0BRANCH          
       HERE 0 , COMPILE DROP 5 ; IMMEDIATE                      
 : <OF 4 ?PAIRS COMPILE OVER COMPILE > COMPILE 0BRANCH          
       HERE 0 , COMPILE DROP 5 ; IMMEDIATE                      
 : =OF 4 ?PAIRS COMPILE OVER COMPILE = COMPILE 0BRANCH          
       HERE 0 , COMPILE DROP 5 ; IMMEDIATE                      
 : INOF 4 ?PAIRS COMPILE ROT COMPILE >R COMPILE R COMPILE >=    
       COMPILE SWAP COMPILE R COMPILE <= COMPILE AND            
       COMPILE R> COMPILE SWAP COMPILE 0BRANCH                  
       HERE 0 , COMPILE DROP 5 ; IMMEDIATE                      
 : ENDOF 5 ?PAIRS COMPILE BRANCH HERE 0 , SWAP 2 [COMPILE] THEN 
       4 ; IMMEDIATE                                            
 : ENDCASES 4 ?PAIRS COMPILE DROP BEGIN SP@ CSP @ = 0= WHILE 2  
    [COMPILE] THEN REPEAT CSP ! ; IMMEDIATE ;S                  
 ( FIXL0 FIXL1 )                                                
\ : FIXL2 ( W# ) MAPCLR + C@ COLRMEM 14 28* + 28 ROT FILL ;     
                                                                
;S                                                              
 : FIXL0  ( W# )    ( SCORE LINE )                              
   DUP 40*  ( W# MASK )                                         
   SWAP 5 * SCRMEM +                                            
   DUP 5 + SWAP                                                 
   DO I C@ 3F AND OVER OR I C!                                  
   LOOP DROP ;                                                  
 : FIXL1 ( W# )     ( NAME LINE )                               
   DUP 40* ( W# MASK )                                          
   SWAP 5 * SCRMEM 28 + +                                       
   DUP 5 + SWAP                                                 
   DO I C@ 3F AND OVER OR I C!                                  
   LOOP DROP ;  ;S                                              
 ( ADDONE )                                                     
                                                                
\ : ADDONE ( ADDR )                                             
\  >R BEGIN                                                     
\   R C@ 3F AND DUP 2F 38 RANGE                                 
\     IF 1+ R C! 1 ELSE                                         
\  DUP  39 = IF DROP 30 R C! R> 1- >R 0 ELSE                    
\               DROP 31 R C! 1 THEN THEN                        
\    UNTIL R> DROP ;                                            
: PUTSCORE ( N A )                                              
>R BEGIN 0A /MOD SWAP                                           
   2* A0 + DUP R 1- C! 1+ R C!                                  
   R> 2- >R DUP 0= UNTIL R> 2DROP ;                             
;S                                                              
                                                                
                                                                
 (  KEYPOT GKEYPOT X! Y! XY! )                                  
 0 VARIABLE TBUF 10 ALLOT 0 VAR CCNT                            
 HEX 0 VAR SEL ( SELECTED WORM )                                
: HIL2 ( SEL @ FIXL2 ) ; : FIXL2 DROP ;                         
: GKEYPOT (  -> N )                                             
   ?SPACE KEYPOT +! KEYPOT @ ;                                  
: PROMPTM P" ." PADDLE/SPACE&RETURN"  ;                         
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
 ;S                                                             
 ( LEGAL CHKMOVES      )                                        
 0 VAR LEGAL 4 ALLOT                                            
 0 VAR #LEGAL                                                   
 HERE  1 C, 2 C, 4 C,                                           
 8 C, 10 C, 20 C, <-> MMASK                                     
                                                                
 : CHKMOVES ( WORM )                                            
  0 #LEGAL !                                                    
  [.VAL] 6 0 DO                                                 
  DUP I MMASK + C@ AND 0= IF                                    
  #LEGAL @ LEGAL + I SWAP C!                                    
  1 #LEGAL +! ENDIF                                             
  LOOP DROP ;                                                   
                                                                
                                                                
 ;S                                                             
 ( GPOT ) HEX                                                   
  : GPOT ( WORM --> INDX ) DROP GKEYPOT                         
\   .WRM#  DUP POT0 + C@ MAXPOT = IF DROP GKEYPOT ELSE          
\     CPOT + C@ E6 SWAP -                                       
\     E6 #LEGAL @ 2 * /                                         
\     /  GKEYPOT + THEN                                         
    #LEGAL @ 1 MAX MOD                                          
    LEGAL + C@  0 MAX 5 MIN ;                                   
  : TRIGCLR ( MV WORM - MOVE )  DROP JSCLR ; ;S                 
\    >R W# TRIGMASK + C@                                        
\   BEGIN DUP PORTA C@ AND                                      
\   R DUP GPOT !CRIX (  R UPDMV )                               
\   W# DUP + MAPCDT + >R                                        
\   R C@ 0= IF 7 R C! ENDIF R>                                  
\   DROP UNTIL                                                  
\   DROP DROP R> .CRIX ; ;S                                     
 ( PEEKMAP HOME? SAMEDIR? )                                     
 : XY->V #H * + WDATA + C@ ;                                    
 : PEEKMAP ( W M -> COUNT ) 2DUP                                
  SWAP >R 2 * R GXINC + @ R>                                    
   .XPOS + [X]   ROT ROT                                        
  SWAP >R 2 * YINC + @ R> .YPOS                                 
   + [Y]                                                        
  XY->V #BITS ;                                                 
 : HOME? ( W M --> F )                                          
  DROP DROP 0 ( NOT IMPL. ) ;                                   
 : SAMEDIR? ( W M --> NO/?/YES )                                
  SWAP [.VAL] DUP #BITS 1 = IF                                  
   SWAP NXTMOVE TMASK + C@ = 2 *                                
  ELSE SWAP NXTMOVE TMASK + C@                                  
   AND IF 1 ELSE 0 ENDIF ENDIF ;                                
 ;S                                                             
 ( EVAL )                                                       
 HERE 5 C, 7 C, 7 C,                                            
 9 C, B C, 2 C, <-> SIX                                         
 HERE 5 C, 7 C, 7 C,                                            
 9 C, 5 C, 2 C, <-> VALFN                                       
 : EVAL ( W M --> )                                             
 OVER .VAL OVER TMASK + C@                                      
 AND IF SIX + 0 SWAP C! DROP                                    
 ELSE ( W LEGALMOVE )                                           
  2DUP PEEKMAP VALFN + C@ >R                                    
  2DUP HOME? R> + >R                                            
  SWAP OVER SAMEDIR? R> SWAP -                                  
  SWAP SIX + C!                                                 
  ENDIF ;                                                       
                                                                
 ;S                                                             
( DOAI )                                                        
 : CK6 ( W --> )                                                
 6 0 DO DUP I EVAL LOOP DROP ;                                  
 0 VAR TEMP                                                     
 : 6T SIX + C@ TEMP @ ;                                         
 : PICKBEST ( --> M ) 0 TEMP !                                  
  6 0 DO                                                        
   I 6T MAX TEMP ! LOOP  0                                      
  6 0 DO I 6T = IF 1+ ENDIF                                     
   LOOP   RAND SWAP MOD 1+                                      
  6 0 DO I 6T = IF 1 - -DUP                                     
   0= IF I LEAVE ENDIF ENDIF                                    
   LOOP ;                                                       
 : DOAI ( W --> M )                                             
  ( 300 0 DO I DROP LOOP )                                      
  CK6 PICKBEST ;   ;S                                           
   ( DOS EMULATION )                                            
  1 VAR CMDOK : CHKST ;                                         
\ 80 CONSTANT SECSZ                                             
\ 0 VARIABLE FBUF SECSZ ALLOT                                   
: IOBUF PAD ;                                                   
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
 : VDOSDISK 1 ;                                                 
 ;S                                                             
 : GETNEWMOVE DUP 7 !CRIX CLRKBD                                
  XSND DUP SETTIME 2*W#                                         
 >R DUP UPDCRLOC                                                
 DUP CHKMOVES  (  1 PSCAN C!  )                                 
    DUP UPDMAP                                                  
 6 BEGIN DROP DUP GPOT                                          
 OVER .CRIX OVER = IF ELSE                                      
 2DUP !CRIX                                                     
   OVER UPDMV ENDIF R MAPCDT +                                  
 C@ 0= IF 7 MAPCDT R + ! ENDIF                                  
 TIMEOUT UNTIL OVER TRIGCLR                                     
 ( 0 COLBK C! DETRACT ) [MOVE]                                  
  SWAP OVER SWAP                                                
 DUP [.VAL] ROT !MOVE  ( 0 PSCAN C! )                           
 ( 1 VBION ! ) R> DROP ; ;S                                     
                                                                
 ( HILITE DELITE          )                                     
 HERE  38 C, 60 C, 88 C, B0 C, <-> MHP                          
 : MVHILTE ( W# )                                               
   DUP 0A * DM 28 17 * + + 0A F7 FILL                           
  MAPCLR OVER + C@ SWAP D027 + C! ;                             
 : MVDELITE (  )                                                
  DM 28 17 * + 28 20 FILL W# SPCOLR + C@ W# D027 + C! ;         
 : HILITE ( WORM )  DROP ;                                      
\  WRM# DUP COLPF0 + SWAP                                       
\  COLPFT + C@  F0 AND 8 OR SWAP C! ;                           
 : DELITE (    )    ;                                           
\  4 0 DO COLPFT I + C@ F0                                      
\ AND 4 OR COLPF0 I +                                           
\ C! LOOP ;                                                     
 : XLITE ( ALL COLORS OFF )     ; ;S                            
\ 4 0 DO 0 COLPF0 I + C! LOOP ; ;S                              
 (  GETMOVE   )                                                 
 : GETMOVE ( WORM -> MOVE )                                     
   DUP [.VAL] OVER SWAP .MOVE                                   
   DUP                                                          
   6 <  IF ELSE DUP ( W M )                                     
   GETNEW# = IF DROP W#  MVHILTE                                
          DUP GETNEWMOVE MVDELITE DELITE ELSE DUP               
   DOAI#   = IF DROP DUP DOAI                                   
           >R DUP DUP [.VAL] R !MOVE R> ELSE DUP                
   DIE#    = IF DROP DUP UPDSTATUS UNDFMOVE ELSE                
  (  ." ????? " S.  )                                           
   THEN THEN THEN THEN SWAP DROP ;                              
 ;S                                                             
                                                                
                                                                
                                                                
 ( INC#MOVES NXTMOVE UPDXPOS )                                  
 : INC#MOVES ( WORM )                                           
  DUP 4 + @ 1+                                                  
          SWAP 4 + ! ;                                          
 : UPDXPOS ( WORM MOVE )                                        
  SWAP >R 2* R GXINC + @ R                                      
  .XPOS + [X] R> SWAP !XPOS ;                                   
 : UPDYPOS ( WORM MOVE )                                        
  SWAP >R 2* YINC + @ R .YPOS                                   
  + [Y] R> SWAP !YPOS ;                                         
 0 VAR WSCR 6 ALLOT                                             
 : UPDSCR ( WORM )                                              
  .WRM# DUP 0A * SCRMEM + 2E +                                  
>R ( ADDONE ) ( DUP  FIXL0 ) DUP +                              
   WSCR + DUP >R 1 SWAP +! R> @ R> PUTSCORE ;                   
 ;S                                                             
 ( UPDCNODE INSTALLVBI INSTALLHBI INITMEM   )                   
 : UPDCNODE ( WORM MOVE )                                       
   2DUP !CRIX                                                   
   OVER .VAL SWAP TMASK + C@                                    
   OR DUP 3F AND 3F = IF                                        
   OVER 7 !CRIX OVER UPDSCR                                     
   ENDIF !VAL ;                 ;S                              
 : INSTALLVBI ( ADDR -> )                                       
   7 SET-INT  ;                                                 
 : INSTALLHBI ( ADDR -> )                                       
  40 D40E C! 200 ! C0 D40E C! ;                                 
 : INITMEM DLH @ [ ' SAVDLH ] LITERAL !                         
            58 @ [ ' SCRMEM ] LITERAL !                         
   SCRMEM 50 + WDL 41 + ! SCRMEM WDL 46 + !                     
   SCRMEM 28 + WDL 4B + ! ;                                     
 ;S                                                             
 (          MAKEMOVE       )                                    
 : MAKEMOVE ( WORM -> STATUS )                                  
 DUP UPDSTATUS  DUP .STATUS                                     
  IF  DUP GETMOVE                                               
   DUP 6 < IF    OVER ZMAP                                      
 OVER UPDCRLOC                                                  
 2DUP MSND 2DUP UPDCNODE                                        
   OVER UPDMV OVER UPDMAP                                       
 ( OVER PAUSE ) OVER UPDCOL                                     
 2DUP UPDXPOS 2DUP UPDYPOS                                      
 OVER UPDLOC OVER UPDCOL                                        
 ( OVER ZCRMAP OVER UPDCRLOC )                                  
 ( OVER INC#MOVES ) 2DUP NXTMOVE                                
 UPDCNODE OVER UPDSTATUS                                        
 ENDIF DROP ENDIF DUP UPDSTATUS                                 
 .STATUS ; ;S                                                   
 ( GAME  ENDGAME  CSOUND    )                                   
 0 VARIABLE MVCNT 4 VAR WTICKS                                  
 4 VARIABLE PERIOD                                              
 : SETW# DUP ' W# ! DUP + ' 2*W# ! ;                            
 : GAME ( #PLAYERS )                                            
  (  1 VBION ! ) 0 QFLG !                                       
  DUP 0 DO I GWORM UPDCRLOC LOOP                                
  BEGIN 0 OVER 0 DO ( XSND ) ?PAUSE I SETW#                     
  BEGIN I 2* CDT + C@ 0= UNTIL                                  
   I GWORM MAKEMOVE + LOOP ?SPEED                               
   ( DUP 1+ DEN @ * STO ! )  0=                                 
   CONT 0= OR                                                   
 UNTIL DROP XSND ( 0 VBION ! ) ;                                
\ : CLEARPM  (  )  280 0                                        
\  DO 0 PMA 180 + I + C! LOOP ;                                 
 ;S                                                             
07 VAR CURSCROLL 0 VAR SADD 1 VAR SCRLFLAG                      
: FIXDUMMY LSADD @ 1A +  FSADD @                                
       DO I C@ 20 - I C! LOOP ;                                 
: SCRLON ;                                                      
\ 0 SCRLFLAG C! 56 WDL 40 + C! FSADD @ WDL 41 + ! ;             
: SCRLOFF ;                                                     
\ 1 SCRLFLAG C! 46 WDL 40 + C! SCRMEM 50 + WDL 41 + ! ;         
;S                                                              
 ( PSCORE  SCORE2         )                                     
 1E6 VAR FTIME 0 VAR WTIME                                      
 : SCORE2 ( #PLAYERS ) DROP ; ;S ( FOR NOW )                    
 0 OVER 0 DO I GWORM .WRM#                                      
 DUP + WSCR + @ + LOOP                                          
 FTIME @ SWAP / WTIME !  ANONB @ >R ANON                        
 0 DO XLITE I GWORM DUP HILITE                                  
 0 220 ! FF 22E C! .WRM# DUP +                                  
 WSCR + @ WTIME @ * -DUP                                        
 IF 20 MAX  220 !  BEGIN                                        
 22E C@ 0= ?START OR  UNTIL                                     
 ENDIF                                                          
 LOOP R> 0= IF ANOFF THEN                                       
 DELITE XSND  ;                                                 
                                                                
 ;S                                                             
 ( PLAYGAME  DELAY INITSTAT )                                   
 : INITSTAT DUP .KIND                                           
  IF 1 ELSE 0 ENDIF !STATUS ;                                   
 : PLAYGAME                                                     
\ 0 D208 C! 3 D20F C! ( INIT SOUND )                            
   CLEARF  ( CLEARPM ) ( WDISPLAY )                             
  #WORMS @ 0 DO I GWORM DUP                                     
  INITPOS I ' W# ! MVDELITE                                     
  INITSTAT LOOP                                                 
  #WORMS @ DUP GAME                                             
  CONT IF SCORE2 ELSE DROP THEN CLRKBD  ;                       
                                                                
                                                                
                                                                
 ;S                                                             
                                                                
 (  !NMOFE BANDOMIZE INCTALL )                                  
: !NMOVE ( WORM MOVE INDEX -> F)                                
  3E MIN                                                        
  OVER TMASK + C@ OVER AND IF                                   
  DROP                                                          
  DROP DROP 0 ELSE SWAP !MOVE 1                                 
  ENDIF ;                                                       
 : RANDOMIZE ( WORM )                                           
  3F 0 DO                                                       
  BEGIN DUP RAND 7 AND 5 MIN                                    
  I !NMOVE UNTIL LOOP                                           
  DROP ;                                                        
 : WLDISPLAY 50 28 DO 20 I SCRMEM + C! LOOP                     
  8 0 DO 0 I WSCR + ! 2 +LOOP ;                                 
 : IDISPLAY ( CLEARPM ) CLEARF ( WDISPLAY ) ( LOGO )            
    ;        ;S                                                 
( WORM I/O )                                                    
 0 <-> DED 1 <-> NEW 2 <-> AUTO 3 <-> WILD                      
 4 <-> SAME 5 <-> NAMED                                         
 : VMOVE ( I MOVE --> MOVE )                                    
   DUP   (  I  M )                                              
   6 < IF SWAP OVER TMASK + C@ AND IF DROP GETNEW# THEN ELSE    
   DUP GETNEW# = IF SWAP DROP ELSE DUP                          
   DOAI#   = IF SWAP DROP ELSE DUP                              
   DIE#    = IF SWAP DROP ELSE                                  
           DROP GETNEW# SWAP DROP THEN THEN THEN THEN ;         
 : RWORM ( WORM --> )                                           
   >R R 4 !KIND R 4 0 DO DUP I TBUF + C@ SWAP .NAME I + C! LOOP 
  40 0 DO DUP I IOBUF I + C@ VMOVE I SWAP !MOVE LOOP DROP       
  R> DROP ;                                                     
 : WWORM ( WORM )                                               
  40 0 DO DUP I .MOVE IOBUF I + C! LOOP DROP ;  ;S              
 ( INTROGAME  )                                                 
                                                                
 : INTROGAME                                                    
   ( CLEARPM ) CLEARF ( LFIELD )                                
  #WORMS @ 0 DO I GWORM DUP                                     
  CONT IF  RAND 3 AND IF                                        
   DUP 3 !KIND DUP INITMOVES DUP RANDOMIZE                      
   ELSE DUP 2 !KIND DUP INITAUTO THEN                           
  ELSE                                                          
     DUP 0 !KIND ENDIF                                          
  INITPOS ( DUP INITMOVES )                                     
  (  DUP ZMAP ) INITSTAT                                        
  LOOP CONT IF WLDISPLAY                                        
  #WORMS @ DUP GAME PROMPT2 SCORE2  ENDIF                       
  #WORMS @ 0 DO I GWORM 0 !SCORE                                
   LOOP  ; ;S                                                   
 ( SUMO  INITIALIZATION )                                       
  0 VAR CSBUILT                                                 
 : SUMO  ( INITIALIZE WORLD )                                   
 ( 0 COLBK C! DELITE ) ( WDL SETHSCRL  )                        
  CSBUILT @ IF ELSE                                             
  WCHSET BUILDCHSET CLEARF                                      
  1 CSBUILT ! ENDIF                                             
  0 QFLG !                                                      
  ?TERMINAL 0= IF                                               
  BEGIN ( LOGO ) INTROGAME                                      
  STOP UNTIL ENDIF                                              
  WLDISPLAY ( FF 02FC C! )                                      
0 GWORM 1 !KIND 1 GWORM 2 !KIND                                 
2 GWORM 3 !KIND 3 GWORM 0 !KIND  ; ;S                           
                                                                
                                                                
 ( SEL ILINE DELINE     )                                       
 : FIXLIN ( W# -> ) DROP ;                                      
\  DUP FIXL0 FIXL1 ;                                            
                                                                
 : HILINE  ( -> ) SEL @ MVHILTE ;                               
\ SEL @ COLPF0 + DUP C@ F0 AND                                  
\ A OR SWAP C! HIL2 ;                                           
\ : DELINE ( -> )  SEL @ ' W# ! MVDELITE ;                      
\ SEL @ COLPF0 + DUP C@ F0 AND                                  
\ 4 OR SWAP C! ;                                                
 : FIXCOL ( -> )    ;                                           
\ 4 0 DO COLPFT I + C@ COLPF0 I                                 
\        + C! LOOP ;                                            
;S                                                              
                                                                
                                                                
( COMMAND SHELL SUPPORT HBI1 FXHBI )  HEX                       
: CINC ( MAX VAL -> NEWVAL)                                     
  1+ SWAP OVER = IF DROP 0                                      
 ENDIF ;                                                        
                                                                
 4153 VARIABLE SAMEN 4D C, 45 C,                                
 : NAMESAME ( W ) .NAME DUP 8 BLANKS SAMEN SWAP 4 CMOVE ;       
;S                                                              
                                                                
 CODE HBI1 PHA,                                                 
   2 # LDA, WSYNC STA,                                          
   D01A STA, ( BKGRND LUM )                                     
   CURSCROLL LDA, HSCROL STA,                                   
   E0 # LDA,  CHBASE STA,                                       
   PLA, RTI, END-CODE                                           
( : FXHBI ' HBI1 INSTALLHBI  ;   )                              
( APPLY OPTIONC )                                               
( : DOMAN INITMOVES SFIELD ; )                                  
: DONEW DUP NAMESAME INITMOVES ;                                
: DOAUTO DUP NAMESAME INITAUTO ;                                
: DOWILD DUP NAMESAME RANDOMIZE ;                               
: VDOKIND DROP DONEW DOAUTO DOWILD DROP DROP ;                  
: DOKIND 5 MIN DUP + ' VDOKIND + @ EXECUTE ;                    
 : SETOPTS WLDISPLAY                                            
  ( LFIELD  )                                                   
   #WORMS @ 0 DO I GWORM                                        
 DUP .KIND DOKIND LOOP ;                                        
                                                                
 ;S                                                             
                                                                
                                                                
                                                                
   DUP .NAME I + C@ TEMIT LOOP DROP ;                           
: VKINDCASE  TDED TNEW  TSMART TMYST  TNAMED NOOP ;             
: KINDCASE  DUP + ' VKINDCASE + @ EXECUTE ;                     
: TYKIND DUP MAPCLR + C@ 286 C!                                 
         DUP A * 16 XY! DUP GWORM                               
.KIND KINDCASE DROP ( FIXL1 ) ;                                 
 ;S                                                             
                                                                
( WOPTION WSELECT NOP SHOWALL PROMPT HI LO )                    
: PROMPT P" ." F7:START F5:SELECT WORM F3:CHANGE WORM"          
  SEL @ FIXL2  ;                                                
: WOPTION 5 CURW .KIND CINC                                     
  CURW SWAP !KIND                                               
  SEL @ TYKIND                                                  
  ( FIXL1 PROMPT ) ;                                            
: WSELECT MVDELITE                                              
  1 SEL +! SEL @                                                
  MAX#W  = IF 0 SEL !                                           
   ENDIF SEL @ MVHILTE ;                                        
: NOP ;                                                         
: SHOWALL INITL0 SEL @  MAX#W 0 DO I SEL !                      
 I TYKIND LOOP SEL ! HILINE ;                                   
\ : DROPKEY 02FC C@ FF = IF ELSE KEY DROP THEN ;                
;S : HI hi ;  : LO lo ;                                         
    ( DOS EMULATION 2 )                                         
                                                                
                                                                
 : DOFF SPOFF RESET-IRQ ;                                       
                                                                
 : DON SPON INSTALL-DOIRQ ;                                     
;S                                                              
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
 (  KEYPOT GKEYPOT X! Y! XY! )                                  
                                                                
                                                                
                                                                
                                                                
: DECCNT DROP CCNT @ IF -1 CCNT +! 14 EMIT                      
BL CCNT @ TBUF + C! THEN ;                                      
: BMPCNT DUP CCNT @ 3 > IF 14 EMIT EMIT                         
 ELSE 1 CCNT +! EMIT THEN  CCNT @ 1- TBUF + C! ;                
: GTBUF 0 CCNT ! BEGIN KEY DUP                                  
  30 61 RANGE IF BMPCNT HIL2 0 ELSE DUP                         
  61 7A RANGE IF 20 - BMPCNT HIL2 0 ELSE DUP                    
  14 = IF DECCNT 0 ELSE DUP                                     
  20 2B RANGE IF BMPCNT 0 ELSE DUP                              
  D =  IF DROP 1 ELSE                                           
  DROP 0  BELL  THEN  THEN THEN THEN THEN UNTIL ; ;S            
( DOS EMULATION 4 )                                             
                                                                
 : FIXTBUF                                                      
     57 TBUF 8 + C! 52 TBUF 9 + C! 4D TBUF A + C!  ;            
 : TBUFINIT B 0 DO 20  TBUF I + C! LOOP ;                       
 : GNAME P" PROMPT2  P" DON                                     
   ."  FILE NAME:"  HIL2 TBUFINIT GTBUF FIXTBUF DOFF ;          
\ : MNAME (  --> FLG & PTR ) 0                                  
\  40 0 DO TBUF I GDIR MATCH IF SWAP DROP 1  I F# ! LEAVE THEN  
\             LOOP ;                                            
                                                                
                                                                
                                                                
                                                                
 : NOK ( NAMEOK ) 0 4 0 DO 20 TBUF I + C@ = 0= OR LOOP ; ;S     
                                                                
 ( DOS EMULATION F#PTR PTRF# )                                  
                                                                
                                                                
                                                                
                                                                
: DODIR P" ." DIRECTORY " ;                                     
                                                                
                                                                
                                                                
 : READF  DOFF 1 CMDOK !                                        
     VDOSDISK (  FXHBI ) IF                                     
     GNAME NOK IF                                               
     IOBUF READ-WORM   ELSE                                     
      P" ."  NOT A LEGAL NAME   " 0 CMDOK !  THEN               
    ELSE P" ."  INSERT A DOS DISK   "  THEN DON ;               
;S                                                              
;S ( DOS EMULATION )                                            
: ALLOCSEC ( --> SEC# FLAG )                                    
 0 720 1 DO I SECFREE? IF DROP I 1 LEAVE THEN LOOP ;            
: COPYNAME ( PTR )                                              
   0B 0 DO DUP TBUF I + C@ SWAP I +  5 + C! LOOP                
   DROP ;                                                       
: WRITENF ( PTR --> )                                           
  >R ALLOCSEC IF 40 R C! 1 R 1+ ! ( COUNT )  DUP MARKSEC WVTOC  
   R 3 + ! ( SSN ) R COPYNAME  R 3 + @ FBUF SWAP WDSEC UPDDIR   
   ELSE P" ."  NO ROOM ON DISK    " 0 CMDOK ! THEN R> DROP  ;   
 : FIXFBUF                                                      
    F# @ 4 * FBUF 7D + C! 0 FBUF 7E + C! ( LINK )               
    7D FBUF 7F + C! ( BYTE COUNT ) ;                            
                                                                
                                                                
;S                                                              
   ( DOS EMULATION WRITEF )                                     
: YOUR-DISK P" ."  INSERT A FORMATTED DISK    " ;               
 : WRITEF DOFF 1 CMDOK !                                        
    VDOSDISK (  FXHBI ) IF                                      
     GNAME NOK IF WRITE-WORM                                    
                                                                
                                                                
     THEN                                                       
    ELSE YOUR-DISK  THEN DON ;                                  
 : SAVEF 1 CMDOK !                                              
     DOFF VDOSDISK ( FXHBI ) IF WRITE-WORM                      
                                                                
                                                                
    ELSE YOUR-DISK  THEN DON ;                                  
                                                                
 ;S                                                             
( WRPWDL BMPWDL NWORM FINCMD DOREAD )                           
: =OR ( V A B --> )                                             
    ROT >R R = SWAP R> = OR ;                                   
                                                                
: P* PROMPT2 P" ;                                               
                                                                
                                                                
 : NWORM ( W )                                                  
   4 0 DO DUP .NAME I + TBUF I + C@ SWAP C! LOOP                
     4 !KIND ;                                                  
: FINCMD CMDOK @ IF PROMPT ELSE                                 
    P" PROMPT2 P" ."  DISK ERROR: " DISKERR @ LSB . THEN  ;     
: DOREAD P* ."  LOAD? <RETURN>      "  HIL2 CNFRM               
   IF READF CMDOK @ IF CURW RWORM THEN THEN FINCMD ;            
: DO? P* ."  LOAD/SAVE/UPDATE" ;                                
 ;S                                                             
;S   ( COMMANDS )                                               
 WDATA VAR SCRPOS                                               
: WRMENT ( PTR --> FLAG )                                       
  >R R C@ 40 AND 40 = R D + C@ 57 = AND                         
   R E + C@ 52 = AND R> F + C@ 4D = AND ;                       
: WEMIT ( CHR )                                                 
  20 - SCRPOS @ C! 1 SCRPOS +! ;                                
: PWENTRY ( PTR )                                               
  20 WEMIT 9 5 DO DUP I + C@ WEMIT LOOP DROP ;                  
: PDIR WDATA SCRPOS ! FF LASTDIRSEC !  1 CMDOK C!               
  VDOSDISK IF DOFF 40 0 DO I GDIR DUP WRMENT                    
   IF PWENTRY ELSE DROP (  0 CMDOK C! ) THEN LOOP  DON          
  ELSE P" ."  INSERT DOS DISK   " 0 CMDOK ! THEN ;              
: DODIR P" ."  DIRECTORY? <RETURN> " HIL2  CNFRM IF CLEARF      
  CLEARPM E0 CHNOW C! F SCRLVAL C! 2 WAIT  PDIR CMDOK @         
   IF P"  PROMPT2 THEN  THEN FINCMD ; ;S                        
( COMMANDS: DOWRITE SNAME DOSAVE ? CMD )                        
: DOWRITE P* ."  SAVE? <RETURN>       " HIL2                    
    CNFRM IF CURW WWORM WRITEF CMDOK @ IF CURW NWORM THEN THEN  
    FINCMD ;                                                    
: SNAME  TBUFINIT CURW 4 0 DO DUP .NAME I + C@                  
         TBUF I + C! LOOP DROP  FIXTBUF ;                       
: DOSAVE P* ."  UPDATE" TNAMED ." ?<RET.>  " HIL2               
    CNFRM IF SNAME CURW WWORM SAVEF THEN  FINCMD ;              
: ?CMD ?SPEED ?CH -DUP IF KEY DUP                               
  4C 6C =OR IF DOREAD ELSE DUP                                  
  53 73 =OR IF DOWRITE ELSE DUP                                 
  55 75 =OR IF DOSAVE ELSE DUP                                  
  47 67 =OR IF GCMD ELSE DUP                                    
  3F 2F =OR IF DO? ELSE DUP                                     
  3F 2F =OR IF DODIR  THEN THEN THEN THEN THEN THEN DROP        
  ( FXHBI ) SHOWALL THEN ; ;S                                   
 ( MAIN LOOP )                                                  
: WSTART SETOPTS ( WLDISPLAY ) FIXCOL ( WCHSETH CHNOW C! )      
PROMPT2 MVDELITE PLAYGAME  SHOWALL                              
  PROMPT SEL @ MVHILTE   ;                                      
: VTERMCHOICE NOP  WSTART WSELECT NOP WOPTION ;                 
: TERMCHOICE 4 MIN DUP + ' VTERMCHOICE + @ EXECUTE ;            
: ?WAITTERM                                                     
 BEGIN ?START -DUP ?CMD UNTIL ;                                 
: ?NOTERM BEGIN ?TERMINAL DUP 5                                 
 > SWAP 0= OR UNTIL ?TERMINAL ;                                 
: RUNLOOP SHOWALL PROMPT                                        
 BEGIN ?CLEAR ( DELETE FOR PRD)                                 
 ?WAITTERM DETRACT        DUP 5 < IF                            
 TERMCHOICE DEPTH -DUP IF P" . THEN                             
                    ELSE  DROP ENDIF                            
BEGIN ?CH DUP IF KEY DROP THEN 0= UNTIL AGAIN ; ;S              
\ INSTALLEM  FIXINTRPT                                          
: INSTALLEM  ( INSTALL INTRPTS )                                
 IRQVEC @ DUP E000 U< IF DROP ELSE IRQVAL ! THEN                
 INSTALL-DOIRQ ;                                                
: FIXINTRPTS                                                    
   RESET-IRQ SPOFF ;                                            
                                                                
: INITSPRITES ( INITSPRITE WORLD )                              
   D000 11 0 FILL                                               
  4 0 DO SPMEM I 2* + @                                         
      40 0 FILL LOOP                                            
   SPON ;                                                       
                                                                
 : DOFF FIXINTRPTS ;                                            
                                                                
 : DON SPON INSTALL-DOIRQ ; ;S                                  
 ( FIXINTRPTS DORTI )                                           
 CODE DORTI RTI, END-CODE                                       
0A CONSTANT VL                                                  
;S                                                              
 : FIXINTRPTS                                                   
  E462 INSTALLVBI )                                             
  ( E7B3 INSTALLHBI ) ( NOT FOR ALL VERSIONS OF O.S. ! )        
  ' DORTI INSTALLHBI                                            
  E0 CHBAS C! E0 CHBASE ! ;                                     
                                                                
                                                                
                                                                
                                                                
                                                                
 ;S                                                             
                                                                
( RESET ) 40 VARIABLE DORTI                                     
 0 #WORMS ! HERE 58 ALLOT <-> W1 HERE 58 ALLOT <-> W2           
            HERE 58 ALLOT <-> W3 HERE 58 ALLOT <-> W4           
 : GRIDGLIDERS  PAGE SP! RP! VL D418 C! ANOFF INITL0 HEARLD     
  0 #WORMS ! 0 SEL ! DORTI 318 !                                
  W1 INITWORM W2 INITWORM W3 INITWORM W4 INITWORM               
CSBUILT @ IF ELSE WCHSET BUILDCHSET 1 CSBUILT ! CLEARF          
 INITSPRITES INSTALLEM  THEN                                    
 SUMO PAGE    CLEARF                                            
 INITL0 0 9D C! ( NO MESSAGES )                                 
 4 0 DO I GWORM INITMOVES LOOP                                  
 4 #WORMS ! ( 1 2F0 C! ) ( CURSOR OFF ) DECIMAL                 
 ( BEGIN )  RUNLOOP (  AGAIN  )                                 
( RESTORE )  ( FIXINTRPTS )                                     
( 0 2F0 C! ) ( CURSOR ON  ) ; DECIMAL                           
 : TASK ; ( IS-FENCE  FINIS )  (   PROF ) ;S                    
( C64 WORM CHAR MAPS TEMPLATES ) HEX                            
HERE 7038 , C0E0 , 0080 ,    0 , CONSTANT M0R                   
HERE    0 , FF00 ,    0 ,    0 , CONSTANT M1R                   
HERE    0 , 8000 , E0C0 , 3870 , CONSTANT M2R                   
HERE    0 , 0100 , 0703 , 1C0E , CONSTANT M0L                   
HERE    0 , FF00 ,    0 ,    0 , CONSTANT M1L                   
HERE 0E1C , 0307 , 0001 ,    0 , CONSTANT M2L                   
HERE M0R , M1R , M2R , CONSTANT RCVEC                           
HERE M0L , M1L , M2L , CONSTANT LCVEC                           
 800 CONSTANT CHRMEM                                            
 400 CONSTANT DM                                                
D800 CONSTANT COLRMEM                                           
CHRMEM C0 8 * + CONSTANT LCS                                    
LCS 40 + CONSTANT RCS                                           
;S                                                              
                                                                
\ SOFT-CHARS                                                    
                                                                
\ : TO-BASIC                                                    
\ 8002 6 6 FILL                                                 
\ FFFC EXECUTE ;                                                
                                                                
: SOFT-CHARS                                                    
                                                                
  DC0E C@ FE AND DC0E C! ( NO KEYSCAN INT TIMER )               
  1 C@ FB AND 1 C!       ( CHAR ROM IN )                        
  D000 CHRMEM 800 CMOVE    ( COPY CHARS )                       
  1 C@ 4 OR 1 C!         ( CHAR ROM OUT, I/O IN )               
  DC0E C@ 1 OR DC0E C!   ( RESTART KEYSCAN )                    
  D018 C@ F0 AND CHRMEM 400 / + D018 C!                         
  ;                                                             
;S                                                              
\ EXPAND                                                        
CHRMEM CONSTANT CHST                                            
CODE EXPAND ( B - N ) XSAVE STX, N STY, N 1+ STY,               
 BOT LDA, 8 # LDY, BEGIN,                                       
 .A ROL, TAX, 0 # LDA, .A ROL, PHA,                             
 N ROL, N 1+ ROL, N ORA, N STA, PLA,                            
 CLC, N ROL, N 1+ ROL, N ORA, N STA,                            
 TXA, DEY, 0= UNTIL,                                            
 N LDA, XSAVE LDX, BOT STA, N 1+ LDA, BOT 1+ STA,               
 NEXT JMP, END-CODE                                             
: EXPD.MP  ( SRC DEST -- )                                      
 8 0 DO OVER I + C@ EXPAND OVER I + >R                          
  DUP MSB R C! LSB R> 8 + C!                                    
  LOOP 2DROP ;                                                  
: EXP.SET 40 0 DO I 8 * CHST + I 10 * CHST + 200 + EXPD.MP LOOP 
 ; ;S                                                           
\ TRY SOFT CHARS                                                
HEX                                                             
: TRY 100 0  DO I I 400 + C! LOOP                               
 COLRMEM 400 DISP C@ FILL ;                                     
        ;S                                                      
: IWDISPLAY                                                     
  ( SET PLAYER HPOS )                                           
  ( SET PLAYER HPOS )                                           
\ 28*                                                           
CODE 28* ( BYTE -- N )                                          
 BOT LDA, .A ASL, BOT 1+ ROL,                                   
          .A ASL, BOT 1+ ROL,                                   
 BOT ADC, TAY,                                                  
 0 # LDA, BOT 1+ ADC, BOT 1+ STA,                               
 TYA,                                                           
 .A ASL, BOT 1+ ROL,                                            
 .A ASL, BOT 1+ ROL,                                            
 .A ASL, BOT 1+ ROL,                                            
 BOT STA, NEXT JMP, END-CODE                                    
;S                                                              
                                                                
                                                                
                                                                
                                                                
\ SAVE-SYSTEM                                                   
DECIMAL                                                         
 HERE 64 C, 48 C, 58 C, 83 C, 89 C, 83 C, 84 C, 69 C, 77 C,     
 CONSTANT "SAV"                                                 
                                                                
: START-PROMPT ." DOIT " KEY DROP ;                             
 : SAVE-SYSTEM ( -- )                                           
  ." INSERT SAVE FILE DISK " START-PROMPT                       
  "SAV" 9 SETNAM                                                
  8 8 8 SETLFS                                                  
  0 +ORIGIN SP@     ( .A )                                      
   FENCE @ DUP LSB  ( .X )                                      
   SWAP MSB 1+      ( .Y )                                      
   65496 CALL ?ECODE                                            
  DROP ;                                                        
 ;S                                                             
HEX  ( ID.SMASH SMASHEM  MAKEBOOT )                             
: ID.SMASH ( NFA --> )                                          
   DUP C@ 1F AND 1+ 2 FILL ;                                    
: SMASHEM ( --> )                                               
  CONTEXT @ @ BEGIN                                             
  DUP >R PFA LFA @                                              
  R> ID.SMASH DUP 0= UNTIL DROP ;                               
                                                                
: MAKEBOOT ( --> )                                              
  ." STARTING "                                                 
  ' GRIDGLIDERS CFA ' QUIT !                                    
  SMASHEM ." HEADS SMASHED " CR                                 
( W.AS.CART )                                                   
BASE @ HEX                                                      
 C2C3 8004 ! 38CD 8006 ! 30 8008 C!                             
 0 +ORIGIN DUP 8000 ! 8002 !                                    
 ' NOOP CFA ' ABORT @ 8 + !                                     
BASE ! ;S                                                       
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
\ WORM I/O                                                      
HERE  40 C, 30 C, 3A C,                                         
      42 C, 4C C, 49 C, 50 C, 2E C, 57 C, 52 C, 4D C,           
 CONSTANT "WRM"                                                 
 0 VARIABLE DISKERR                                             
                                                                
 : WRITE-WORM  ( ? -- )                                         
  TBUF [ "WRM" 3 + ] LITERAL 4 CMOVE                            
  "WRM" 0B SETNAM                                               
  8 8 8 SETLFS                                                  
  IOBUF SP@           ( .A )                                    
  IOBUF 40 + DUP LSB  ( .X )                                    
   SWAP MSB           ( .Y )                                    
   FFD8  CALL DUP MSB IF 0 CMDOK ! DISKERR !                    
  ELSE DROP THEN  DROP                                          
 8 CLOSE DROP ; ;S                                              
\ WORM I/O                                                      
                                                                
                                                                
                                                                
                                                                
                                                                
 : READ-WORM  ( ? -- )                                          
  TBUF [ "WRM" 3 + ] LITERAL 4 CMOVE                            
  "WRM" 0B SETNAM                                               
  8 8 0 SETLFS                                                  
   0                ( .A )                                      
   IOBUF DUP LSB    ( .X )                                      
   SWAP MSB         ( .Y )                                      
   FFD5  CALL DUP MSB IF 0 CMDOK ! DISKERR !                    
  ELSE DROP THEN DROP                                           
  8 CLOSE DROP ; ;S                                             
 ( XEMIT )                                                      
BASE @ HEX                                                      
                                                                
                                                                
CODE SEMIT ( CHR  --> )                                         
  D3 LDY, BOT LDA, D1 )Y STA,                                   
\   286 LDA, F3 )Y STA,                                         
   D3 INC, POP JMP, END-CODE                                    
                                                                
: TEMIT DUP 40 < IF 40 + THEN                                   
 2* 40 - DUP SEMIT 1+ SEMIT ;                                   
;S                                                              
: TNAMED 20 SEMIT CURW 4 0 DO                                   
  DUP .NAME I + C@ TEMIT                                        
  LOOP DROP ;                                                   
BASE ! ;S                                                       
 ( CODE FOR JOY STICKS )                                        
 CODE (FLKBD)                                                   
  BEGIN, FFE4 JSR, 0= UNTIL,                                    
  RTS, END-CODE                                                 
                                                                
 CODE FLKBD XSAVE  STX,                                         
  ' (FLKBD) JSR,                                                
  XSAVE LDX, NEXT JMP, END-CODE                                 
  CODE RJS1 XSAVE STX,                                          
   SEI, 0 # LDA, DC02 STA,                                      
   DC01 LDA, 1F # AND, 1F # EOR, PHA,                           
   0= NOT IF, ' (FLKBD) JSR, THEN,                              
   FF # LDA, DC02 STA,                                          
   CLI, XSAVE LDX, PLA,                                         
   PUSH0A JMP, END-CODE                                         
   ;S                                                           
 ( CODE FOR JOY STICKS )                                        
 CODE (JS1) SEI, 0 # LDA, DC02 STA,                             
 DC01 LDA, 1F # AND, 1F # CMP, PHA,                             
 FF # LDA, DC02 STA, CLI, PLA,                                  
 RTS, END-CODE                                                  
                                                                
                                                                
                                                                
  CODE RJS0 XSAVE STX,                                          
   SEI, 0 # LDA, DC02 STA,                                      
   DC00 LDA, 1F # AND, 1F # EOR, PHA,                           
   0= NOT IF, ' (FLKBD) JSR, THEN,                              
   FF # LDA, DC02 STA,                                          
   CLI, XSAVE LDX, PLA,                                         
   PUSH0A JMP, END-CODE                                         
   ;S                                                           
 ( JSKEYS  )                                                    
 : TRXJS    ( NUM --> KEY )                                     
    DUP 10 = IF DROP 1 ELSE                                     
    DUP IF DROP 20 ELSE THEN THEN ;                             
 : JKEY0 RJS0 TRXJS ;                                           
 : JKEY1 RJS1 TRXJS ;                                           
 : ?JSKEY 0 20 0 DO                                             
   JKEY0 OR LOOP ;                                              
 : JSCLR                                                        
   BEGIN ?JSKEY 0= UNTIL ;                                      
;S CODE (?CH) XSAVE STX,                                        
  ' (JS1) JSR, 0= NOT IF,                                       
  ' (FLKBD) JSR, THEN,                                          
   C6 LDA, 0= NOT IF, 277 LDA, THEN,                            
  XSAVE LDX, PUSH0A JMP, END-CODE                               
 ;S                                                             
( HEARLD )                                                      
: HEARLD 0 14 XY! ." PRESS F7 WHEN READY "                      
  0 16 XY! ." WORMS? COPYRIGHT 1983 BY DAVID S MAYNARD"         
  0 17 XY! ." C64 VERSION BY D MAYNARD & STEVEN HAYES"          
( STARTUP CODE ) BASE @ HEX                                     
: WORMS? PAGE                                                   
  INSTALLEM RESET-IRQ                                           
  0 CSBUILT ! GRIDGLIDERS ;                                     
  ' WORMS? CFA ' QUIT !                                         
                                                                
                                                                
                                                                
C2C3 8004 ! 38CD 8006 ! 30 8008 C!                              
0 +ORIGIN   8002 !                                              
                                                                
FREEZE   BASE ! ;S                                              
( INTERRUPT TESTING CODE ) BASE @ HEX                           
(  0 VARIABLE CLKLO )                                           
CODE BUMP CLKLO  INC, 0= IF,                                    
     CLKLO  1+ INC, THEN,                                       
     RTS, END-CODE                                              
CODE FBUMP ' BUMP JSR,                                          
     NEXT JMP, END-CODE                                         
314 CONSTANT IRQVEC                                             
FFFC VARIABLE IRQVAL                                            
CODE DOIRQ                                                      
     1 # LDA, D019 AND, 0= IF,                                  
     ELSE, 1 # LDA, D019 STA,                                   
 ' BUMP JSR, ' VBI JSR,                                         
 PLA, TAY, PLA, TAX, PLA, RTI, THEN,                            
     IRQVAL ) JMP, END-CODE                                     
BASE ! ;S                                                       
 ( INTERRUPT TESTING CONT ) BASE @ HEX                          
CODE INSTALL-DOIRQ                                              
 SEI, ' DOIRQ LSB # LDA,                                        
     IRQVEC STA,                                                
   ' DOIRQ MSB # LDA,                                           
     IRQVEC 1+ STA,                                             
     FB # LDA, D012 STA,                                        
     7F # LDA, D011 AND, D011 STA,                              
     1 # LDA, D01A ORA, D01A STA,                               
 CLI, NEXT JMP, END-CODE                                        
CODE RESET-IRQ SEI,                                             
 D01A LDA, PHA, 0 # LDA, D01A STA,                              
    IRQVAL LDA, IRQVEC STA,                                     
    IRQVAL 1+ LDA, IRQVEC 1+ STA,                               
    PLA, FE # AND, D01A STA,                                    
 CLI, NEXT JMP, END-CODE BASE ! ;S                              
( LOAD SCREEN ) FORTH 1 DISP ! 0 DISP 2+ C! DISPLAY             
?FIND SEVEN* IFSO 100 FENCE ! FORGET SEVEN* FREEZE              
DECIMAL 103 105 HEX THRU                                        
DECIMAL 150 150 HEX THRU                                        
DECIMAL  01  14 HEX THRU                                        
DECIMAL  93  93 HEX THRU                                        
DECIMAL  89  92 HEX THRU                                        
DECIMAL 138 140 HEX THRU                                        
DECIMAL  15  18 HEX THRU                                        
DECIMAL 102 102 HEX THRU                                        
DECIMAL 160 167 HEX THRU                                        
DECIMAL 145 147 HEX THRU                                        
DECIMAL  19  64 HEX THRU DECIMAL 143 143 HEX THRU               
DECIMAL  66  70 HEX THRU                                        
\ HERE U. AS.CART HEX 8012 HERE - 0 MAX ALLOT                   
DECIMAL  71  74 HEX THRU   -->                                  
\ CONT ...                                                      
DECIMAL 118 119 HEX THRU                                        
DECIMAL  75  76 HEX THRU                                        
DECIMAL 100 101 HEX THRU                                        
DECIMAL  77  87 HEX THRU                                        
DECIMAL 106 106 HEX THRU                                        
HEX HERE U. 8012 HERE - 0 MAX ALLOT                             
DECIMAL  88  88 HEX THRU                                        
FREEZE ( AS.CART )                                              
                                                                
                                                                
;S                                                              
                                                                
' GRIDGLIDERS CFA ' QUIT !                                      
                                                                
                                                                
( LOAD SCREEN ) AS.CART LOCATE-ON FORTH 1 DISP ! DISPLAY        
\ ?FIND SEVEN* IFSO 100 FENCE ! FORGET SEVEN* FREEZE            
\ DECIMAL 103 105 HEX THRU                                      
\ DECIMAL 150 150 HEX THRU                                      
\ DECIMAL  01  14 HEX THRU                                      
\ DECIMAL  93  93 HEX THRU                                      
\ DECIMAL  89  92 HEX THRU                                      
\ DECIMAL 138 140 HEX THRU                                      
\ DECIMAL  15  18 HEX THRU                                      
\ DECIMAL 102 102 HEX THRU                                      
\ DECIMAL 160 167 HEX THRU                                      
\ DECIMAL 145 147 HEX THRU                                      
( DECIMAL  19  64 HEX THRU ) DECIMAL 143 143 HEX THRU           
DECIMAL  66  70 HEX THRU                                        
HERE U. AS.CART HEX 8012 HERE - 0 MAX ALLOT                     
DECIMAL  71  74 HEX THRU   -->                                  
\ CONT ...                                                      
DECIMAL 118 119 HEX THRU                                        
DECIMAL  75  76 HEX THRU                                        
DECIMAL 100 101 HEX THRU                                        
DECIMAL  77  87 HEX THRU                                        
                                                                
DECIMAL 106 106 HEX THRU                                        
DECIMAL  88  88 HEX THRU                                        
FREEZE ( AS.CART )                                              
                                                                
                                                                
;S                                                              
                                                                
' GRIDGLIDERS CFA ' QUIT !                                      
                                                                
                                                                
( TMASK ) HEX                                                   
HERE 1 C, 2 C, 4 C, 8 C, 10 C, 20 C, 40 C, CONSTANT TMASK       
HERE 5 C, 2 C, 6 C, 4 C, CONSTANT MAPCLR                        
COLRMEM 28 15 * + CONSTANT COLRL0                               
: INITL0  ( INIT COLORS FOR SCORE )                             
   28  0 DO I A / MAPCLR + C@                                   
         COLRL0 I + C! LOOP                                     
  28 0 DO I A / MAPCLR + C@                                     
( CHSET BUILDING )  HEX                                         
: ORCHARVEC ( CHARADDR VECADDR --> )                            
  8 0 DO 2DUP I + C@ OVER I + C@ OR SWAP                        
  I + C! LOOP DROP DROP ;                                       
                                                                
 : ORCHAR ( CHB CINDX CVEC --> CHB )                            
   >R 4 0 DO 2DUP TMASK I + C@                                  
   AND IF OVER 8 * + J  I + I + @ ORCHARVEC                     
   ELSE DROP THEN LOOP DROP R> DROP ;                           
 : BUILDCHSET DROP SOFT-CHARS EXP.SET                           
    LCS 40 0 FILL RCS 40 0 FILL                                 
    LCS 8 0 DO                                                  
    I LCVEC ORCHAR LOOP DROP                                    
    RCS 8 0 DO                                                  
    I RCVEC ORCHAR LOOP DROP 1 LCS 3 + C!                       
    FF KEYMASK ! ; ;S                                           
\ C64 !VAL SUPPORT                                              
\ : XFORM ( N - R L )                                           
\   2* DUP 40 AND IF 1+ THEN 3F AND                             
\ DUP 7 AND C8 + SWAP 2/ 2/ 2/ C0 + ;                           
8D 8D THRU ( LOADS XFORM )                                      
: C64!CELL ( 8BIT X Y )                                         
 ->EVEN SWAP 28* SWAP - SWAP 2* +                               
 ( V OFFSET ) >R DUP XFORM ( V R L )                            
DM R + >R R C! R> 1+ C!                                         
COLBITS MAPCLR + C@ R> COLRMEM + 2DUP 1+ C! C! ;                
: 64!VAL ( W 8BIT -- )                                          
SWAP DUP .XPOS SWAP .YPOS C64!CELL ;                            
: CLEAR64PF                                                     
2 ( #V ? ) 0 DO #H 0 DO 00 I J C64!CELL LOOP LOOP               
DM DM 50 + 2D0 CMOVE                                            
COLRMEM 348 1 FILL ;                                            
\ ASSY XFORM                                                    
CODE XFORM ( 6BIT - R L ) BOT LDA, 3F # AND,                    
 .A ASL, BOT STA, 40 # AND, 0= NOT IF, INY, THEN,               
 TYA, BOT ADC, ( 6BIT CIRC. SHIFT )                             
 PHA, 7 # AND, C8 # ORA, BOT STA,                               
 PLA, .A LSR, .A LSR, .A LSR,                                   
 7 # AND, C0 # ORA, PUSH0A JMP,                                 
 END-CODE                                                       
CODE ->EVEN ( N -- N F )                                        
 BOT LDA, .A LSR, CS NOT IF, INY, THEN, TYA,                    
 PUSH0A JMP, END-CODE                                           
CODE COLBITS                                                    
 6 # LDY, BEGIN, BOT LSR, DEY, 0= UNTIL, NEXT JMP, END-CODE     
                                                                
                                                                
;S                                                              
( SCREEN TO SPRITE ) BASE @ HEX                                 
0 VARIABLE SCRPTR 0 VARIABLE SPPTR                              
HERE 1 C, 2 C, 4 C, 8 C, 10 C, 20 C, 40 C,  80 C, CONSTANT PWT  
: SETBIT ( ROW COL )                                            
   8 /MOD ROT 3 * + SWAP 7 SWAP -                               
   SWAP SPPTR @ + SWAP                                          
   PWT + C@ OVER C@ OR SWAP C! ;                                
: GETCH ( ROW COL --> FLAG )                                    
   SWAP 40 * + SCRPTR @ + C@ BL = 0= ;                          
: MAKESP ( SCREEN# SPADDR )                                     
  SPPTR ! BLOCK SCRPTR !                                        
  10 0 DO 18 0 DO                                               
    J I GETCH IF J I SETBIT THEN                                
   LOOP LOOP ;                                                  
   BASE ! ;S                                                    
                                                                
\ \ 3PICK SCORE2 ( C64 PAYOFF STUFF )                           
HEX                                                             
CODE 3PICK SEC 2+ LDA, PHA, SEC 3 + LDA, PUSH JMP, END-CODE     
: C64!COLR ( W# X Y )                                           
 ->EVEN SWAP 28* SWAP - SWAP 2* +                               
 ( V OFFSET ) >R                                                
 MAPCLR + C@ R> PAD + 2DUP 1+ C! C! ;                           
: SHOWPTS ( W# )                                                
PAD 320 0 FILL DUP 40* 3F +                                     
WDATA 14 0 DO 14 0 DO ( W# C A )                                
  DUP C@ 3PICK = IF 3PICK I J C64!COLR THEN 1+ LOOP             
 LOOP 2DROP 2* WSCR + @                                         
PAD COLRMEM 320 CMOVE CONT IF 80 MAX JIFFYS ELSE DROP THEN ;    
: SCORE2 DROP CONT IF COLRMEM PAD 320 + 320 CMOVE               
 4 0 DO I GWORM .KIND IF I SHOWPTS THEN STOP IF LEAVE THEN      
LOOP PAD 320 + COLRMEM 320 CMOVE THEN ;                         
( SPRITE STUFF ) BASE @   HEX 4 CONSTANT #SP                    
HERE 340 , 380 , 3C0 , 2040 , CONSTANT SPMEM                    
HERE  0D C, 0E C, 0F C, 81 C, CONSTANT SP-PTR                   
HERE 0D C, 0A C, 0E C, 03 C, CONSTANT SPCOLR                    
: SPON F D015 C! 0F D017 C! 0F D01D C!                          
 #SP 0 DO I SP-PTR + C@ 07F8 I + C! LOOP                        
(  #SP 0 DO SPMEM I I + + @ 40 00 FILL LOOP )                   
 #SP 0 DO SPCOLR I + C@ D027 I + C! LOOP                        
  ;                                                             
: SPOFF 0 D015 C! ;                                             
: SPPOS ( X Y SP# --> )                                         
  DUP + D000 + >R R C! R> 1+ C! ;                               
                                                                
BASE !                                                          
                                                                
 ;S                                                             
( SCREEN TO SPRITE ) BASE @ HEX                                 
0 VARIABLE SCRPTR 0 VARIABLE SPPTR                              
HERE 1 C, 2 C, 4 C, 8 C, 10 C, 20 C, 40 C,  80 C, CONSTANT PWT  
: SETBIT ( ROW COL )                                            
   8 /MOD ROT 3 * + SWAP 7 SWAP -                               
   SWAP SPPTR @ + SWAP                                          
   PWT + C@ OVER C@ OR SWAP C! ;                                
: GETCH ( ROW COL --> FLAG )                                    
   SWAP 40 * + SCRPTR @ + C@ BL = 0= ;                          
: MAKESP ( SCREEN# SPADDR )                                     
  SPPTR ! BLOCK SCRPTR !                                        
  10 0 DO 18 0 DO                                               
    J I GETCH IF J I SETBIT THEN                                
   LOOP LOOP ;                                                  
   BASE ! ;S                                                    
                                                                
\ YXSPRITE                                                      
CODE YXSPRITE ( Y X W# )                                        
 3 # LDA, SETUP JSR,                                            
 N LDY, PWT ,Y LDA, N 5 + STA,                                  
 FF # EOR, D010 AND,                                            
 N 3 + ROR, CS IF, N 5 + ORA, THEN,                             
 D010 STA, TYA, .A ASL, TAY,                                    
 N 2+ LDA, D000 ,Y STA, INY,                                    
 N 4 + LDA, D000 ,Y STA,                                        
 NEXT JMP, END-CODE                                             
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
 ( TEST SOUND ROUTINES ) BASE @ HEX                             
: SNDSETUP                                                      
   19 D405 C! ( ATACT/DECAY )                                   
   44 D406 C! ( SUSTAIN/RELEASE )                               
   11 D404 C! ( WV )                                            
   800 D400 C! ( FREQ )    ;                                    
 40 VAR SDLY                                                    
: TSND SNDSETUP  1+ SWAP DO                                     
\ SOUND INTERFACE                                               
HERE 0 C, 7 C, E C, CONSTANT SEVEN*                             
: C64SOUND ( W SR AD F N )                                      
( ADSR FIRST THEN WAVEFORM )                                    
  SEVEN* + C@ >R                                                
0 D404 R + C! ( SOUND OFF )                                     
  D400 R + !  ( FREQ )                                          
  D405 R  + C! ( ATTACK/DECAY )                                 
  D406 R  + C! ( SUSTAIN/RELEASE )                              
  D404 R> + C! ( WAVEFORM )                                     
  ;                                                             
: XSND ( -- ) ( ALL SOUND OFF )                                 
3 0 DO 0 0 0 0 I C64SOUND LOOP ;                                
                                                                
                                                                
                                                                
 ( TKILLW FSOUND ) 0 VAR SCH                                    
 : ROVR SCH @ 1+ DUP 3 = IF DROP 0 THEN DUP SCH ! ;             
11 <-> WV 00 <-> SR 19 <-> AR                                   
: FSOUND ( FREQ --> )                                           
 WV SR AR 4 ROLL ROVR C64SOUND ;                                
02 <-> DLY                                                      
: TKILLW ( -- )                                                 
  09 64 DO                                                      
  I >< DUP 2000 - 0 MAX SWAP  DO                                
   I FSOUND DLY 0 DO NOOP LOOP                                  
 -200 +LOOP STOP IF LEAVE THEN                                  
 -8 +LOOP ;                                                     
 ;S                                                             
                                                                
                                                                
                                                                
 \ WORMS? COPYRIGHT (C) 1983 BY DAVID S. MAYNARD                
 ( MASTER   JULY 10, 1983.  COMMODORE 64 INITIAL ) HEX          
  C000 CONSTANT HIMEM HIMEM 1000 - CONSTANT APMAREA             
 0 VARIABLE FNCUR 0 VARIABLE CHCUR                              
 CODE (?CH)                                                     
   C6 LDA, 0= NOT IF, 277 LDA, THEN, PUSH0A JMP, END-CODE       
HERE 0 C, 4 C, 2 C, 1 C, 0 C, 85 - CONSTANT CV85-               
 : ?CH BEGIN (?CH)                                              
     DUP IF DUP 85 MAX 89 MIN CV85- + C@                        
       DUP IF FNCUR ! ELSE DROP DUP CHCUR ! THEN                
     KEY DROP THEN                                              
 0= UNTIL CHCUR @ ;                                             
 : KEY BEGIN CHCUR @ 0= WHILE ?CH DROP REPEAT                   
 CHCUR @ 0 CHCUR ! ;                                            
 : =KEY SWAP OVER = IF KEY = ELSE DROP 0 THEN ;                 
 : ?TERMINAL ?CH DROP FNCUR @ 0 FNCUR ! ; ;S                    
 \ WORMS? COPYRIGHT (C) 1983 BY DAVID S. MAYNARD                
 ( WITH DISK/IO WITHOUT BUILDS DOES  )                          
 ( MASTER   JULY 10, 1983.  COMMODORE 64 INITIAL ) HEX          
  C000 CONSTANT HIMEM HIMEM 1000 - CONSTANT APMAREA             
\ CODE CTCH C6 LDA, PUSH0A JMP, END-CODE                        
 CODE (?CH)                                                     
  C6 LDA, 0= NOT IF, 277 LDA, THEN, PUSH0A JMP, END-CODE        
 : =KEY SWAP OVER = IF KEY = ELSE DROP 0 THEN ;                 
 0 VARIABLE FNCUR 0 VARIABLE CHCUR                              
HERE 0 C, 4 C, 2 C, 1 C, 0 C, DUP CONSTANT CONSOLVEC            
 85 - CONSTANT CV85-                                            
 : ?CH BEGIN (?CH) DUP DUP IF 85 MAX 89 MIN CV85- + C@ THEN     
 -DUP WHILE FNCUR ! KEY 2DROP                                   
 REPEAT ;                                                       
 ( HANDLES F, F X, NOT X F BUT S.B. OK )                        
 : ?TERMINAL ?CH DROP FNCUR @ 0 FNCUR ! ; ;S                    
  XX        XX                                                  
   XX      XX                                                   
   XX      XX                                                   
    XX    XX                                                    
    XX    XX                                                    
     XX  XX                                                     
     XXXXXX                                                     
XXXXXXXXXXXXXXXX                                                
XXXXXXXXXXXXXXXX                                                
     XXXXXX                                                     
    XXX  XXX                                                    
   XXX    XXX                                                   
   XXX    XXX                                                   
  XXX      XXX                                                  
  XXX      XXX                                                  
  XXX      XXX                                                  
 X    X                                                         
  X  X                                                          
  X  X                                                          
XXXXXXXX                                                        
   XX                                                           
  X  X                                                          
  X  X                                                          
 X    X                                                         
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
HERE 00 C, 00 C, 00 C,                                          
 0 C,  0 C,  0 C,                                               
 0 C,  0 C,  0 C,                                               
00 C, FF C,  0 C,                                               
 0 C,  0 C,  0 C,                                               
 0 C,  0 C,  0 C,                                               
 0 C,  0 C,  0 C,                                               
 0 C,  0 C,  0 C,                                               
;S                                                              
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
 0 C, 00 C,  0 C,                                               
 0 C, 00 C,  0 C,                                               
 0 C, 00 C,  0 C,                                               
01 C, 80 C,  0 C,                                               
00 C, C0 C,  0 C,                                               
00 C, 60 C,  0 C,                                               
00 C, 30 C,  0 C,                                               
00 C, 18 C,  0 C,                                               
;S                                                              
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
 0 C, 00 C,  0 C,                                               
 0 C, 00 C,  0 C,                                               
 0 C, 00 C,  0 C,                                               
01 C, 80 C,  0 C,                                               
 3 C, 00 C,  0 C,                                               
 6 C, 00 C,  0 C,                                               
 C C, 00 C,  0 C,                                               
18 C, 00 C,  0 C,                                               
;S                                                              
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
 0 C,  0 C,  0 C,                                               
 0 C,  0 C,  0 C,                                               
 0 C,  0 C,  0 C,                                               
FF C,  0 C,  0 C,                                               
 0 C,  0 C,  0 C,                                               
 0 C,  0 C,  0 C,                                               
 0 C,  0 C,  0 C,                                               
 0 C,  0 C,  0 C,                                               
;S                                                              
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
18 C, 00 C,  0 C,                                               
 C C, 00 C,  0 C,                                               
 6 C, 00 C,  0 C,                                               
01 C, 80 C,  0 C,                                               
 0 C, 00 C,  0 C,                                               
 0 C, 00 C,  0 C,                                               
 0 C, 00 C,  0 C,                                               
 0 C, 00 C,  0 C,                                               
;S                                                              
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
00 C, 18 C,  0 C,                                               
00 C, 30 C,  0 C,                                               
00 C, 60 C,  0 C,                                               
01 C, 80 C,  0 C,                                               
00 C, 00 C,  0 C,                                               
00 C, 00 C,  0 C,                                               
00 C, 00 C,  0 C,                                               
00 C, 00 C,  0 C,                                               
;S                                                              
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
 0 C,  0 C,  0 C,                                               
 0 C,  0 C,  0 C,                                               
 0 C,  0 C,  0 C,                                               
01 C, 80 C,  0 C,                                               
 0 C,  0 C,  0 C,                                               
 0 C,  0 C,  0 C,                                               
 0 C,  0 C,  0 C,                                               
00 C, 00 C, 00 C,                                               
;S                                                              
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
18 C, 18 C,  0 C,                                               
 C C, 30 C,  0 C,                                               
 6 C, 60 C,  0 C,                                               
FF C, FF C,  0 C,                                               
 3 C, C0 C,  0 C,                                               
 6 C, 60 C,  0 C,                                               
 C C, 30 C,  0 C,                                               
18 C, 18 C,  0 C, CONSTANT CRMAP ;S                             
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
18 C, 18 C,  0 C,                                               
 C C, 30 C,  0 C,                                               
 6 C, 60 C,  0 C,                                               
FF C, FF C,  0 C,                                               
 3 C, C0 C,  0 C,                                               
 6 C, 60 C,  0 C,                                               
 C C, 30 C,  0 C,                                               
18 C, 18 C,  0 C, CONSTANT CRMAP ;S                             
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                
: FOO 8 0 DO I 1 AND 0= IF CR ." XX P " THEN                    
I OVER    + C@ EXPAND DUP MSB 3 .R ."  C,"                      
LSB 3 .R ."  C, 00 C,"                                          
LOOP DROP ;                                                     
                                                                
                                                                
                                                                
                                                                
C2C3 8004 ! 38CD 8006 ! 30 8008 C!                              
20CB 8002 !                                                     
                                                                
                                                                
                                                                
                                                                
                                                                
                                                                

(lldb) 