\ WORMS? COPYRIGHT (C) 1983 BY DAVID S. MAYNARD                
( WITH DISK/IO WITHOUT BUILDS  )                               
( DOES>     APRIL  3, 1983.     ) HEX                          
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
HEX   HERE                                                      
A4 C,   A6 C,   A8 C,   A8 C,   A7 C,   A7 C,   A6 C,   A5 C,  
CONSTANT BASSA                                                 
HERE                                                            
8A C,   82 C,   88 C,   82 C,   81 C,                          
CONSTANT DRUMSA                                                
HERE                                                            
A5 C,   A7 C,   A7 C,   A6 C,   A5 C,                          
CONSTANT TENORA                                                
HERE                                                            
A6 C,   A8 C,   A8 C,   A7 C,   A6 C,                          
CONSTANT ALTOA                                                 
                                                               
                                                               
                                                               
                                                               
HERE                                                            
A3 C,   A2 C,   A1 C,                                          
CONSTANT BASSR                                                 
HERE                                                            
80 C,   80 C,   80 C,                                          
CONSTANT DRUMSR                                                
HERE                                                            
A4 C,   A2 C,   A1 C,                                          
CONSTANT TENORR                                                
HERE                                                            
A5 C,   A3 C,   A1 C,                                          
CONSTANT ALTOR                                                 
HERE                                                            
 0 C,   78 C,    0 C,   78 C,    0 C,    A C,   B6 C,   1E C,  
F3 C,   1E C,   D9 C,   1E C,   C1 C,   1E C,   B6 C,   1E C,  
F3 C,   1E C,   D9 C,   1E C,   C1 C,   14 C,    0 C,    A C,  
B6 C,   1E C,   F3 C,   1E C,   D9 C,   1E C,   C1 C,   1E C,  
B6 C,   1E C,   F3 C,   1E C,   D9 C,   1E C,   C1 C,   14 C,  
 0 C,    A C,   B6 C,   1E C,   F3 C,   1E C,   D9 C,   1E C,  
C1 C,   1E C,   B6 C,   1E C,   F3 C,   1E C,   D9 C,   1E C,  
C1 C,   14 C,    0 C,    A C,   B6 C,   1E C,   F3 C,   1E C,  
D9 C,   1E C,   C1 C,   1E C,   B6 C,   1E C,   F3 C,   1E C,  
D9 C,   1E C,   C1 C,   14 C,    0 C,    A C,   B6 C,   1E C,  
F3 C,   1E C,   D9 C,   1E C,   C1 C,   1E C,   B6 C,   1E C,  
F3 C,   1E C,   D9 C,   1E C,   C1 C,   14 C,                  
CONSTANT BASS                                                  
HERE                                                            
1E C,    A C,   1E C,    A C,   1E C,    A C,   1E C,    A C,  
1E C,   14 C,   1E C,    A C,   1E C,    A C,   1E C,    A C,  
1E C,    A C,   1E C,   14 C,   1E C,    A C,   1E C,    A C,  
1E C,    A C,   1E C,    A C,   1E C,    A C,   1E C,    A C,  
1E C,    A C,   1E C,    A C,   1E C,    A C,   1E C,    A C,  
1E C,   14 C,                                                  
CONSTANT DRUMS                                                 
HERE                                                            
 0 C,   78 C,    0 C,   78 C,    0 C,   78 C,    0 C,   78 C,  
 0 C,   78 C,    0 C,   78 C,   A2 C,    A C,   90 C,   14 C,  
79 C,    A C,   90 C,   14 C,   A2 C,    A C,   90 C,   14 C,  
79 C,    A C,   90 C,   14 C,   A2 C,    A C,   90 C,   14 C,  
79 C,    A C,   79 C,   1E C,   A2 C,   14 C,   90 C,    A C,  
88 C,   14 C,   A2 C,    A C,   90 C,   14 C,   79 C,    A C,  
90 C,   14 C,   A2 C,    A C,   90 C,   14 C,   79 C,    A C,  
90 C,   14 C,   A2 C,    A C,   90 C,   14 C,   79 C,    A C,  
79 C,   1E C,   A2 C,   14 C,   90 C,    A C,   88 C,   14 C,  
A2 C,    A C,   90 C,   14 C,   79 C,    A C,   90 C,   14 C,  
A2 C,    A C,   90 C,   14 C,   79 C,    A C,   90 C,   14 C,  
A2 C,    A C,   90 C,   14 C,   79 C,    A C,   79 C,   1E C,  
A2 C,   14 C,   90 C,    A C,   88 C,   14 C,                  
CONSTANT TENOR                                                 
HERE                                                            
 0 C,   78 C,    0 C,   78 C,    0 C,   78 C,    0 C,   78 C,  
