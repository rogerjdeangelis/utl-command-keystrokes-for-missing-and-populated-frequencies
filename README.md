# utl-command-keystrokes-for-missing-and-populated-frequencies
Command keystrokes for missing and populated frequencies  
    %let pgm=utl-command-keystrokes-for-missing-and-populated-frequencies;

    Command keystrokes for missing and populated frequencies

    Suppose you want to compute these crosstabs.
    Just highlight the sas dataset and type mish on the classic editor command line
    /*                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    data class ;
      set sashelp.class;
      if _n_ in (1,4,6,8) then sex=" ";
      if uniform(123)<.2 then age=.;
    run;quit;

    /*           _               _
      ___  _   _| |_ _ __  _   _| |_
     / _ \| | | | __| `_ \| | | | __|
    | (_) | |_| | |_| |_) | |_| | |_
     \___/ \__,_|\__| .__/ \__,_|\__|
                    |_|
    */

    Frequency of sex datasets class 15NOV2022:19:23:17

                                    Cumulative    Cumulative
    SEX    Frequency     Percent     Frequency      Percent
    --------------------------------------------------------
    MIS           4       21.05             4        21.05
    POP          15       78.95            19       100.00

    Frequency of sex datasets class 15NOV2022:19:23:17

                                    Cumulative    Cumulative
    SEX    Frequency     Percent     Frequency      Percent
    --------------------------------------------------------
    MIS           4       21.05             4        21.05
    POP          15       78.95            19       100.00


    Frequency of age*sex datasets class 15NOV2022:19:22:01

                                           Cumulative    Cumulative
    AGE    SEX    Frequency     Percent     Frequency      Percent
    ---------------------------------------------------------------
    MIS    POP           4       21.05             4        21.05
    POS    MIS           4       21.05             8        42.11
    POS    POP          11       57.89            19       100.00

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    Highlight

      work.class

    and

    ====> mish age*sex

    00001
    00002  work.class
    00003

    /*                  _
      ___ _ __ ___   __| |  _ __ ___   __ _  ___ _ __ ___
     / __| `_ ` _ \ / _` | | `_ ` _ \ / _` |/ __| `__/ _ \
    | (__| | | | | | (_| | | | | | | | (_| | (__| | | (_) |
     \___|_| |_| |_|\__,_| |_| |_| |_|\__,_|\___|_|  \___/

    */


    * You can eliminate the fist macro using %let rc=%sysfunc(dosubl(;
    * Command macro for populated and missing counts;

    %macro mish /cmd parmbuff des="highlight dataset and type frqh sex and  for on frequency on sexxage";
       %let argx=%scan(&syspbuff,1,%str( ));
       store;note;notesubmit '%misha;';
       run;
    %mend mish;

    %macro misha;
      proc format;

         value num2mis
          .          = 'MIS'
          .<-.Z      = 'MIS'
          0          = 'ZRO'
          0<-high    = "POS"
          low-<0     = 'NEG'
          other      = 'POP'
          ;

       value $chr2mis
       " "  ='MIS'
       other='POP'
        ;
       run;quit;

       filename clp clipbrd ;
       data _null_;
         infile clp;
         input;
         put _infile_;
         call symputx('argd',_infile_);
         call symputx("__dtetym",put(datetime(),datetime23.));
       run;
       dm "out;clear;";
       options nocenter;
       footnote;
       ODS NOPROCTITLE;
       title1 "Frequency of &argx in dataset &argd &__dtetym";
       proc freq data=&argd;
       format _numeric_ num2mis. _character_ $chr2mis.;
       tables &argx./list missing out=frqh;
       run;
       title;
       ODS PROCTITLE
       dm "out;top;";
    %mend misha;

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
