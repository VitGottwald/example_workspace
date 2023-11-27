*/*PROPRIETARY V3 STATEMENT                                          */ 00004000
*/*LICENSED MATERIALS - PROPERTY OF IBM                              */ 00006000
*/*"RESTRICTED MATERIALS OF IBM"                                     */ 00008000
*/*5695-DF1                                                          */ 00010000
*/*COPYRIGHT 1992, 2011 IBM CORP.                                    */ 00012500
*/*END PROPRIETARY V3 STATEMENT                                      */ 00015000
*/*                                                                  */ 00018000
.* Notes                                                                00018600
.*    Supports invocation in 24-bit or 31-bit.                          00019200
         MACRO                                                          00020000
&NAME    BUILD &POOL,&NO,&LTH                                      @L1C 00025700
.********************************************************************** 00031400
.* CHANGE ACTIVITY                                                      00032800
.* $L1=64BIT,JDZ1D10,20101110,SVLWER: 64-bit groundwork            @L1A 00034200
.********************************************************************** 00035600
         GBLC  &SYSAM64    GLOBAL VARIABLE SET BY SYSSTATE MACRO   @L1A 00037000
         GBLC  &SYSASCE    GLOBAL VARIABLE SET BY SYSSTATE MACRO   @L1A 00038400
         GBLA  &SYSALVL    GLOBAL VARIABLE SET BY SYSSTATE MACRO   @L1A 00039200
         LCLB  &NOGO                                                    00040000
         SYSSTATE TEST     Test how macros are to expand           @L1A 00042700
         AIF   ('&SYSAM64' NE 'YES').L1    Go if AMODE64 not YES   @L1A 00045400
         IGGERMAC 203,BUILD    BUILD macro does not support 64-bit @L1A 00048100
&NOGO    SETB  1           Set error switch                        @L1A 00050800
.L1      AIF   (('&SYSASCE' EQ '') OR ('&SYSASCE' EQ 'P')).L3      @L1A 00053500
         IGGERMAC 204,BUILD    BUILD macro does not support AR     @L1A 00056200
&NOGO    SETB  1           Set error switch                        @L1A 00058900
.L3      AIF   ('&POOL' NE '').R1    Branch of pool location coded @L1C 00061600
         IHBERMAC 02       Write error message                     @L1M 00064300
&NOGO    SETB  1           Set error switch                        @L1M 00067000
.R1      ANOP                                                           00070000
         AIF   ('&NO' NE '').R2      Branch if number coded        @L1C 00073100
         IHBERMAC 03                                               @L1M 00076200
&NOGO    SETB  1                                                   @L1M 00079300
.R2      AIF   ('&NO' NE '(0)').T1                                      00082400
         AIF   ('&LTH' NE '').E4                                        00085500
         AGO   .R3                                                      00088600
.T1      AIF   ('&LTH' EQ '').E3                                        00091700
.R3      AIF   (&NOGO EQ 1).TERM                                        00095000
.* No error found.  begin the expansion.                           @L1A 00097500
&NAME    IHBINNRA &POOL                                                 00100000
         AIF   ('&LTH' EQ '&LTH(1)' AND '&NO' EQ '&NO(1)').CONT         00120000
         AIF   ('&NO' EQ '&NO(1)').NUMB                                 00140000
         AIF   ('&NO' EQ '(0)').ZERO                                    00160000
         STH   &NO(1),0(0,1)            STORE BUFFER NUMBER             00180000
         AGO   .CHKLTH                                                  00200000
.ZERO    ST    &NO(1),0(0,1)            STORE PARAMETERS                00220000
         AGO   .LINK                                                    00240000
.NUMB    AIF   (T'&NO NE 'N').USEDC                                     00260000
         AIF   (&NO GT 4095).USEDC                                      00280000
         LA    14,&NO.(0,0)             GET BUFFER NUMBER               00300000
         STH   14,0(0,1)                STORE INTO LIST                 00320000
         AGO   .CHKLTH                                                  00340000
.* If possible, avoid dependence on a code base register.          @L1A 00348000
.USEDC   AIF   (&SYSALVL GT 0).USEDC2   Go if OS/390 R10 or later  @L1A 00356000
         B     *+6                      BRANCH AROUND VALUE             00364000
         DC    AL2(&NO)                 NUMBER OF BUFFERS               00372000
         MVC   0(2,1),*-2               STORE INTO LIST                 00380000
         AGO   .CHKLTH                                             @L1A 00388000
.USEDC2  BRAS  14,*+6                   BRANCH AROUND VALUE        @L1A 00396000
         DC    AL2(&NO)                 NUMBER OF BUFFERS          @L1A 00404000
         MVC   0(2,1),0(14)             STORE INTO LIST            @L1A 00412000
.CHKLTH  AIF   ('&NO' EQ '(0)').LINK                                    00420000
         AIF   ('&LTH' EQ '').E3                                        00440000
         AIF   ('&LTH' EQ '&LTH(1)').NUMBL                              00460000
         STH   &LTH(1),2(0,1)           STORE BUFFER LENGTH             00480000
         AGO   .LINK                                                    00500000
.NUMBL   AIF   (T'&LTH NE 'N').USEDCL                                   00520000
         AIF   (&LTH GT 4095).USEDCL                                    00540000
         LA    14,&LTH.(0,0)            GET BUFFER LENGTH               00560000
         STH   14,2(0,1)                STORE IN LIST                   00580000
         AGO   .LINK                                                    00600000
.* If possible, avoid dependence on a code base register.          @L1A 00613300
.USEDCL  AIF   (&SYSALVL GT 0).USEDCL2  Go if OS/390 R10 or later  @L1A 00626600
         B     *+6                      BRANCH AROUND VALUE             00639900
         DC    AL2(&LTH)                LENGTH OF BUFFERS               00653200
         MVC   2(2,1),*-2               STORE INTO LIST                 00666500
         AGO   .LINK                                                    00680000
.USEDCL2 BRAS  14,*+6                   Branch around value        @L1A 00684000
         DC    AL2(&LTH)                Length of buffers          @L1A 00688000
         MVC   2(2,1),0(14)             Store into list            @L1A 00692000
         AGO   .LINK                                               @L1A 00696000
.CONT    AIF   ('&LTH' EQ '').E3                                        00700000
.* If possible, avoid dependence on a code base register.          @L1A 00709000
         AIF   (&SYSALVL GT 0).CONT2    Go if OS/390 R10 or later  @L1A 00718000
         B     *+8                      BRANCH AROUND VALUES            00727000
         DC    AL2(&NO)                 NUMBER OF BUFFERS               00736000
         DC    AL2(&LTH)                LENGTH OF BUFFERS               00745000
         MVC   0(4,1),*-4               MOVE INTO LIST                  00754000
         AGO   .LINK                                               @L1A 00763000
.CONT2   BRAS  14,*+8                   Branch around values       @L1A 00772000
         DC    Y(&NO,&LTH)              Number & length of buffers @L1A 00781000
         MVC   0(4,1),0(14)             Move into list             @L1A 00790000
.LINK    LINK  EP=IECBBFB1                                              00800000
         MEXIT                                                          00820000
.E4      IHBERMAC 38,,&LTH                                              00904000
         MEXIT                                                          00912000
.E3      IHBERMAC 04                                                    00921000
         MEXIT                                                     @L1A 00922500
.TERM    ANOP                                                           00930000
         MEND                                                           00940000