79 C,    A C,   5B C,   14 C,   48 C,    A C,   5B C,   14 C,  
79 C,    A C,   5B C,   14 C,   48 C,    A C,   5B C,   14 C,  
79 C,    A C,   5B C,   14 C,   51 C,    A C,   48 C,    A C,  
51 C,    A C,   5B C,    A C,   60 C,   14 C,   5B C,    A C,  
51 C,   14 C,   79 C,    A C,   5B C,   14 C,   48 C,    A C,  
5B C,   14 C,   79 C,    A C,   5B C,   14 C,   48 C,    A C,  
5B C,   14 C,   79 C,    A C,   5B C,   14 C,   51 C,    A C,  
48 C,    A C,   51 C,    A C,   5B C,    A C,   60 C,   14 C,  
5B C,    A C,   51 C,   14 C,   79 C,    A C,   5B C,   14 C,  
48 C,    A C,   5B C,   14 C,   79 C,    A C,   5B C,   14 C,  
48 C,    A C,   5B C,   14 C,   79 C,    A C,   5B C,   14 C,  
51 C,    A C,   48 C,    A C,   51 C,    A C,   5B C,    A C,  
60 C,   14 C,   5B C,    A C,   51 C,   14 C,   79 C,    A C,  
5B C,   14 C,   48 C,    A C,   5B C,   14 C,   79 C,    A C,  
5B C,   14 C,   48 C,    A C,   5B C,   14 C,   79 C,    A C,  
5B C,   14 C,   51 C,    A C,   48 C,    A C,   51 C,    A C,  
5B C,    A C,   60 C,   14 C,   5B C,    A C,   51 C,   14 C,  
                                                               
CONSTANT ALTO                                                  
HERE CONSTANT END-ALTO                                         
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
( MUSIC TOP LEVEL TABLES ) BASE @ HEX                          
HERE BASSA , DRUMSA , TENORA , ALTOA , CONSTANT ATK            
HERE BASSR , DRUMSR , TENORR , ALTOR , CONSTANT RLS            
HERE 8 , 5 , 5 , 5 , CONSTANT ATK.CT                           
HERE 3 , 3 , 3 , 3 , CONSTANT RLS.CT                           
HERE 0 , 0 , 0 , 0 , CONSTANT MCDT                             
HERE 0 , 0 , 0 , 0 , CONSTANT  CUT                             
HERE BASS 5D + , DRUMS 29 + ,                                  
     TENOR 65 + , ALTO 8F + , CONSTANT NCUR                    
HERE BASS 5E + , DRUMS 2A + ,                                  
     TENOR 66 + , ALTO 90 + , CONSTANT NMAX                    
HERE BASS , DRUMS , TENOR , ALTO , CONSTANT NORG               
0 CONSTANT REST                                                
2 CONSTANT MTMP ( 0-PAGE PTR )                                 
BASE ! ;S                                                       
                                                               
( MUSIC BOX INTERRUPT CODE  ) BASE @ HEX                       
CODE NEWNOTE \ POINT TO NEXT [PITCH,DURATION] AND SETUP        
 NCUR ,X INC, 0= IF, NCUR 1+ ,X INC, THEN,                     
 NCUR ,X LDA, NMAX ,X CMP, 0=  IF,                             
  NCUR 1+ ,X LDA, NMAX 1+ ,X CMP, 0=                           
   IF, NORG ,X LDA, NCUR ,X STA,                               
    NORG 1+ ,X LDA, NCUR 1+ ,X STA, THEN,                      
  THEN,                                                        
 0 # LDA, CUT ,X STA, AUDC ,X STA, TAY,                        
 NCUR ,X LDA, MTMP STA, NCUR 1+ ,X LDA, MTMP 1+ STA,           
 MTMP )Y LDA, AUDF ,X STA, ( STORE NOTE )                      
 NCUR ,X INC, 0= IF, NCUR 1+ ,X INC, THEN,                     
 MTMP    INC, 0= IF, MTMP 1+    INC, THEN,                     
 MTMP )Y LDA, MCDT ,X STA,  ( STORE DURATION )                 
 RTS, END-CODE                                                 
