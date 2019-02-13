# utl-unusual-but-useful-sas-filenames-and-destinations
Unusual but useful sas filenames and destinations
    Unusual but useful sas filenames and destinations

    Win 7 64bit SAS 9.4M2

     Examples

        1. The bit bucket
        2. Temporary files
        3. Dimport and dexport csv and xlsx
        4. Catalog and catams
        5. Ft15f001
        6. Clipboard
        7. Pipes (named pipes even more powerfull in client server environment)
        8. Url
        9. Email
       10. Zip and unzip
       11. Default printer
       12. Filerefs you cannot use AUX, CON, NUL, PRN, LPT1 - LPT9, and COM1 - COM9


    =================
    1. The bit bucket
    =================

       filename rubbish dummy ;
       data _null_ ;
         file rubbish ;
         set sashelp.class ;
         put _all_ ;
       run ;

       filename rubbish "/dev/null" ;
       data _null_ ;
         file rubbish ;
         set sashelp.class ;
         put _all_ ;
       run ;

    ==================
    2. Temporary files
    ==================

       filename wrk temp ;
       data _null_ ;
         file wrk ;
         put 'test' ;
       run ;

       data _null_ ;
         infile wrk ;
         input ;
         put _infile_ ;
       run ;


    ====================================
    3. dimport and dexport csv and xlsx
    ====================================

    dm "dexport sashelp.class 'd:\csv\csv.csv' replace";
    dm "dexport sashelp.class 'd:\xls\xlsx.xlsx' replace";

    dm "dimport 'd:\csv\csv.csv' work.froCsv replace";
    dm "dimport 'd:\xls\xlsx.xlsx' work.froXls replace";


       * Can read & write to catalog members of various types ;
       filename cat catalog 'work.test.my.catams' ;
       data _null_ ;
         file cat ;
         set sashelp.class ;
         put name;
       run ;

       data _null_ ;
         infile cat ;
         input;
         put _infile_ ;
       run ;


    ======================
    4. catalog and catams
    ======================

        filename foo ZIP 'd:\zip\clas.zip';
        data _null_;
           infile "d:\csv\clas.csv";
           input;
           file foo(class);
           put _infile_;
        run;quit;


      UNZIP the csv file

      filename foo ZIP 'd:\zip\clas.zip';
      data _null_;
         infile foo(class);
         input;
         put _infile_;
      run;quit;

    ========================================
    5. ft15f001 (originally from data_null_)
    ========================================

    filename ft15f001 "c:/oto/deletexyz.sas";
    parmcards4;
    %macro delete_wrk/dex="kill sas fork";
     proc datasets lib=work kill;
     run;quit;
    %mend delete_wrk;
    ;;;;
    run;quit;

    data _null_;
      infile ft15f001;
      input;
      put _infile_;
    run;quit;

    filename ft15f001 clear;

    ============
    6. clipboard
    ============

    * copy some text ie "cntl c';

       filename clip clipbrd ;
       data _null_ ;
         infile clip ;
         input ;
         put _infile_;
       run ; quit;

    =======================================================================
    7. Pipes (named pipes even more powerfull in client server environment)
    ========================================================================

       * read output of a program from a pipe ;
       filename cmd pipe 'ls -l *.log' ;
       data _null_ ;
         infile cmd ;
         input ;
         put _infile_ ;
       run ;

    filename pipetree pipe "tree ""C:\Program Files\Windows NT"" /F /A" lrecl=5000;
    data _null_;
    infile pipetree truncover;
    input dirlist $char1000.;
    *if index(dirlist,'.') =0;
    put dirlist;
    run;

    Folder PATH listing for volume c
    Volume serial number

    C:\PROGRAM FILES\WINDOWS NT
      +---Accessories
      |   |   wordpad.exe
      |   |   WordpadFilter.dll
      |   |
      |   \---en-US
      |           wordpad.exe.mui
      |
      \---TableTextService
      |   TableTextService.dll
      |   TableTextServiceAmharic.txt
      |   TableTextServiceArray.txt
      |   TableTextServiceDaYi.txt
      |   TableTextServiceYi.txt
      |
      \---en-US
      TableTextService.dll.mui


    ======
    8. URL
    ======

    filename x url 'http://www.sas.com' ;
    data _null_ ;
      infile x ;
      input ;
      put _infile_ ;
    run ;


    ========
    9. Email
    ========

    filename report email 'regusers@ACHME.com' subject='test mail' attach='c:\test.txt' ;
    file report ;
      put 'Hi. Here is my test email' ;
    run ;


    =================
    10. Zip and unzip
    ==================

     * ZIP;
    filename foo ZIP 'd:\zip\clas.zip';
    data _null_;
        infile "d:\csv\clas.csv";
        input;
        file foo(class);
        put _infile_;
    run;quit;

    *UNZIP the csv zip file;
    filename foo ZIP 'd:\zip\clas.zip';
    data _null_;
      infile foo(class);
      input;
      put _infile_;
    run;quit;


    ===================
    11. Default printer
    ====================

    * Send output directly to default printer ;
    filename p printer ;
    data _null_ ;
      file p ;
      set sashelp.class ;
      put _all_ ;
    run ;

    =============================================================================
    12. Filerefs you cannot use AUX, CON, NUL, PRN, LPT1 - LPT9, and COM1 - COM9
    =============================================================================

    filename con "d:/txt/con.txt";
    data _null_ ;
      file con ;
      set sashelp.class ;
      put _all_ ;
    run ;

    ERROR: Physical file does not exist, d:\txt\con.txt.

