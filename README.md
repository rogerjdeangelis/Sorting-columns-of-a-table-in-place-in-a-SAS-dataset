# Sorting-columns-of-a-table-in-place-in-a-SAS-dataset
Sorting columns of a table in place. Keywords: sas sql join merge big data analytics macros oracle teradata mysql sas communities stackoverflow statistics artificial inteligence AI Python R Java Javascript WPS Matlab SPSS Scala Perl C C# Excel MS Access JSON graphics maps NLP natural language processing machine learning igraph DOSUBL DOW loop stackoverflow SAS community.
    Sorting columns of a table in place

    WPS/R or IML/R

    inspired by

    https://tinyurl.com/ybqzyvok
    https://communities.sas.com/t5/SAS-Enterprise-Guide/Sort-columns-independently/m-p/451265

    and
    https://goo.gl/UTP3LD
    http://stackoverflow.com/questions/42834439/r-sort-one-column-ascending-all-others-descending-based-on-column-order

    slot machine probability? (1/n*1/n*1/n)

    Not so easy in SQL or datastep(hash - I don't see it?)

    IML and especially R often force programmers to think
    non sequentially.

    HAVE ( Casino Slot machine )
    ============================

    Up to 40 obs WORK.TRECOL total obs=10

    No slot machine winners

    Obs   SPIN    COL1    COL2    COL3

      1      1      5       3       0
      2      2      3       3       1
      3      3      0       3       5
      4      4      3       4       0
      5      5      3       0       5
      6      6      1       1       2
      7      7      5       3       2
      8      8      5       0       3
      9      9      4       2       2
     10     10      0       4       1

    WANT Lets sort each column in place)
    ====================================

         SPIN COL1 COL2 COL3
     [1,]   1    0    0    1
     [2,]   2    0    0    1
     [3,]   3    1    1    1
     [4,]   4    2    1    2
     [5,]   5    3    2    3
     [6,]   6    3    2    3
     [7,]   7    3    3    3  WINNER
     [8,]   8    5    4    4
     [9,]   9    5    5    4
    [10,]  10    5    5    5  WINNER


    WORKING CODE  (a one liner)
    ===========================

          winner<-apply(slots,2,sort);

    FULL SOLUTION
    =============

    ods listing;
    data "d:/sd1/slots.sas7bdat";
     do spin=1 to 10;
       col1=int(6*uniform(-1));
       col2=int(6*uniform(-1));
       col3=int(6*uniform(-1));
       output;
     end;
    run;

    * Roll those slots;
    %utl_submit_wps64('
      source("c:/Program Files/R/R-3.3.2/etc/Rprofile.site",echo=T);
      library(haven);
      slots=as.matrix(read_sas("d:/sd1/slots.sas7bdat"));
      winner<-apply(slots,2,sort);
      winner;
    ');

    > library(haven); slots=as.matrix(read_sas('d:/sd1/slots.sas7bdat'));
      winner<-apply(slots,2,sort); winner;

          CID COL1 COL2 COL3
     [1,]   1    1    0    0
     [2,]   2    1    0    0
     [3,]   3    1    1    1
     [4,]   4    2    2    1
     [5,]   5    2    3    1
     [6,]   6    2    3    3
     [7,]   7    3    3    4

     [8,]   8    4    4    4  Winner

     [9,]   9    4    5    4


    [10,]  10    5    5    5 Winner
    >

    %utl_submit_wps64('
    options set=R_HOME "C:/Program Files/R/R-3.3.2";
    libname wrk sas7bdat "%sysfunc(pathname(work))";
    proc r;
    submit;
    source("C:/Program Files/R/R-3.3.2/etc/Rprofile.site", echo=T);
    library(haven);
    slots=as.matrix(read_sas("d:/sd1/slots.sas7bdat"));
    winner<-apply(slots,2,sort);
    winner;
    endsubmit;
    import r=winner data=wrk.winner;
    run;quit;
    ');


    wor.winner total obs=10

    Obs    V1    V2    V3    V4

      1     1     0     0     0
      2     2     0     0     0
      3     3     0     0     0
      4     4     0     0     0
      5     5     1     0     1
      6     6     1     1     1
      7     7     1     1     1
      8     8     3     1     2
      9     9     4     2     3
     10    10     4     4     5