BASE ! ;S                                                      
( MUSIC BOX INTERRUPT CODE  ) BASE @ HEX                       
                                                               
CODE DORLS                                                     
 RLS.CT ,X LDA, SEC, MCDT ,X SBC, TAY,                         
 RLS ,X LDA, MTMP STA,                                         
 RLS 1+ ,X LDA, MTMP 1+ STA,                                   
 MTMP )Y LDA, AUDC ,X STA, RTS, END-CODE                       
                                                               
CODE DOATK                                                     
 CUT ,X LDA, TAY,                                              
 ATK ,X LDA, MTMP STA,                                         
 ATK 1+ ,X LDA, MTMP 1+ STA,                                   
 MTMP )Y LDA, AUDC ,X STA, RTS, END-CODE                       
                                                               
BASE ! ;S                                                      
                                                               
( MUSIC BOX INTERRUPT CODE  ) BASE @ HEX                       
                                                               
CODE EACHTIME ( SEQUENCING AND ENVELOPE )                      
 MCDT ,X LDA, 0= IF, ' NEWNOTE JSR, THEN,                      
 NCUR ,X LDA, MTMP STA,                                        
 NCUR 1+ ,X LDA, MTMP 1+ STA, 0 # LDY,                         
 MTMP )Y LDA, REST # CMP, 0= NOT IF,                           
  MCDT ,X LDA, RLS.CT ,X CMP, CS NOT                           
   IF, ' DORLS JSR, THEN,                                      
  CUT ,X LDA, ATK.CT ,X CMP, CS NOT                            
   IF, ' DOATK JSR, THEN,                                      
  THEN,                                                        
 CUT ,X INC, MCDT ,X DEC,                                      
 CLC, RTS, END-CODE                                            
                                                               
BASE ! ;S                                                       
( MUSIC BOX INTERRUPT CODE  )  BASE @ HEX                      
CODE MUSICVBI                                                  
 8 # LDX, BEGIN, DEX, DEX,                                     
   ' EACHTIME JSR,                                             
   0 # CPX, 0= UNTIL,                                          
   E462 JMP,                                                   
   END-CODE                                                    
                                                               
 BASE !  ;S                                                    
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
( MUSIC INITILIZATION ) BASE @ HEX                             
HERE BASS 5D + , DRUMS 29 + ,                                  
     TENOR 65 + , ALTO 8F + , CONSTANT MSTRT                   
                                                               
: INITMUSIC                                                    
   MCDT 8 0 FILL                                               
    CUT 8 0 FILL                                               
   MSTRT NCUR 8 CMOVE ;                                        
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
 BASE ! ;S                                                     
                                                               
( EA   LOGO   ) BASE @ HEX                                     
HERE                                                           
FF C, C0 C, FF C, C0 C, FF C, C0 C, FF C, C0 C,                
FF C, C0 C, FF C, C0 C, FF C, C0 C, FF C, C0 C,                
FF C, C0 C, FF C, C0 C, FF C, C0 C, FF C, FF C,                
CONSTANT EASQ                                                  
HERE                                                           
03 C, 0C C, 1F C, 38 C, 3F C, 70 C, 7F C, 60 C,                
FF C, E0 C, FF C, F0 C, FF C, F8 C, FF C, FC C,                
7F C, 7E C, 7F C, 3F C, 3F C, 1F C, 0F C, 03 C,                
CONSTANT EACR1  HERE                                           
C0 C, 00 C, F8 C, 00 C, FC C, 00 C, FE C, 00 C,                
FF C, 00 C, FF C, 00 C, FF C, 00 C, FF C, 00 C,                
FE C, 00 C, FE C, 80 C, FC C, E0 C, F0 C, C0 C,                
CONSTANT EACR2                                                 
BASE !   ;S                                                     
( TETRA ) BASE @ HEX                                           
HERE                                                           
00 C, 00 C, 00 C, 01 C, 00 C, 03 C, 00 C, 07 C,                 
00 C, 07 C, 00 C, 0F C, 00 C, 1F C, 00 C, 1F C,                 
00 C, 3F C, 00 C, 7F C, 00 C, 7F C, 00 C, FF C,                 
CONSTANT TETRA1                                                
HERE                                                           
80 C, 80 C, 80 C, C0 C, 40 C, C0 C, 60 C, E0 C,                 
60 C, F0 C, 70 C, F0 C, 38 C, F8 C, 38 C, FC C,                 
3C C, FC C, 3E C, FE C, 1E C, FF C, 1F C, FF C,                 
CONSTANT TETRA2                                                
                                                               
                                                               
                                                               
                                                               
BASE ! ;S                                                      
( PLAYER MISSLE STUFF )  BASE @ HEX                            
PMAREA <-> PMX                                                 
 D008 <-> SIZEP0 D000 <-> HPOSP0                               
