# utl_more_flexible_and_maintainable_crosstab_than_proc_tabulate
More flexible and maintainable crosstab than proc tabulate. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverfl SAS community.
    More flexible and maintainable crosstab than proc tabulate

    github
    https://tinyurl.com/yb98oq9w
    https://github.com/rogerjdeangelis/utl_more_flexible_and_maintainable_crosstab_than_proc_tabulate

    gather macro Alea Iacta
    https://github.com/clindocu

    SAS Forum
    https://communities.sas.com/t5/General-SAS-Programming/Collating-Proc-Freq-Table-Values/m-p/440948

    Original topic: Collating Proc Freq Table Values

    INPUT
    =====

     RULES
       Compute counts and row percents for VARB, VARC, SEX, VARE with ATYPE Across

     WORK.HAVE total obs=16

       ATYPE    VARB    VARC    SEX    VARE

         1       X        1      M       1
         1       X        2      M       1
         1       Y        3      M       5
         2       Y        1      F       2
         2       C        2      F       3
         3       D        3      M       3
         3       X        1      M       3
         3       Y        2      F       1
         3       Y        3      F       1
         4       D        1      M       2
         4       X        2      F       2
         5       C        3      F       2
         5       C        1      F       1
         5       D        2      F       1
         5       D        3      F       1
         5       D        1      M       1


     EXAMPLE OUTPUT  (looks better in the pdf)
     =========================================

      WORK.HAVFIX total obs=14

        MJR     MNR     NPCT1     NPCT2      NPCT3     NPCT4      NPCT5      TOTAL

        SEX      F     0(0%)      2(22%)    2(22%)     1(11%)    4(44%)     9(100%)
        SEX      M     3(43%)     0(0%)     2(29%)     1(14%)    1(14%)     7(100%)

        VARB     C     0(0%)      1(33%)    0(0%)      0(0%)     2(67%)     3(100%)
        VARB     D     0(0%)      0(0%)     1(20%)     1(20%)    3(60%)     5(100%)
        VARB     X     2(50%)     0(0%)     1(25%)     1(25%)    0(0%)      4(100%)
        VARB     Y     1(25%)     1(25%)    2(50%)     0(0%)     0(0%)      4(100%)

        VARC     1     1(17%)     1(17%)    1(17%)     1(17%)    2(33%)     6(100%)
        VARC     2     1(20%)     1(20%)    1(20%)     1(20%)    1(20%)     5(100%)
        VARC     3     1(20%)     0(0%)     2(40%)     0(0%)     2(40%)     5(100%)

        VARE     1     2(25%)     0(0%)     2(25%)     0(0%)     4(50%)     8(100%)
        VARE     2     0(0%)      1(25%)    0(0%)      2(50%)    1(25%)     4(100%)
        VARE     3     0(0%)      1(33%)    2(67%)     0(0%)     0(0%)      3(100%)
        VARE     5     1(100%)    0(0%)     0(0%)      0(0%)     0(0%)      1(100%)

        Sum            12(19%)    8(13%)    16(25%)    8(13%)    20(31%)    64(100%)


    PROCESS
    =======

      * you could eliminate gather and corresp and substitute 'proc report' with an output dataset;

      %utl_gather(have,var,val,atype,havxpo,valformat=$5.);

      * almost there just have to make it nice;
      ods exclude all;
      ods output observed=havCnt;
      proc corresp data=havXpo dim=1 observed cross=both;
      tables var val, atype;
      run;quit;
      ods select all;

      * this looks like a lot but it will often eliminate being painted in a corner;
      data havFix;
        retain mjr mnr ;
        length mjr mnr $6;
        set havCnt;
        array cnts[*] _:;
        array cntchr[5] $12 npct1-npct5;
        do i=1 to 5;
            cntchr[i]=cats(put(cnts[i],4.),'(',put(cnts[i]/sum ,percent.),')');
        end;
        length total $10;
        total=cats(put(sum,4.),'(',put(sum/sum ,percent.),')');
        drop i;
        mjr=scan(label,1,'*');
        mnr=scan(label,2,'*');
        drop _: label sum;
      run;quit;

      * balance what you do here with the previous datastep;
      ods pdf file="d:/pdf/utl_more_flexible_and_maintainable_crosstab_than_proc_tabulate.pdf";
      proc report data=havFix style(column)={just=right} ;
      run;quit;
      ods pdf close;



    OUTPUT (straight proc print - but you ahve a dataset)
    ======================================================

      WORK.HAVFIX total obs=14

        MJR     MNR     NPCT1     NPCT2      NPCT3     NPCT4      NPCT5      TOTAL

        SEX      F     0(0%)      2(22%)    2(22%)     1(11%)    4(44%)     9(100%)
        SEX      M     3(43%)     0(0%)     2(29%)     1(14%)    1(14%)     7(100%)

        VARB     C     0(0%)      1(33%)    0(0%)      0(0%)     2(67%)     3(100%)
        VARB     D     0(0%)      0(0%)     1(20%)     1(20%)    3(60%)     5(100%)
        VARB     X     2(50%)     0(0%)     1(25%)     1(25%)    0(0%)      4(100%)
        VARB     Y     1(25%)     1(25%)    2(50%)     0(0%)     0(0%)      4(100%)

        VARC     1     1(17%)     1(17%)    1(17%)     1(17%)    2(33%)     6(100%)
        VARC     2     1(20%)     1(20%)    1(20%)     1(20%)    1(20%)     5(100%)
        VARC     3     1(20%)     0(0%)     2(40%)     0(0%)     2(40%)     5(100%)

        VARE     1     2(25%)     0(0%)     2(25%)     0(0%)     4(50%)     8(100%)
        VARE     2     0(0%)      1(25%)    0(0%)      2(50%)    1(25%)     4(100%)
        VARE     3     0(0%)      1(33%)    2(67%)     0(0%)     0(0%)      3(100%)
        VARE     5     1(100%)    0(0%)     0(0%)      0(0%)     0(0%)      1(100%)

        Sum            12(19%)    8(13%)    16(25%)    8(13%)    20(31%)    64(100%)

    *                _               _       _
     _ __ ___   __ _| | _____     __| | __ _| |_ __ _
    | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |
    | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |
    |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|

    ;

     data have;
      input atype varb $  varc sex $ vare;
    datalines;
    1 X 1 M 1
    1 X 2 M 1
    1 Y 3 M 5
    2 Y 1 F 2
    2 C 2 F 3
    3 D 3 M 3
    3 X 1 M 3
    3 Y 2 F 1
    3 Y 3 F 1
    4 D 1 M 2
    4 X 2 F 2
    5 C 3 F 2
    5 C 1 F 1
    5 D 2 F 1
    5 D 3 F 1
    5 D 1 M 1
    run;
    *          _       _   _
     ___  ___ | |_   _| |_(_) ___  _ __
    / __|/ _ \| | | | | __| |/ _ \| '_ \
    \__ \ (_) | | |_| | |_| | (_) | | | |
    |___/\___/|_|\__,_|\__|_|\___/|_| |_|

    ;

    %utl_gather(have,var,val,atype,havxpo,valformat=$5.);


    /*
    Up to 40 obs WORK.HAVXPO total obs=64

    Obs    ATYPE    VAR     VAL

      1      1      VARB     X
      2      1      VARC     1
      3      1      SEX      M
      4      1      VARE     1
      5      1      VARB     X
      6      1      VARC     2
      7      1      SEX      M
      8      1      VARE     1
      9      1      VARB     Y
     10      1      VARC     3
     11      1      SEX      M
     12      1      VARE     5
     13      2      VARB     Y
    ...
    */

    * you can get row or column percents however I like to use a datastep for percents;
    ods exclude all;
    ods output observed=havCnt;
    proc corresp data=havXpo dim=1 observed cross=both;
    tables var val, atype;
    run;quit;
    ods select all;

    /(
    Up to 40 obs from havCnt total obs=14

    Obs    LABEL       _1    _2    _3    _4    _5    SUM

      1    SEX * F      0     2     2     1     4      9
      2    SEX * M      3     0     2     1     1      7
      3    VARB * C     0     1     0     0     2      3
      4    VARB * D     0     0     1     1     3      5
      5    VARB * X     2     0     1     1     0      4
      6    VARB * Y     1     1     2     0     0      4
      7    VARC * 1     1     1     1     1     2      6
      8    VARC * 2     1     1     1     1     1      5
      9    VARC * 3     1     0     2     0     2      5
     10    VARE * 1     2     0     2     0     4      8
     11    VARE * 2     0     1     0     2     1      4
     12    VARE * 3     0     1     2     0     0      3
     13    VARE * 5     1     0     0     0     0      1
     14    Sum         12     8    16     8    20     64
    */

    * see above;
    data havFix;
      retain mjr mnr ;
      length mjr mnr $6;
      set havCnt;
      array cnts[*] _:;
      array cntchr[5] $12 npct1-npct5;
      do i=1 to 5;
          cntchr[i]=cats(put(cnts[i],4.),'(',put(cnts[i]/sum ,percent.),')');
      end;
      length total $10;
      total=cats(put(sum,4.),'(',put(sum/sum ,percent.),')');
      drop i;
      mjr=scan(label,1,'*');
      mnr=scan(label,2,'*');
      drop _: label sum;
    run;quit;

    * ypu could judt print havFix;
    ods pdf file="d:/pdf/utl_more_flexible_and_maintainable_crosstab_than_proc_tabulate.pdf";
    proc report data=havFix style(column)={just=right} ;
    run;quit;
    ods pdf close;



