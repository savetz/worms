\ WORMS? COPYRIGHT (C) 1983 BY DAVID S. MAYNARD                
( WITH DISK/IO WITHOUT BUILDS DOES  )                          
( MASTER   APRIL 18, 1983.  ATARI 800  FINAL   ) HEX           
 8000 CONSTANT HIMEM HIMEM 1000 - CONSTANT PMAREA              
CODE ?TERMINAL                                                 
  D01F LDA, 7 # EOR, 7 # AND,                                  
  PHA, 0 # LDA, PUSH JMP,                                      
  END-CODE                                                     
: SOUND                                                        
  0232 C@ 07 AND                                               
  D20F C! 0 D208 C!                                            
  2 * D200 + >R >R 10 * OR                                     
  EF AND                                                       
  100 * R> OR R> ! ;                                           
: XSND                                                          
 D208 D200 DO 0 I C! LOOP ;  ;S                                
( WORM GAME START )  HEX                                       
 : <-> CONSTANT ;  : < U< ;                                    
 : VAR VARIABLE ;                                              
4 <-> 4  8 <-> 8 5 <-> 5 6 <-> 6 3F <-> 3F FF <-> FF           
40 <-> 40 A <-> A 10 <-> 10 7 <-> 7 20 <-> 20 FFFF <-> FFFF    
-1 <-> -1 28 <-> 28 F0 <-> F0 80 <-> 80 9 <-> 9 200 <-> 200    
50 <-> 50 21E <-> 21E 4C <-> 4C 22E <-> 22E 220 <-> 220 F <-> F
14 <-> #H  16 <-> #V  12 <-> #DH 12  <-> #DV 1 <-> H0 1 <-> V0 
#DH 2 / <-> STX                                                
#DV 2 / <-> STY                                                
0 <-> W# 0 <-> 2*W#                                            
0 VAR QFLG                                                     
: STOP ?TERMINAL QFLG @ OR                                     
  DUP QFLG ! ;                                                 
: CONT STOP 0= ;                                               
                                                               
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
C VAR MXLUM  0A VAR STO                                        
1 <-> PWDTH                                                    
0 VAR HCSHI                                                    
0 VAR LOCK ( INTERLOCK FOR FORTH VBI ! IN UPDMAP )             
           ( LOCKED IN UPDMAP CHECKED IN CDEST2 )              
( WORM GAME CONSTANTS CONT )                                   
( FIRST 1 + 800 / 800 * 800 - )                                
( 02E5 @ C1F - ) PMAREA DUP <-> PMA                            
 200 - DUP <-> WCHSET ( FITS! )                                
 #H #V * - <-> WDATA                                           
D40A <-> WSYNC E4 <-> MAXPOT                                   
022F <-> DMACTL D01D <-> GRACTL                                
D407 <-> PMBASE                                                
2C8 <-> COLBK 2C4 <-> COLPF0                                   
2C5 <-> COLPF1 2C6 <-> COLPF2                                  
2C7 <-> COLPF3 2C0 <-> COLPM0                                  
2C1 <-> COLPM1 2C2 <-> COLPM2                                  
2C3 <-> COLPM3 26F <-> GPRIOR                                  
D300 <-> PORTA 0270 <-> POT0                                   
D40E <-> NMIEN   ;S                                            
                                                               
( SOUND TABLES AND CONST     )                                 
 D201 <-> AUDC D200 <-> AUDF                                   
 D208 <-> AUDCTL 1 VAR VBION                                   
HERE                                                           
  0 , 0 , 0 , 0 ,                                              
<-> FRQT ( FREQ. TABLE )                                       
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
HERE  PMA ,                                                    
  PMA , PMA , PMA ,                                            
<-> MAPX ( MAPADDR TABLE )                                     
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
( INTERRUPT ROUTINES UPDHS)                                    
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
( INTERRUPT ROUTINES SETVOL )                                  
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
(  PMXTAB SETVOL             )                                 
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
( INTERRUPT DAMPPOT  )                                         
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
( INTERRUPT ROUTINES RPOTS )                                   
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
: .XPOS ( WORM -> XPOS ) C@ ;                                  
: .YPOS 1+ C@ ;                                                
: .LOC 2+ @ ;                                                  
: .#MOVES 4 + @ ;                                              
CODE .STATUS 1 # LDA, SETUP JSR, 6 # LDY, N )Y LDA, PHA,       
  0 # LDA, PUSH JMP, END-CODE                                  
CODE .WRM# 1 # LDA, SETUP JSR, 7 # LDY, N )Y LDA, PHA,         
  0 # LDA, PUSH JMP, END-CODE                                  
: .MOVE ( WORM N -> MOVE )                                     
   B + + C@ ;                                                  
: !XPOS ( WORM XPOS )                                          
  SWAP C! ;                                                    
: !YPOS SWAP 1+ C! ;                                           
: !LOC SWAP 2+ ! ;                                             
: !#MOVES SWAP 4 + ! ;     ;S                                  
( WORM PRIMITIVES CONT )                                       
: .HPOS ( WORM )                                               
  8 + C@ ;                                                     
: !HPOS ( WORM HPOS )                                          
  SWAP 8 + C! ;                                                
: .VPOS  9 + C@ ;                                              
: !VPOS SWAP 9 + C! ;                                          
: .CRIX A + C@ ;                                               
: !CRIX SWAP A + C! ;                                          
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
: WAIT ( TICKS -> )                                            
 0 220 ! FF 22E C! 1 MAX                                       
 220 ! BEGIN 22E C@ 0= UNTIL                                   
 ;                                                             
;S                                                             
                                                               
( WORM PRIMITIVES CNT WAITTAB )                                
: !STATUS SWAP 6 + C! ;                                        
: !WORM# SWAP 7 + C! ;                                         
: !MOVE ( WORM N MOVE )                                        
  SWAP ROT B + + C! ;                                          
: .VAL .LOC C@ ;                                               
: !VAL SWAP .LOC C! ;                                          
: [.VAL] .VAL 3F AND ;                                         
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
: 2* DUP + ;                                                   
: 3* DUP 2* + ;                                                
: 4* 2* 2* ;                                                   
: 8* 4* 2* ;                                                   
55 <-> COLCRS  54 <-> ROWCRS                                   
: X! COLCRS ! ; : X@ COLCRS @ ;                                 
: Y! ROWCRS C! ; : Y@ ROWCRS C@ ;                               
: XY!  Y! X! ;   : ^ 1 0 XY! ;                                  
: P" 0 2 XY! ;    ;S                                            
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
                                                               
                                                               
( TABLE BUILDERS PMTAB )                                       
HERE                                                           
   PMA 200 + ,                                                 
   PMA 280 + ,                                                 
   PMA 300 + ,                                                 
   PMA 380 + ,                                                 