HERE PMX 400 + , PMX 500 + , PMX 600 + , PMX 700 + , PMX 300 + 
 ,     <-> PMXTAB                                              
: XCLEARPM PMX 300 + 500 0 FILL ;                              
: PMON  PMX hi PMBASE C! 3C DMACTL C!  3 GRACTL C!             
        4 0 DO  88 COLPM0 I + C! LOOP                          
  4 0   DO 1 D008 I + C! LOOP ;                                
: PMOFF ( 22 DMACTL C! ) 0 GRACTL C!  XCLEARPM                 
        5 0 DO 0 D00D I + C! LOOP ;                            
                                                               
BASE ! ;S                                                      
                                                               
                                                               
                                                               
 ( PLAYER MISSLE STUFF CONT ) BASE @ HEX                       
                                                               
0 VARIABLE XWAITV                                              
: SETUP XCLEARPM PMON 0 XWAITV C!                              
     4 0 DO 88 COLPM0 I + C! LOOP 88 2C7 C!                    
     21 D01B C! 21 26F C! ( 68 D000 C! 70 D001 C! 78 D002! )   
 ( 4 0 DO I I + 98 + D007 I - C! LOOP  )  80 D003 C!           
          4 0 DO 0 D009 I + C! LOOP  1 D008 C!  ;              
: HELLO 02FC C@ FF = IF ELSE 1 XWAITV C! KEY DROP THEN         
        ?TERMINAL IF 1 XWAITV C! THEN XWAITV C@ ;              
: XWAIT ( TICKS )                                              
  0 220 ! FF 22E C! 1 MAX 220 !                                
  BEGIN 22E C@ 0= HELLO OR UNTIL ;                             
                                                               
 BASE ! ;S                                                     
                                                               
( ASSEMBLER LOGO CODE ) BASE @ HEX                             
68 VARIABLE SH  76 VARIABLE CH  82 VARIABLE  TH                
68 VARIABLE SHT 76 VARIABLE CHT 82 VARIABLE THT                
68 VARIABLE SV  68 VARIABLE CV  68 VARIABLE  TV                
68 VARIABLE SVT 68 VARIABLE CVT 68 VARIABLE TVT                
01 VARIABLE DSH FF VARIABLE DCH 01 VARIABLE DTH                
FF VARIABLE DSV 01 VARIABLE DCV 01 VARIABLE DTV                
18 CONSTANT BMH ( BIT MAP HEIGHT )                             
:  XINITPOS                                                     
  68 SH  ! 76 CH  ! 82 TH  !                                   
  68 SHT ! 76 CHT ! 82 THT !                                   
  68 SV  ! 68 CV  ! 68 CV  !                                   
  68 SVT ! 68 CVT ! 68 TVT !                                   
  01 DSH ! FF DCH ! 01 DTH !                                   
  FF DSV ! 01 DCV ! 01 DTV ! ;                                 
BASE ! ;S                                                      
( ASSEMBLER LOGO CODE CONT ) BASE @ HEX                        
 CODE XSH ( PLACE SQUARE HORIZONTALLY )                        
   SHT LDA, SH STA, D000 STA, RTS, END-CODE                    
 CODE XCH                                                      
   CHT LDA, CH STA, D001 STA, CLC, 8 # ADC, D002 STA,          
   CLC,  RTS, END-CODE                                         
 CODE XTH                                                      
   THT LDA, TH STA, D003 STA, CLC, 8 # ADC, D007 STA,          
   2 # ADC, D006 STA, 2 # ADC, D005 STA, 2 # ADC, D004 STA,    
   CLC, RTS, END-CODE                                          
                                                               
                                                               
                                                               
                                                               
                                                               