<-> PMTAB                                                      
HERE                                                           
26 C, 56 C, 86 C, C6 C, <-> COLPFT                             
HERE                                                           
1C C, 5C C, 8C C, CC C, <-> COLPMT                             
                                                               
                                                               
                                                               
                                                               
;S                                                             
( PLAYER MAPS )                                                
HERE                                                           
 00 C, 00 C, 00 C, 0F C, ( 0 )                                 
 00 C, 00 C, 00 C, 00 C,                                       
 00 C, 00 C, 00 C, 00 C, ( 1 )                                 
 08 C, 04 C, 04 C, 02 C,                                       
 00 C, 00 C, 00 C, 00 C, ( 2 )                                 
 10 C, 20 C, 20 C, 40 C,                                       
 00 C, 00 C, 00 C, F0 C, ( 3 )                                 
 00 C, 00 C, 00 C, 00 C,                                       
 40 C, 20 C, 20 C, 10 C, ( 4 )                                 
 00 C, 00 C, 00 C, 00 C,                                       
 02 C, 04 C, 04 C, 08 C, ( 5 )                                 
 00 C, 00 C, 00 C, 00 C,                                       
 00 C, 00 C, 00 C, 00 C, ( 6 )                                 
 00 C, 00 C, 00 C, 00 C,    ;S                                 
( PLAYER MAPS CONT   )                                         
 24 C, 24 C, 18 C, FF C, ( 7 )                                 
 18 C, 18 C, 24 C, 24 C,                                       
 00 C, 42 C, 24 C, 7E C, ( 8 )                                 
 18 C, 24 C, 42 C, 00 C,                                       
 24 C, 24 C, 18 C, 7E C, ( 9 )                                 
 18 C, 18 C, 24 C, 24 C,                                       
 00 C, 00 C, 00 C, 18 C, ( 10 )                                
 18 C, 00 C, 00 C, 00 C,                                       
 <-> CRMAP                                                     
0 VAR ANONB ( ANIMATION ON )                                   
: ANON 1 ANONB ! ;                                             
;S                                                             
                                                               
                                                               
                                                               
 HERE                                                          
 00 C, 00 C, 00 C, 18 C,                                       
 18 C, 00 C, 00 C, 00 C, ( 0 )                                 
 00 C, 00 C, 3C C, 3C C,                                       
 3C C, 3C C, 00 C, 00 C, ( 1 )                                 
 00 C, 7E C, 7E C, 7E C,                                       
 7E C, 7E C, 7E C, 00 C, ( 2 )                                 
 FF C, FF C, FF C, FF C,                                       
 FF C, FF C, FF C, FF C, ( 3 )                                 
 FF C, FF C, FF C, E7 C,                                       
 E7 C, FF C, FF C, FF C, ( 4 )                                 
 FF C, FF C, C3 C, C3 C,                                       
 C3 C, C3 C, FF C, FF C, ( 5 )                                 
 FF C, 81 C, 81 C, 81 C,                                       
 81 C, 81 C, 81 C, FF C, ( 6 )                                 
 ;S                                                            
( INTERRUPT CRMAP ROUTINES )                                   
 0 C, 0 C, 0 C, 0 C,                                           
 0 C, 0 C, 0 C, 0 C, ( 7 )                                     
 <-> MAPMASK                                                   
CRMAP VAR CRMADDR                                              
CODE CSOURCE ( ASSUMES X )                                     
 CLC, MAPMOVE ,X LDA,                                          
 .A ASL, .A ASL, .A ASL,                                       
 CLC, CRMADDR ADC, F9 STA,                                     
 0 # LDA, CRMADDR 1+ ADC,                                      
 FA STA, CLC, RTS, END-CODE                                    
                                                               
: GCMD WCHSET 3 + @ IF 0 WCHSET 3 + C! ELSE                    
                       8 WCHSET 3 + C! THEN ;                  
;S                                                             
                                                               
( CDEST CCANDM VBISMASK VBIMASKTAB )                           
MAPMASK VAR MMADDR 0 VAR VBISMASK  1 VAR VBIMAPMASK            
HERE  ( PAIRS: MASKVAL , STOVAL )                              
 0 C,  2 C, 0 C, 04 C, 0 C, 06 C,                              
 0 C,  8 C, 0 C, 0C C, 0 C, 10 C,                              
 1 C, 10 C, 3 C,  C C, 7 C, 10 C, F C, 10 C, <-> VBIMASKT      
CODE CDEST2 CLC, LOCK LDA, 0= IF,                              
 MAPX ,X LDA,                                                  
 FB STA, MAPX 1+                                               
,X LDA, FC STA, CLC, THEN, RTS, END-CODE                       
CODE CANDM ( AND MASK ADDR )                                   
 7 # LDA, SEC, MAPCDT ,X SBC,                                  
 CLC, .A ASL, .A ASL, .A ASL,                                  
 MMADDR ADC, F7 STA,                                           
 0 # LDA, MMADDR 1+ ADC,                                       
 F8 STA,  CLC, RTS, END-CODE ;S                                
( INTRPT CODE CHAR ANIMATION  )                                
CRMAP 7 8 * + <-> SRCBASE 14 <-> CLKLO                         
WCHSET 3F 8 * + <-> DESTBAS                                    
: ANOFF 0 ANONB ! 8 0 DO                                       
  SRCBASE I + C@ DESTBAS I +                                   
  C! LOOP ;                                                    
CODE ANIM                                                      
 ANONB LDA, 0= NOT IF, CLKLO LDA, 3 # AND, 0= IF,              
 CLKLO LDA,  1C # AND,                                         
 .A ASL,  TAY,                                                 
 7 # LDX, BEGIN,                                               
 SRCBASE ,X LDA,                                               
 MAPMASK ,Y AND, DESTBAS ,X                                    
 STA, DEX, INY, FF # CPX,                                      
 0= UNTIL,  THEN,                                              
 THEN, CLC, RTS, END-CODE  ;S                                  
( INTRPT CODE VBISND WRTP )                                    
CODE VBISND ( VERTICAL BLANK )                                 
 VBION LDA, 0= NOT                                             
 IF, ( ' ANIM JSR, )                                           
      8 # LDX, BEGIN, DEX, DEX,                                
 ' SETVOL JSR,                                                 
 ' DECCDT JSR,                                                 
 0 # CPX, 0= UNTIL,                                            
 CLC, THEN,                                                    
 CLC, RTS, END-CODE                                            
CODE WRTP ( WRITE PLAYERS ) LOCK LDA, 0= IF,                   
 7 # LDY, BEGIN,                                               
 F9 )Y LDA, F7 )Y AND,                                         
 FB )Y STA,                                                    
 DEY, FF #  CPY, 0=                                            
 UNTIL, THEN,  CLC, RTS, END-CODE ;S                           
( INTERRUPT ROUTINES VBI     )                                 
CODE VBI ( VERTICAL BLANK )                                    
 CLKLO LDA, VBISMASK AND, 0= IF, ' VBISND JSR, THEN,           
 ' ANIM JSR,                                                   
 ' RPOTS JSR,  CLKLO LDA,                                      
 VBIMAPMASK AND, 0=   IF,                                      
 0 # LDX, BEGIN,                                               
 MAPCDT ,X LDA, 0= NOT IF,                                     
 ' CSOURCE JSR, ' CDEST2 JSR,                                  
 ' CANDM JSR, ' WRTP JSR,                                      
 ' DECMAP JSR, THEN,                                           
             INX, INX,                                         
 8 # CPX, 0=   UNTIL,                                          
 THEN,  ' UPDHS JSR,  CLC,                                     
 E462 JMP, END-CODE ( SYS VBI RETURN)                          
;S                                                             
( ?SPEED  ?SPEED2 CNFRM     )                                  
: ACMD ANONB @ IF ANOFF ELSE ANON  THEN ;                      
: ?SPEED2  02FC C@ FF = IF                                     
    ELSE KEY DUP 2F > IF                                       
       DUP 3A < IF                                             
         39 SWAP - 2*  VBIMASKT + DUP C@ DUP  VBISMASK C!      
         1 MAX VBIMAPMASK C!  1+ C@ STO !                      
        ELSE DROP THEN                                         
       ELSE DROP THEN THEN ;                                   
: ?SPEED                                                       
   02FC C@ FF = IF ELSE  02FC C@ 38 = IF KEY DROP ACMD THEN    
   02FC C@ >R R 17 > R 20 < AND                                
              R 2F > R> 36 < AND OR                            
   IF ?SPEED2 THEN THEN ;                                      
: PROMPT2 P"  ."                     " ;                       
: CNFRM KEY D = DUP IF ELSE PROMPT2 THEN ; ;S                  
( TRIG AND FREQ TABLES CRMAP .GWADR MOVE8 )                    
HERE                                                           
 04 C, 08 C, 40 C, 80 C,                                       
 <-> TRIGMASK                                                  
: ?START 0 4 0 DO I TRIGMASK +                                 
  C@ PORTA C@ AND 0= OR LOOP                                   
  ?TERMINAL 7 AND OR ;                                         
: ?CLEAR BEGIN ?START 0= UNTIL                                 
  ;                                                            
: MOVE8 ( TO FROM )                                            
 SWAP 8 CMOVE ;                                                
                                                               
;S                                                             
                                                               
                                                               
                                                               
( UPDCRLOC ANON ANOFF )                                        
: UPDCRLOC  ( WORM )                                           
 DUP .XPOS 8* 28 +                                             
 PWDTH 0= IF 4 + ENDIF OVER                                    
 .YPOS 1 AND IF 4 + ENDIF                                      
 2DUP !HPOS                                                    
 W# HPSHAD +  C!                                               
 DUP .YPOS 4*  A + !VPOS ;                                     