BASE ! ;S                                                      
( ASSEMBLER LOGO CODE ) BASE @ HEX                             
CODE XZMAP ( ASSUMES FB )                                      
  0 # LDA, BMH # LDY, BEGIN,                                   
    DEY, FB )Y STA, 0 # CPY, 0= UNTIL,                         
    RTS, END-CODE                                              
CODE CPYMAP ( ASSUMES FB=DEST FD=SOURCE )                      
   BMH # LDY, BEGIN,                                           
    DEY, FD )Y LDA, FB )Y STA, 0 # CPY, 0= UNTIL,              
    RTS, END-CODE                                              
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
BASE ! ;S                                                       
( ASSEMBLER LOGO CODE ) BASE @ HEX                             
CODE YS1                                                       
   SV LDA, CLC, PMXTAB ADC, FB STA,                            
   0 # LDA,   PMXTAB 1+ ADC, FC STA,  CLC,                     
   EASQ lo # LDA, FD STA, EASQ hi # LDA, FE STA, RTS, END-CODE 
CODE YC1                                                       
   CV LDA, CLC, PMXTAB 2 + ADC, FB STA,                        
   0 # LDA,   PMXTAB 3 + ADC, FC STA,  CLC, EACR1 lo # LDA,    
   FD STA, EACR1 hi # LDA, FE STA, RTS, END-CODE               
CODE YC2                                                       
   CV LDA, CLC, PMXTAB 4 + ADC, FB STA,                        
   0 # LDA,   PMXTAB 5 + ADC, FC STA,  CLC, EACR2 lo # LDA,    
   FD STA, EACR2 hi # LDA, FE STA, RTS, END-CODE               
BASE ! ;S                                                       
                                                               
                                                               
( ASSEMBLER LOGO CODE ) BASE @ HEX                             
CODE YT1                                                       
   TV LDA, CLC, PMXTAB 6 + ADC, FB STA,                        
   0 # LDA,   PMXTAB 7 + ADC, FC STA,  CLC, TETRA1 lo # LDA,   
   FD STA, TETRA1 hi # LDA, FE STA, RTS, END-CODE              
CODE YT2                                                       
   TV LDA, CLC, PMXTAB 8 + ADC, FB STA,                        
   0 # LDA,   PMXTAB 9 + ADC, FC STA,  CLC, TETRA2 lo # LDA,   
   FD STA, TETRA2 hi # LDA, FE STA, RTS, END-CODE              
                                                               
                                                               
                                                               
                                                               
                                                               
BASE ! ;S                                                       
                                                               
( LOGO CODE CONT ) BASE @ HEX                                  
 CODE XSV ' YS1 JSR, ' XZMAP JSR, SVT LDA, SV STA,             
          ' YS1 JSR, ' CPYMAP JSR, CLC,                        
          RTS, END-CODE                                        
 CODE XCV ' YC1 JSR, ' XZMAP JSR, ' YC2 JSR, ' XZMAP JSR,      
          CVT LDA, CV STA,                                     
          ' YC1 JSR, ' CPYMAP JSR, ' YC2 JSR, ' CPYMAP JSR,    
          RTS, END-CODE                                        
 CODE XTV ' YT1 JSR, ' XZMAP JSR, ' YT2 JSR, ' XZMAP JSR,      
          TVT LDA, TV STA,                                     
          ' YT1 JSR, ' CPYMAP JSR, ' YT2 JSR, ' CPYMAP JSR,    
          RTS, END-CODE                                        
                                                               
                                                               
                                                               
BASE ! ;S                                                      
( ASSEMBLER LOGO CODE ) BASE @ HEX                             
CODE MSH SH LDA, CLC, DSH ADC, SHT STA, ' XSH JSR, SH LDA,     
 38 # CMP, 0= IF, 1 # LDA, DSH STA, THEN,  SH LDA, B8 # CMP,   
 0= IF, FF # LDA, DSH STA, THEN, CLC, RTS, END-CODE            
CODE MCH CH LDA, CLC, DCH ADC, CHT STA, ' XCH JSR, CH LDA,     
 38 # CMP, 0= IF, 1 # LDA, DCH STA, THEN, CH LDA, B8 # CMP,    
 0= IF, FF # LDA, DCH STA, THEN, CLC, RTS, END-CODE            
CODE MTH TH LDA, CLC, DTH ADC, THT STA, ' XTH JSR, TH LDA,     
 38 # CMP, 0= IF, 1 # LDA, DTH STA, THEN, TH LDA, B8 # CMP,    
 0= IF, FF # LDA, DTH STA, THEN, CLC, RTS, END-CODE            
                                                               
BASE ! ;S                                                       
                                                               
                                                               
                                                               
                                                               
( ASSEMBLER LOGO CODE ) BASE @ HEX                             
                                                               
CODE MSV SV LDA, CLC, DSV ADC, SVT STA, ' XSV JSR, SV LDA,     
 38 # CMP, 0= IF, 1 # LDA, DSV STA, THEN,  SV LDA, B8 # CMP,   
 0= IF, FF # LDA, DSV STA, THEN, CLC, RTS, END-CODE            
CODE MCV CV LDA, CLC, DCV ADC, CVT STA, ' XCV JSR, CV LDA,     
 38 # CMP, 0= IF, 1 # LDA, DCV STA, THEN, CV LDA, B8 # CMP,    
 0= IF, FF # LDA, DCV STA, THEN, CLC, RTS, END-CODE            
CODE MTV TV LDA, CLC, DTV ADC, TVT STA, ' XTV JSR, TV LDA,     
 38 # CMP, 0= IF, 1 # LDA, DTV STA, THEN, TV LDA, B8 # CMP,    
 0= IF, FF # LDA, DTV STA, THEN, CLC, RTS, END-CODE            
                                                               
BASE ! ;S                                                       
                                                               
                                                               
                                                               
( ASSEMBLER LOGO CODE ) BASE @ HEX                             
0 VARIABLE CLRX                                                
0 VARIABLE LFLAG                                                
CODE VBIPROC LFLAG LDA, 0= NOT IF,                              
 ' MSH JSR, ' MSV JSR,                                         
 ' MCH JSR, ' MCV JSR,                                         
 ' MTH JSR, ' MTV JSR, THEN,                                   
14 LDA, # 3 AND, 0= IF, CLRX LDA, CLC, 1 # ADC, CLRX STA, THEN,
  ' MUSICVBI JMP,  E462 JMP,   END-CODE                        
: INITVBI  ' VBIPROC 7 SET-INT 0 LFLAG ! 0 XWAITV ! ;           
: QUITVBI E462 7 SET-INT ;                                      
: START 1 LFLAG C! ;                                            
: STOPX 0 LFLAG C! ;                                            
                                                               
BASE ! ;S                                                       
                                                               
( DISPLAY LIST ) BASE @ HEX                                    
 CODE INITL ' XSV JSR, ' XCV JSR, ' XTV JSR,                   
  ' XSH JSR, ' XCH JSR, ' XTH JSR, NEXT JMP, END-CODE          
: COLOR-OFF SAVDLH 230 ! ;                                     
BASE ! ;S                                                      
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
 ( EA   LOGO DL  ) BASE @ HEX                                  
 HERE HERE                                                     
  30 C, 30 C, 30 C, 30 C, 30 C, 30 C, 30 C, 30 C,              
  B0 C, B0 C, B0 C, B0 C, B0 C, B0 C, B0 C, B0 C,              
  B0 C, B0 C, B0 C, B0 C, B0 C, B0 C, B0 C, B0 C,              
  B0 C, B0 C, B0 C, B0 C, B0 C, B0 C, B0 C, B0 C,              
  B0 C, B0 C, B0 C, B0 C, B0 C, B0 C, B0 C, B0 C,              
  30 C, 30 C, 30 C, 30 C, 30 C, 30 C, 30 C, 30 C,              
  41 C, , <-> TDL                                              
                                                               
BASE !   ;S                                                     
                                                               
                                                               
                                                               
                                                               
                                                               
;S                                                              
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
;S                                                              
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
                                                               
( WLOGO DATA ) BASE @ HEX HERE                                 
80 C, 80 C, 80 C, 80 C, 80 C, 80 C,                            
80 C, 88 C, DD C, 5D C, 77 C, 22 C,                            
                                                               
80 C, 80 C, 9C C, A2 C, A2 C, A2 C,                            
A2 C, A2 C, A2 C, 22 C, 22 C, 1C C,                            
                                                               
00 C, 00 C, F1 C, 89 C, 89 C, 89 C,                            
F1 C, A1 C, B1 C, 91 C, 89 C, 89 C,                            
                                                               