;S                                                             
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
( UPDMAP UPDMV ZRMAP ZMAP )                                    
: ZRMAP ( 2*W# -> )                                            
  DUP MAPCDT + 0 SWAP !                                        
  MAPX + @                                                     
  30 CRMAP + MOVE8 ;                                           
: ZMAP ( W )  DROP 2*W# ZRMAP ;                                
: UPDMAP ( W ) 2*W#                                            
   >R R ZRMAP                                                  
 .VPOS PMTAB R + @ +                                           
 MAPX R +  1 LOCK C! !                                         
 0 LOCK C! 7 MAPCDT R> + ! ;                                   
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
                                                               
                                                               
                                                               
                                                               
( KILLW )     HEX                                              
 : KILLW ( WORM -> )                                           
 0 VBION ! DUP A !CRIX                                         
 XSND .WRM# COLPF0 +                                           
 FF 50 DO                                                      
 120 I - 10 / OVER C@ F0 AND OR                                
  OVER C!                                                      
 I I 50 - 0 MAX DO                                             
   5 F I I 3 AND SOUND 8 +LOOP                                 
 STOP IF LEAVE ENDIF                                           
 8 +LOOP                                                       
 5 OVER C@ F0 AND OR SWAP C!                                   
 XSND 1 VBION ! ;                                              
                                                               
                                                               
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
  DUP UPDCRLOC DUP UPDMV                                       
  DUP UPDMAP                                                   
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
( DLH SAVELIST BUILDER ) HEX                                   
230 CONSTANT DLH                                               
D404 CONSTANT HSCROL                                           
DLH @ CONSTANT SAVDLH                                          
SAVDLH 4 + @ <-> SCRMEM                                        
: RESTORE SAVDLH DLH !                                         
 22 DMACTL C! 0 GRACTL C!                                      
 E0 2F4 C! 28 COLPF0 C! 0 COLPF1 C!                            
 C COLPF2 C! 46 COLPF3 C!                                      
5 0 DO ( CLEAR PM REGISTRS )                                   
  0 D00D I + C! LOOP ;                                         
                                                               
                                                               
                                                               
                                                               
;S                                                             
( DISPLAY LIST BUILDER ) HEX                                   
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
HERE                                                           
1 C, 2 C, 4 C, 8 C, 10 C, 20 C,                                
<-> TMASK                                                      
;S                                                              
                                                               
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
: ORCHARVEC ( CHARADDR VECADR )                                
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
: DALLY FF 0 DO I DROP LOOP ;                                  
: RAND D20A C@ ;                                               
                                                               
;S                                                             
( CHSET BUILD WDISPLAY  )                                      
: CLEARF WDATA #H #V * 0 FILL ;                                
: BUILDCHSET ( CHBASE )                                        
  DUP 200 0 FILL                                               
40 0 DO                                                        
 I ORCHAR   LOOP  DROP  8 WCHSET 3 + C! ;                      
: IWDISPLAY 2A DMACTL C!                                       
02 GRACTL C!   WDL DLH !                                       
C0 NMIEN C! GPR C@ GPRIOR C!                                   
PMA hi PMBASE C!                                               
C 0 DO 0 I D000 + C! LOOP                                      
4 0 DO PWDTH D008 I + C!                                       
COLPFT I + C@ COLPF0 I + C!                                    
COLPMT I + C@ COLPM0 I + C!                                    
LOOP WCHSETH CHBAS C! ;  ;S                                    
                                                               
( [X] [Y] SCALE )                                              
: [X]   ( X -> X  CLIPS )                                      
DUP #DH-1 H0 +  > IF #DH - ELSE                                
DUP H0 < IF #DH + ENDIF ENDIF ;                                
: [Y]  ( Y -> Y CLIPS )                                        
DUP #DV-1 V0 + > IF #DV - ELSE                                 
   DUP V0 < IF #DV + ENDIF                                     
 ENDIF ;                                                       
HERE                                                           
 FF C, E3 C, BF C, AA C,                                       
 97 C, 7F C, 71 C, 5F C,                                       
 57 C, 4B C, 3F C, 38 C,                                       
 2F C, 2A C, 25 C, 1F C,                                       
 25 C, 1F C, <-> SCALE                                         
                                                               
;S                                                             
( SOUND BSND MSND PWORM )                                      
 : BSND ( WORM --> ) DROP                                      
 2*W# DUP FRQT + FF SWAP !                                     
 CDT + 10 SWAP ! ;                                             
C VAR FV 4 VAR BV ( FOR,BK VOL)                                
: MSND ( W M )  0 AUDCTL C! 3  232 ( SKCTL) C!                 
W# >R SWAP .STATUS                                             
IF R 3* + SCALE + C@ DUP R R + AUDF + C!                       
   R R + FRQT + ! STO @                                        
   R R + CDT  + ! ELSE                                         
 0 R R + FRQT + !                                              
0 R R + CDT  + ! ENDIF R> DROP ;                                
: PWORM 1 VBION !                                              
 3F 0 DO DUP DUP I                                             
.MOVE MSND 8 WAIT  LOOP                                        
DROP XSND 0 VBION ! ;  ;S                                      
( [MOVE] DECTMO GXINC XY! X! Y! ^ )                            
: [MOVE] ( WORM M -> WORM M )                                  
 DUP 5 > IF OVER BSND ELSE                                     
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
: ?SPACE 02FC C@ 3F AND 21 = IF KEY 20 =                        
  ELSE 0 THEN ;                                                
: ?NOTSP 02FC C@ DUP FF = SWAP 3F AND 21 = OR                   
          IF 0 ELSE KEY THEN ;                                 
: ?CR 02FC C@ 3F AND 0C =  IF KEY 0D = ELSE 0 THEN ;            
: MOVEOK ( WORM M -> FLAG   )                                  
  TMASK + C@ SWAP .VAL AND 0= ;                                
: DETRACT 0 4D C! ;                                            
: PAUSEP P" ." PAUSED: HIT ESC    " ;                          
: ?PAUSE  02FC C@ 1C = IF  PAUSEP KEY DROP                     
    BEGIN 02FC C@ FF = IF 0 ELSE KEY 3F AND 1B = THEN          
    ?TERMINAL OR UNTIL PROMPT2 THEN ;                          
: TIMEOUT ( MOVE --> MOVE F )  ?PAUSE                          
  W# ( DUP BLINK ) ?SPEED                                      
  TRIGMASK + C@ PORTA C@ AND 0= ?NOTSP OR STOP OR ;  ;S        
( SETTIME NXTMOVE CLRKBD   )                                   
0 VAR KEYPOT                                                   
: SETTIME ( WORM )                                             
  0 KEYPOT !                                                   
 0 21E ! FF 22C C! DUP .TMO                                    
 21E !  DROP 2*W# >R                                           
 R R + SCALE + C@                                              
 R FRQT + !                                                    
 STO @ R> CDT + ! ;                                            
: NXTMOVE ( MOVE -> MOVE )                                     
 NMOVE + C@ 5 MIN 0 MAX ;                                      
: CLRKBD FF 02FC C! ;                                          
                                                               
                                                               
                                                               
  ;S                                                           
: =OR ROT >R R = SWAP R> = OR ; ;S  ( CASES   )                
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
: FIXL0  ( W# )                                                
  DUP 40*  ( W# MASK )                                         
  SWAP 5 * SCRMEM +                                            
  DUP 5 + SWAP                                                 
  DO I C@ 3F AND OVER OR I C!                                  
  LOOP DROP ;                                                  
: FIXL1 ( W# )                                                 
  DUP 40* ( W# MASK )                                          
  SWAP 5 * SCRMEM 28 + +                                       
  DUP 5 + SWAP                                                 
  DO I C@ 3F AND OVER OR I C!                                  
  LOOP DROP ;                                                  
: FIXL2 ( W# ) 40* SCRMEM 78 +                                 
  SCRMEM 50 + DO I C@ 3F AND                                   
  OVER OR I C! LOOP DROP ;  ;S                                 
( ADDONE )                                                     
: ADDONE ( ADDR )                                              
  >R BEGIN                                                     
   R C@ 3F AND DUP F 18 RANGE                                  
     IF 1+ R C! 1 ELSE                                         
  DUP  19 = IF DROP 10 R C! R> 1- >R 0 ELSE                    
               DROP 11 R C! 1 THEN THEN                        
    UNTIL R> DROP ;                                            
                                                               
                                                               
 ;S                                                            
                                                               
                                                               
                                                               
                                                               
                                                               
(  KEYPOT GKEYPOT X! Y! XY! )                                  
0 VARIABLE TBUF 10 ALLOT 0 VAR CCNT                            
HEX 0 VAR SEL ( SELECTED WORM )                                
: HIL2 SEL @ FIXL2 ;                                           
: GKEYPOT (  -> N )                                             
  ?SPACE KEYPOT +! KEYPOT @ ;                                  
: PROMPTM P" ." PADDLE/SPACE&RETURN"  ;                         
: BMPCNT DUP CCNT @ 3 > IF 7E EMIT EMIT                         
ELSE 1 CCNT +! EMIT THEN  CCNT @ 1- TBUF + C! ;                
: GTBUF 0 CCNT ! BEGIN KEY DUP                                  
 30 61 RANGE IF BMPCNT HIL2 0 ELSE DUP                         
 61 7A RANGE IF 20 - BMPCNT HIL2 0 ELSE DUP                    
 7E = IF EMIT CCNT @ 1 - 0 MAX CCNT ! 0 ELSE DUP               
 D =  IF DROP 1 ELSE                                           
 DROP 0  10 10 BUZZ  THEN THEN THEN THEN                       
 UNTIL ;  ;S                                                   
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
 : GPOT ( WORM --> INDX )                                      
   .WRM#  DUP POT0 + C@ MAXPOT = IF DROP GKEYPOT ELSE          
     CPOT + C@ E6 SWAP -                                       
     E6 #LEGAL @ 2 * /                                         
     /  GKEYPOT + THEN                                         
   #LEGAL @ 1 MAX MOD                                          
   LEGAL + C@  0 MAX 5 MIN ;                                   
 : TRIGCLR ( MV WORM - MOVE )                                  
    >R W# TRIGMASK + C@                                        
   BEGIN DUP PORTA C@ AND                                      
   R DUP GPOT !CRIX R UPDMV                                    
   W# DUP + MAPCDT + >R                                        
   R C@ 0= IF 7 R C! ENDIF R>                                  
   DROP UNTIL                                                  
   DROP DROP R> .CRIX ; ;S                                     
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
 1 VAR CMDOK : CHKST CMDOK @ stat @ 1 = AND CMDOK ! ;          
 168 CONSTANT VTOC#  80 CONSTANT SECSZ                         
 0 VARIABLE FBUF SECSZ ALLOT                                   
 0 VARIABLE VTOC SECSZ ALLOT 0 VARIABLE DBUF SECSZ ALLOT       
169 CONSTANT DIR# 0 VARIABLE FILE#                             
: RDSEC ( READ DOS SEC: BUF SEC# --> )                         
  1- 1 1 R/W-SECS CHKST ;                                      
: WDSEC ( BUF SEC# )                                           
  1- 1 0 R/W-SECS CHKST ;                                      
: RVTOC VTOC VTOC# RDSEC ;                                     
: WVTOC VTOC VTOC# WDSEC ;                                     
: RDIRSEC ( N ) DBUF SWAP DIR# + RDSEC ;                       
: WDIRSEC ( N ) DBUF SWAP DIR# + WDSEC ;                       
: VDOSDISK RVTOC VTOC 1+ @ 2C3 = DUP IF ELSE 0 CMDOK ! THEN ;  
;S                                                             
: GETNEWMOVE DUP 7 !CRIX CLRKBD                                
 XSND DUP SETTIME 2*W#                                         
>R DUP UPDCRLOC                                                
DUP CHKMOVES  1 PSCAN C!                                       
DUP UPDMAP                                                     
6 BEGIN DROP DUP GPOT                                          
OVER .CRIX OVER = IF ELSE                                      
2DUP !CRIX                                                     
OVER UPDMV  ENDIF R MAPCDT +                                   
C@ 0= IF 7 MAPCDT R + ! ENDIF                                  
TIMEOUT UNTIL OVER TRIGCLR                                     
0 COLBK C! DETRACT [MOVE]                                      
 SWAP OVER SWAP                                                
DUP [.VAL] ROT !MOVE 0 PSCAN C!                                
1 VBION ! R> DROP ; ;S                                         
                                                               
( HILITE DELITE          )                                     
HERE  38 C, 60 C, 88 C, B0 C, <-> MHP                          
: MVHILTE ( WORM ) 2E DMACTL C! 3 GRACTL C!                    
 FF PMA 1F2 + C! .WRM# DUP MHP + 4 0 DO DUP C@ I 6 * + D004    
  I + C! LOOP DROP FF D00C C! ( PROMPTM ) FIXL2 ;              
: MVDELITE (  ) 2A DMACTL C! 2 GRACTL C!                       
 00 PMA 1F2 + C! 4 0 DO 0 D004 I + C! LOOP PROMPT2 ;           
: HILITE ( WORM )                                              
.WRM# DUP COLPF0 + SWAP                                        
 COLPFT + C@  F0 AND 8 OR SWAP C! ;                            
: DELITE (    )                                                
 4 0 DO COLPFT I + C@ F0                                       
AND 4 OR COLPF0 I +                                            
C! LOOP ;                                                      
: XLITE ( ALL COLORS OFF )                                     
4 0 DO 0 COLPF0 I + C! LOOP ; ;S                               
(  GETMOVE   )                                                 
: GETMOVE ( WORM -> MOVE )                                     
  DUP [.VAL] OVER SWAP .MOVE                                   
  DUP                                                          
  6 <  IF ELSE DUP ( W M )                                     
  GETNEW# = IF DROP DUP HILITE DUP MVHILTE                     
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
 .WRM# DUP 5 * 3 + SCRMEM +                                    
  ADDONE DUP  FIXL0 DUP +                                      
  WSCR + 1 SWAP +! ;                                           
;S                                                             
( UPDCNODE INSTALLVBI INSTALLHBI INITMEM   )                   
: UPDCNODE ( WORM MOVE )                                       
  2DUP !CRIX                                                   
  OVER .VAL SWAP TMASK + C@                                    
  OR DUP 3F AND 3F = IF                                        
  OVER 8 !CRIX OVER UPDSCR                                     
  ENDIF !VAL ;                                                 
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
  DUP 6 < IF  OVER ZMAP                                        
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
 1 VBION ! 0 QFLG !                                            
 DUP 0 DO I GWORM UPDCRLOC LOOP                                
 BEGIN 0 OVER 0 DO ( XSND ) ?PAUSE I SETW#                     
BEGIN I DUP + CDT + C@ 0= UNTIL                                 
  I GWORM MAKEMOVE + LOOP ?SPEED                               
  ( DUP 1+ DEN @ * STO ! )  0=                                 
  CONT 0= OR                                                   
UNTIL DROP XSND ( 0 VBION ! ) ;                                
: CLEARPM  (  )  280 0                                         
 DO 0 PMA 180 + I + C! LOOP ;                                  
;S                                                             
( DUMMY SCRLON SCRLOFF FIXDUMMY )                              
                                                               
  ' DUMMY 3 + VAR FSADD                                        
 FSADD @  ' DUMMY 2 + C@ + 1A - VAR LSADD                      
07 VAR CURSCROLL 0 VAR SADD 1 VAR SCRLFLAG                      
: FIXDUMMY LSADD @ 1A +  FSADD @                                
      DO I C@ 20 - I C! LOOP ;                                 
: SCRLON 0 SCRLFLAG C! 56 WDL 40 + C! FSADD @ WDL 41 + ! ;      
: SCRLOFF 1 SCRLFLAG C! 46 WDL 40 + C! SCRMEM 50 + WDL 41 + ! ; 
                                                               
                                                               
;S                                                             
( PSCORE  SCORE2         )                                     
1E6 VAR FTIME 0 VAR WTIME                                      
: SCORE2 ( #PLAYERS )                                          
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
 0 D208 C! 3 D20F C! ( INIT SOUND )                            
  CLEARF  CLEARPM ( WDISPLAY )                                 
 #WORMS @ 0 DO I GWORM DUP                                     
 INITPOS ( DUP INITMOVES )                                     
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
: WLDISPLAY                                                    
 16 0 DO 0 I SCRMEM + C! LOOP                                  
 8 0 DO 0 I WSCR + ! 2 +LOOP ;                                 
: IDISPLAY CLEARPM CLEARF ( WDISPLAY ) ( LOGO ) ;  ;S          
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
 40 0 DO DUP I FBUF I + C@ VMOVE I SWAP !MOVE LOOP DROP        
 R> DROP ;                                                     
: WWORM ( WORM )                                               
 40 0 DO DUP I .MOVE FBUF I + C! LOOP DROP ;  ;S               
( INTROGAME  )                                                 
                                                               
: INTROGAME                                                    
  CLEARPM CLEARF ( LFIELD )                                    
 #WORMS @ 0 DO I GWORM DUP                                     
 CONT IF  RAND 3 AND IF                                        
  DUP 3 !KIND DUP INITMOVES DUP RANDOMIZE                      
  ELSE DUP 2 !KIND DUP INITAUTO THEN                           
 ELSE                                                          
    DUP 0 !KIND ENDIF                                          
 INITPOS ( DUP INITMOVES )                                     
 DUP ZMAP INITSTAT                                             
 LOOP CONT IF WLDISPLAY                                        
 #WORMS @ DUP GAME SCRLOFF PROMPT2 SCORE2  ENDIF               
 #WORMS @ 0 DO I GWORM 0 !SCORE                                
  LOOP  ; ;S                                                   
( SUMO  INITIALIZATION )                                       
 0 VAR CSBUILT                                                 
: SUMO  ( INITIALIZE WORLD )                                   
 0 COLBK C! DELITE ( WDL SETHSCRL  )                           
 CSBUILT @ IF ELSE CLEARF                                      
 WCHSET BUILDCHSET                                             
 1 CSBUILT ! ENDIF                                             
 0 QFLG !                                                      
 ?TERMINAL 0= IF                                               
 BEGIN ( LOGO ) INTROGAME SCRLOFF                              
 STOP UNTIL ENDIF                                              
 WLDISPLAY  FF 02FC C!                                         
0 GWORM 1 !KIND 1 GWORM 2 !KIND                                 
2 GWORM 3 !KIND 3 GWORM 0 !KIND  ; ;S                           
                                                               
                                                               
( SEL ILINE DELINE     )                                       
: FIXLIN ( W# -> )                                             
  DUP FIXL0 FIXL1 ;                                            
                                                               
: HILINE  ( -> )                                               
 SEL @ COLPF0 + DUP C@ F0 AND                                  
 A OR SWAP C! HIL2 ;                                           
: DELINE ( -> )                                                
 SEL @ COLPF0 + DUP C@ F0 AND                                  
 4 OR SWAP C! ;                                                
: FIXCOL ( -> )                                                
 4 0 DO COLPFT I + C@ COLPF0 I                                 
        + C! LOOP ;                                            
;S                                                              
                                                               
                                                               
( COMMAND SHELL SUPPORT HBI1 FXHBI )  HEX                       
: CINC ( MAX VAL -> NEWVAL)                                     
 1+ SWAP OVER = IF DROP 0                                      
ENDIF ;                                                        
                                                               
CODE HBI1 PHA,                                                 
  2 # LDA, WSYNC STA,                                          
  D01A STA, ( BKGRND LUM )                                     
  CURSCROLL LDA, HSCROL STA,                                   
  E0 # LDA,  CHBASE STA,                                       
  PLA, RTI, END-CODE                                           
( : FXHBI ' HBI1 INSTALLHBI  ;   )                              
4153 VARIABLE SAMEN 4D C, 45 C,                                
: NAMESAME ( W ) .NAME DUP 8 BLANKS SAMEN SWAP 4 CMOVE ;       
;S                                                              
                                                               
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
                                                               
                                                               
                                                               
( COMMAND SHELL WORM OUTPUT )                                   
: TDED    ."  ----" ;                                           
: TNEW    ."  NEW " ;                                           
: TMYST   ."  WILD" ;                                           
: TOLD    ."  SAME" ;                                           
: TSMART  ."  AUTO" ;                                           
: CURW SEL @ GWORM ;                                            
: TNAMED 20 EMIT                                                
   CURW 4 0 DO DUP .NAME I + C@ EMIT LOOP DROP ;               
: VKINDCASE  TDED TNEW  TSMART TMYST  TNAMED NOOP ;             
: KINDCASE  DUP + ' VKINDCASE + @ EXECUTE ;                     
: TYKIND DUP 5 * 1 XY! DUP GWORM                                
 .KIND KINDCASE FIXL1 ;                                        
;S                                                             
                                                               
                                                               
( WOPTION WSELECT NOP SHOWALL PROMPT HI LO )                    
: PROMPT P" ."  SELECT/OPTION/START"  HILINE ;                  
: WOPTION 5 CURW .KIND CINC                                     
 CURW SWAP !KIND                                               
 SEL @ TYKIND                                                  
 SEL @ FIXL1  PROMPT ;                                         
: WSELECT DELINE                                                
 1 SEL +! SEL @                                                
 MAX#W  = IF 0 SEL !                                           
  ENDIF PROMPT ;                                               
: NOP ;                                                         
: SHOWALL SEL @  MAX#W 0 DO I SEL !                             
I TYKIND LOOP SEL ! HILINE ;                                   
: DROPKEY 02FC C@ FF = IF ELSE KEY DROP THEN ;                  
: HI hi ;  : LO lo ;                                            
;S                                                             
( DOS EMULATION 2 )                                            
 8040 VARIABLE VBITMASK 20 C, 10 C, 08 C, 04 C, 02 C, 01 C,    
: SECFREE? ( SEC# )                                            
  8 /MOD A +  VTOC + C@ SWAP VBITMASK + C@ AND ;               
: MARKSEC ( SEC# ) 8 /MOD A + VTOC + >R R C@                   
        SWAP VBITMASK + C@ XOR R> C! -1 VTOC 3 + +! ;          
FF VARIABLE MATCHFLAG                                          
: MATCH ( PTR-STR PTRDIR --> PTRDIR FLAG )                     
 FF MATCHFLAG !                                                
0B 0 DO 2DUP I + 5 + C@ SWAP I + C@ =                          
        MATCHFLAG @ AND MATCHFLAG ! LOOP                       
    MATCHFLAG @ IF SWAP DROP 1  ELSE 2DROP 0 THEN ;            
0 VAR DSTATE                                                   
: DOFF 022F C@ DSTATE C! 0 022F C!  0 D400 C!                  
 5 0 DO 0 D00D I + C! LOOP 0 D01D C! ;                         
: DON C0 D40E C!  DSTATE C@ 022F C! 3 D01D C!  ; ;S            
( DOS EMULATION 2 )                                            
 FF VARIABLE LASTDIRSEC                                        
: DENTVALID ( PTR ) C@ 40 AND ;                                
: GDIR ( F# --> PTR  ) 8 /MOD                                  
   DUP LASTDIRSEC @ = IF DROP ELSE                             
   DUP RDIRSEC LASTDIRSEC ! THEN  10 * DBUF + ;                
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
;S                                                              
( DOS EMULATION 4 )                                            
 0 VARIABLE F#                                                 
: FIXTBUF                                                      
    57 TBUF 8 + C! 52 TBUF 9 + C! 4D TBUF A + C!  ;            
: TBUFINIT B 0 DO 20  TBUF I + C! LOOP ;                       
: GNAME P" PROMPT2  P" DON                                     
  ."  FILE NAME:"  HIL2 TBUFINIT GTBUF FIXTBUF DOFF ;          
: MNAME (  --> FLG & PTR ) 0                                   
  40 0 DO TBUF I GDIR MATCH IF SWAP DROP 1  I F# ! LEAVE THEN  
             LOOP ;                                            
: GNF# ( GETNEW FILE #: -> FLAG AND SECTOR )                   
  FF  LASTDIRSEC !  FF                                         
  40 0 DO I GDIR  DENTVALID 0= IF DROP I I F# ! LEAVE THEN     
       LOOP DUP FF = IF DROP 0 ELSE 1 THEN ;                   
: NOK ( NAMEOK ) 0 4 0 DO 20 TBUF I + C@ = 0= OR LOOP ; ;S     
                                                               
( DOS EMULATION F#PTR PTRF# )                                  
: READF  DOFF 1 CMDOK !                                        
    VDOSDISK (  FXHBI ) IF                                     
    GNAME NOK IF  MNAME IF ( DUP PDENTRY ) 3 + @               
    FBUF SWAP RDSEC ELSE                                       
     P" ."  FILE NOT FOUND     " 0 CMDOK ! THEN THEN           
   ELSE P" ."  INSERT A DOS DISK   "  THEN DON ;               
: WRITEOF ( PTR --> )                                          
  ( DUP PDENTRY ) 3 + @ FBUF SWAP WDSEC ;                      
: UPDDIR LASTDIRSEC @ WDIRSEC ;                                
                                                               
: PTRF# ( PTR -> F# )                                          
   DBUF - 10 / ;                                               
: F#PTR ( F# -> PTR ) 7 AND                                    
   10 * DBUF + ;                                               
;S                                                              
( DOS EMULATION )                                              
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
: WRITEF DOFF 1 CMDOK !                                        
   VDOSDISK (  FXHBI ) IF                                      
    GNAME NOK IF MNAME IF FIXFBUF WRITEOF                      
      ELSE                                                     
       GNF# IF F#PTR FIXFBUF  WRITENF                          
       ELSE P" ."  DIRECTORY FULL       " 0 CMDOK ! THEN       
      THEN THEN                                                
   ELSE P" ."  INSERT A DOS DISK  "  THEN DON ;                
: SAVEF 1 CMDOK !                                              
    DOFF VDOSDISK ( FXHBI ) IF MNAME IF FIXFBUF WRITEOF        
    ELSE GNF# IF F#PTR FIXFBUF WRITENF                         
      ELSE P" ."  DIRECTORY FULL"  0 CMDOK ! THEN              
    THEN ELSE P" ."  INSERT A DOS DISK    " THEN DON ;         
                                                               
;S                                                             
( WRPWDL BMPWDL NWORM FINCMD DOREAD )                          
CODE WRPWDL WDL 41 + LDA, LSADD CMP, 0= IF, WDL 42 + LDA,      
       LSADD 1+ CMP, 0= IF, FSADD LDA, WDL 41 + STA, FSADD 1+  
       LDA, WDL 42 + STA, THEN, THEN, CLC, RTS, END-CODE       
CODE BMPWDL CLC, WDL 41 + LDA, 1 # ADC, WDL 41 + STA, CS IF,   
     0 # LDA, WDL 42 + ADC, WDL 42 + STA, THEN, ' WRPWDL JSR,  
     CLC, RTS,  END-CODE                                       
: NWORM ( W )                                                  
  4 0 DO DUP .NAME I + TBUF I + C@ SWAP C! LOOP                
    4 !KIND ;                                                  
: FINCMD CMDOK @ IF PROMPT ELSE stat @ DUP 1 = IF DROP ELSE     
   P" PROMPT2 P" ."  DISK ERROR: " . THEN THEN ;               
: DOREAD P" ."  LOAD? <RETURN>      "  HIL2 CNFRM               
  IF READF CMDOK @ IF CURW RWORM THEN THEN FINCMD ;            
: DO? P" ."  LOAD/SAVE/UPDT/DIR  " HIL2 ;                       
;S                                                             
( COMMANDS )                                                   
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
: DOWRITE P" ."  SAVE? <RETURN>       " HIL2                    
   CNFRM IF CURW WWORM WRITEF CMDOK @ IF CURW NWORM THEN THEN  
   FINCMD ;                                                    
: SNAME  TBUFINIT CURW 4 0 DO DUP .NAME I + C@                  
        TBUF I + C! LOOP DROP  FIXTBUF ;                       
: DOSAVE P" ."  UPDATE" TNAMED ." ?<RET.>  " HIL2               
   CNFRM IF SNAME CURW WWORM SAVEF THEN  FINCMD ;              
: ?CMD ?SPEED 02FC C@ FF = IF ELSE KEY DUP                      
 4C 6C =OR IF DOREAD ELSE DUP                                  
 53 73 =OR IF DOWRITE ELSE DUP                                 
 55 75 =OR IF DOSAVE ELSE DUP                                  
 47 67 =OR IF GCMD ELSE DUP                                    
 3F 2F =OR IF DO? ELSE DUP                                     
 44 64 =OR IF DODIR  THEN THEN THEN THEN THEN THEN DROP        
 ( FXHBI ) SHOWALL THEN ; ;S                                   
( MAIN LOOP )                                                  
: WSTART SETOPTS ( WLDISPLAY ) FIXCOL WCHSETH CHNOW C!          
 C SCRLVAL C! PROMPT2 PLAYGAME  SHOWALL                        
 PROMPT FF 02FC C!   ;                                         
: VTERMCHOICE NOP  WSTART WSELECT NOP WOPTION ;                 
: TERMCHOICE 4 MIN DUP + ' VTERMCHOICE + @ EXECUTE ;            
: ?WAITTERM                                                     
BEGIN ?START -DUP ?CMD UNTIL ;                                 
: ?NOTERM BEGIN ?TERMINAL DUP 5                                 
> SWAP 0= OR UNTIL ?TERMINAL ;                                 
: RUNLOOP SHOWALL PROMPT                                        
BEGIN ?CLEAR ( DELETE FOR PRD)                                 
?WAITTERM DETRACT        DUP 5 < IF                            
TERMCHOICE               ELSE                                  
                         DROP ENDIF                            
?NOTERM UNTIL ;       ;S                                       
( VBI EXTRAS  INSTALLEM  ) FF VAR ENDSCRL                      
CODE UPDSCRL CURSCROLL LDA, SEC, 1 # SBC,                      
CURSCROLL STA,  ENDSCRL  CMP, 0= IF,                           
' BMPWDL JSR, 7 # LDA,  CURSCROLL STA, THEN,                    
CLC, RTS, END-CODE                                             
                                                               
CODE VBIX                                                      
  ( 8 # LDA, WCHSET 3 + STA, )  CHNOW LDA, CHBASE STA,         
  SCRLFLAG LDA, 0= IF,                                         
  CLKLO LDA, 1 # AND, 0= IF, ' UPDSCRL JSR, THEN, THEN,        
  SCRLVAL  LDA, HSCROL STA,                                    
   ' VBI JMP,    END-CODE                                      
: INSTALLEM ' VBIX INSTALLVBI ' HBI1 INSTALLHBI ; ;S           
                                                               
                                                               
                                                               
( FIXINTRPTS DORTI )                                           
CODE DORTI RTI, END-CODE                                       
                                                               
                                                               
: FIXINTRPTS                                                   
 E462 INSTALLVBI                                               
 ( E7B3 INSTALLHBI ) ( NOT FOR ALL VERSIONS OF O.S. ! )        
 ' DORTI INSTALLHBI                                            
 E0 CHBAS C! E0 CHBASE ! ;                                     
                                                               
                                                               
                                                               
                                                               
                                                               
;S                                                             
                                                               
( RESET )                                                      
0 #WORMS ! HERE 58 ALLOT <-> W1 HERE 58 ALLOT <-> W2           
           HERE 58 ALLOT <-> W3 HERE 58 ALLOT <-> W4           
: GRIDGLIDERS  ANOFF C SCRLVAL C!                              
  CSBUILT @ IF ELSE FIXDUMMY THEN 0 #WORMS ! 0 SEL ! INITMEM   
   W1 INITWORM W2 INITWORM W3 INITWORM W4 INITWORM             
3 F 60 1 SOUND XSND  WCHSETH CHNOW C! 06 STO ! 8 WCHSET 3 + C! 
7D EMIT CLEARPM INSTALLEM IWDISPLAY                            
SCRLON SUMO 7D EMIT CLEARF                                     
( PROMPT ) CLEARPM  SCRLOFF                                    
4 0 DO I GWORM INITMOVES LOOP                                  
4 #WORMS ! 1 2F0 C! ( CURSOR OFF ) DECIMAL                     
( BEGIN )  RUNLOOP (  AGAIN  )                                 
RESTORE FIXINTRPTS                                             
0 2F0 C! ( CURSOR ON  ) ; DECIMAL                              
: TASK ; IS-FENCE  FINIS    (   PROF ) ;S                      
\ WORMS? COPYRIGHT (C) 1983 BY DAVID S. MAYNARD                
( WITH DISK/IO WITHOUT BUILDS  )                               
( DOES>     APRIL  3, 1983.     ) HEX                          
CODE ?TERMINAL                                                 
  D01F LDA, 7 # EOR, 7 # AND,                                  
  PHA, 0 # LDA, PUSH JMP,                                      
  END-CODE                                                     
: SOUND                                                        
  0232 C@ 07 AND                                               
  D20F C! 0 D208 C!                                            
  2 * D200 + >R >R 10 * OR                                     
  EF AND                                                       
  100 * R> OR R> ! ;                                           
: XSND                                                          
 D208 D200 DO 0 I C! LOOP ;                                    
                                                              