00 C, 00 C, 13 C, B4 C, F4 C, 56 C,                            
13 C, 11 C, 10 C, 10 C, 14 C, 13 C,                            
                                                               
00 C, 00 C, 8E C, 51 C, 01 C, 02 C,                            
02 C, 84 C, 44 C, 44 C, 40 C, 84 C, CONSTANT WLOGO             
BASE ! ;S                                                      
( METAMORPHOSIS ) BASE @ HEX                                   
 : CRSOURCE ( C R -- DATA BYTE )                               
   DUP C < IF C * SWAP C -  C + ( TIMING ) 2DROP   0 ELSE      
  C - SWAP C * + WLOGO + C@ THEN ;                             
 : CRDESTA  ( C R --> DEST ADDR )                              
    SWAP DUP +  PMXTAB + @ + 68 + ;                            
 : METAM                                                       
    0 D008 C! 6C D000 C! 74 D001 C! 7C D002 C! 84 D003 C!      
              92 D004 C! 90 D005 C! 8E D006 C! 8C D007 C!      
    100 0 DO                                                   
    RAND 5 MOD RAND 18 MOD                                     
    2DUP CRSOURCE ROT ROT                                      
    CRDESTA C! HELLO IF LEAVE THEN                             
    LOOP ;                                                     
  BASE ! ;S                                                    
                                                               
( DISPLAY LIST ) BASE @ HEX                                    
                                                               
 CODE HBIM                                                     
   PHA, PHP,  D40B LDA,  CLRX ADC,  D40A STA,                  
   D012 STA, D013 STA, D014 STA, D015 STA,                     
   D019 STA, PLP, PLA, RTI, END-CODE                           
                                                               
: EMERGE                                                       
   18 0 DO I                                                   
    5 0 DO DUP I SWAP 2DUP CRSOURCE                            
           ROT ROT CRDESTA C! LOOP DROP                        
           HELLO IF LEAVE THEN LOOP ;                          
                                                               
                                                               
BASE ! ;S                                                      
                                                               
( EA   LOGO   ) BASE @ HEX                                     
: COLOR-ON                                                     
  3C D400 C! 40 D40E C!  ' HBIM 200 ! C0 D40E C!               
  TDL 230 ! ;                                                  
: SPLASH                                                       
  0 D208 C! 3 D20F C!  0 2C8 C! INITMUSIC                      
  SETUP INITVBI  XINITPOS INITL COLOR-ON                       
  1 0 DO 80 XWAIT START 100 XWAIT STOPX                        
    HELLO IF LEAVE THEN LOOP                                   
   40 D40E C!  ' HBIM 200 ! C0 D40E C!                         
   80 XWAIT 0 2C8 C!  0 220 ! FF 22E C! 1C0 220 !              
   METAM EMERGE  40 D40E C! BEGIN 22E C@ 0= HELLO OR UNTIL     
   0 DMACTL C! COLOR-OFF  QUITVBI XSND  PMOFF                  
   D208 D200 DO 0 I C! LOOP ;                                  
                                                               
BASE !   ;S                                                     
( RESET ) HEX                                                  
0 #WORMS ! HERE 58 ALLOT <-> W1 HERE 58 ALLOT <-> W2           
           HERE 58 ALLOT <-> W3 HERE 58 ALLOT <-> W4           
: GRIDGLIDERS  INITMEM SPLASH  ANOFF C SCRLVAL C! 0 22F C!     
  CSBUILT @ IF ELSE FIXDUMMY THEN 0 #WORMS ! 0 SEL !           
   W1 INITWORM W2 INITWORM W3 INITWORM W4 INITWORM             
3 F 60 1 SOUND XSND  WCHSETH CHNOW C! 06 STO ! 8 WCHSET 3 + C! 
7D EMIT CLEARPM INSTALLEM IWDISPLAY                            
SCRLON SUMO 7D EMIT CLEARF                                     
( PROMPT ) CLEARPM  SCRLOFF                                    
4 0 DO I GWORM INITMOVES LOOP                                  
4 #WORMS ! 1 2F0 C! ( CURSOR OFF ) DECIMAL                     
BEGIN   RUNLOOP  AGAIN                                         
RESTORE FIXINTRPTS                                             
0 2F0 C! ( CURSOR ON  ) ; DECIMAL                              
: TASK ; IS-FENCE  FINIS    (   PROF ) ;S                      
