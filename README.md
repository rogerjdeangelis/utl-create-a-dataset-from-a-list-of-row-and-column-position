# utl-create-a-dataset-from-a-list-of-row-and-column-position
Create a dataset from a list of row and column position
    %let pgm=utl-create-a-dataset-from-a-list-of-row-and-column-position;

    %stop_submission;

             SOLUTIONS

                 1 r matrix
                 2 r base
                   Keintz, Mark
                   mkeintz@outlook.com

    This is problem best done with a marix language

    Create a dataset from a list of row and column position

    Op says data too large for IML. I have 128gb ram, so I wonder what large means?

    Note, I think the op wants the output matrix to be 3 x 4 not 4 x 4.
    There is not fourth row?

    guthub
    https://tinyurl.com/526wr8ar
    https://github.com/rogerjdeangelis/utl-create-a-dataset-from-a-list-of-row-and-column-position

    sas communities
    https://tinyurl.com/2w4rcrk2
    https://communities.sas.com/t5/SAS-Programming/How-do-I-create-a-data-set-from-a-list-of-row-and-column/td-p/789238


    /**************************************************************************************************************************/
    /*       INPUT            |      PROCESS                       |       OUTPUT                                             */
    /*       =====            |      ========                      |       ======                                             */
    /* SD1.HAVE  obs=3        | #create 3 x 4 matrix               | COL    COL    COL    COL                                 */
    /*                        | mat<-matrix(,                      |  0      1      2      3                                  */
    /*  R    C                |   nrow = max(have[,1])             |                                                          */
    /*                        |  ,ncol = max(have[,2]))            |  1      0      0      0                                  */
    /*  1    1                |                                    |  0      0      1      0                                  */
    /*  2    3                | # fill with 0s                     |  0      0      0      1                                  */
    /*  3    4                | mat[is.na(mat)] <- 0               |                                                          */
    /*                        |                                    |                                                          */
    /*                        | #pop with 1s                       |                                                          */
    /* options                | for (i in have[,1]) {              |                                                          */
    /*  validvarname=upcase;  |    mat[have[i,1],have[i,2]]=1      |                                                          */
    /* libname sd1 "d:/sd1";  |    }                               |                                                          */
    /* data sd1.have;         |                                    |                                                          */
    /* input r c;             |-----------------------------------------------------------------------------------------------*/
    /* cards4;                |1 R MATRIX                          |                                                          */
    /* 1 1                    |==========                          |                                                          */
    /* 2 3                    | proc datasets lib=sd1              | R                                                        */
    /* 3 4                    | nolist nodetails;                  |      [,1] [,2] [,3] [,4]                                 */
    /* ;;;;                   |  delete want;                      | [1,]    1    0    0    0                                 */
    /* run;quit;              | run;quit;                          | [2,]    0    0    1    0                                 */
    /*                        |                                    | [3,]    0    0    0    1                                 */
    /*                        | %utl_rbeginx;                      |                                                          */
    /*                        | parmcards4;                        |                                                          */
    /*                        | library(haven)                     | SAS SD1.HAVE obs=3                                       */
    /*                        | library(sqldf)                     |                                                          */
    /*                        | source("c:/oto/fn_tosas9x.R")      | COL    COL    COL    COL                                 */
    /*                        | have<-as.matrix(                   |  0      1      2      3                                  */
    /*                        |  read_sas("d:/sd1/have.sas7bdat")) |                                                          */
    /*                        | print(have)                        |  1      0      0      0                                  */
    /*                        | mat<-matrix(,                      |  0      0      1      0                                  */
    /*                        |   nrow = max(have[,1])             |  0      0      0      1                                  */
    /*                        |  ,ncol = max(have[,2]))            |                                                          */
    /*                        | mat[is.na(mat)] <- 0               |                                                          */
    /*                        | for (i in have[,1]) {              |                                                          */
    /*                        |    mat[have[i,1],have[i,2]]=1      |                                                          */
    /*                        |    }                               |                                                          */
    /*                        | mat                                |                                                          */
    /*                        | fn_tosas9x(                        |                                                          */
    /*                        |       inp    = mat                 |                                                          */
    /*                        |      ,outlib ="d:/sd1/"            |                                                          */
    /*                        |      ,outdsn ="want"               |                                                          */
    /*                        |      )                             |                                                          */
    /*                        | ;;;;                               |                                                          */
    /*                        | %utl_rendx;                        |                                                          */
    /*                        |                                    |                                                          */
    /*                        | proc print                         |                                                          */
    /*                        |    data=sd1.want split='_';        |                                                          */
    /*                        | run;quit;                          |                                                          */
    /*                        |                                    |                                                          */
    /*----------------------------------------------------------------------------------------------------------------------- */
    /*  data have;            | 2 BASE SAS                         |                                                          */
    /*   input r c;           | ==========                         | ROW  COL1  COL2   COL3   COL4   COL5   COL6              */
    /*  datalines;            |                                    |                                                          */
    /*  1 1                   | proc sql noprint;                  |   1    1     0      0      0      0      0               */
    /*  2 3                   |   select                           |   2    0     0      1      0      0      0               */
    /*  3 4                   |      max(r)                        |   3    0     0      0      1      0      0               */
    /*  8 2                   |     ,max(c)                        |   4    0     0      0      0      0      0               */
    /*  8 6                   |   into                             |   5    0     0      0      0      0      0               */
    /*  12 5                  |      :nrows                        |   6    0     0      0      0      0      0               */
    /*  run;                  |     ,:ncols                        |   7    0     0      0      0      0      0               */
    /*                        |   from                             |   8    0     1      0      0      0      1               */
    /*                        |       have;                        |   9    0     0      0      0      0      0               */
    /*                        | quit;                              |  10    0     0      0      0      0      0               */
    /*                        |                                    |  11    0     0      0      0      0      0               */
    /*                        | data zeroes / view=zeroes;         |  12    0     0      0      0      1      0               */
    /*                        |   array col {&ncols} (&ncols*0);   |                                                          */
    /*                        |   do row=1 to &nrows;              |                                                          */
    /*                        |     output;                        |                                                          */
    /*                        |   end;                             |                                                          */
    /*                        | run;                               |                                                          */
    /*                        |                                    |                                                          */
    /*                        | data want (keep=row col:);         |                                                          */
    /*                        |   update                           |                                                          */
    /*                        |     zeroes                         |                                                          */
    /*                        |     have (in=inh rename=(r=row));  |                                                          */
    /*                        |   by row;                          |                                                          */
    /*                        |   array col {&ncols} ;             |                                                          */
    /*                        |   if inh=1 then col{c}=1;          |                                                          */
    /*                        | run;                               |                                                          */
    /**************************************************************************************************************************/

    /*                          _        _
    / |  _ __   _ __ ___   __ _| |_ _ __(_)_  __
    | | | `__| | `_ ` _ \ / _` | __| `__| \ \/ /
    | | | |    | | | | | | (_| | |_| |  | |>  <
    |_| |_|    |_| |_| |_|\__,_|\__|_|  |_/_/\_\
     _                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */
    options
     validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
    input r c;
    cards4;
    1 1
    2 3
    3 4
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /* SD1.HAVE obs=3                                                                                                         */
    /*                                                                                                                        */
    /*  R    C                                                                                                                */
    /*                                                                                                                        */
    /*  1    1                                                                                                                */
    /*  2    3                                                                                                                */
    /*  3    4                                                                                                                */
    /**************************************************************************************************************************/

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    proc datasets lib=sd1
    nolist nodetails;
     delete want;
    run;quit;

    %utl_rbeginx;
    parmcards4;
    library(haven)
    library(sqldf)
    source("c:/oto/fn_tosas9x.R")
    have<-as.matrix(
     read_sas("d:/sd1/have.sas7bdat"))
    print(have)
    mat<-matrix(,
      nrow = max(have[,1])
     ,ncol = max(have[,2]))
    mat[is.na(mat)] <- 0
    for (i in have[,1]) {
       mat[have[i,1],have[i,2]]=1
       }
    mat
    fn_tosas9x(
          inp    = mat
         ,outlib ="d:/sd1/"
         ,outdsn ="want"
         )
    ;;;;
    %utl_rendx;

    proc print
       data=sd1.want split='_';
    run;quit;

    /**************************************************************************************************************************/
    /* R                        |  COL    COL    COL    COL                                                                   */
    /*      [,1] [,2] [,3] [,4] |   0      1      2      3                                                                    */
    /*                          |                                                                                             */
    /* [1,]    1    0    0    0 |   1      0      0      0                                                                    */
    /* [2,]    0    0    1    0 |   0      0      1      0                                                                    */
    /* [3,]    0    0    0    1 |   0      0      0      1                                                                    */
    /**************************************************************************************************************************/

    /*___           _
    |___ \   _ __  | |__   __ _ ___  ___   ___  __ _ ___
      __) | | `__| | `_ \ / _` / __|/ _ \ / __|/ _` / __|
     / __/  | |    | |_) | (_| \__ \  __/ \__ \ (_| \__ \
    |_____| |_|    |_.__/ \__,_|___/\___| |___/\__,_|___/
     _                   _
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|

    Mark Keintz

    I think this problem is a good use case for data set views
    (with all zeroes for COL vars) and the UPDATE statement:
    The dataset with the R and C variables are just
    "transactions" to be applied to the view:

    */

    data have;
      input r c;
    datalines;
    1 1
    2 3
    3 4
    8 2
    8 6
    12 5
    run;

    proc sql noprint;
      select
         max(r)
        ,max(c)
      into
         :nrows
        ,:ncols
      from
          have;
    quit;

    data zeroes / view=zeroes;
      array col {&ncols} (&ncols*0);
      do row=1 to &nrows;
        output;
      end;
    run;

    data want (keep=row col:);
      update
        zeroes
        have (in=inh rename=(r=row));
      by row;
      array col {&ncols} ;
      if inh=1 then col{c}=1;
    run;

    /**************************************************************************************************************************/
    /*  WORK.WANT total obs=12                                                                                                */
    /*                                                                                                                        */
    /*   COL1    COL2    COL3    COL4    COL5    COL6    ROW                                                                  */
    /*                                                                                                                        */
    /*     1       0       0       0       0       0       1                                                                  */
    /*     0       0       1       0       0       0       2                                                                  */
    /*     0       0       0       1       0       0       3                                                                  */
    /*     0       0       0       0       0       0       4                                                                  */
    /*     0       0       0       0       0       0       5                                                                  */
    /*     0       0       0       0       0       0       6                                                                  */
    /*     0       0       0       0       0       0       7                                                                  */
    /*     0       1       0       0       0       1       8                                                                  */
    /*     0       0       0       0       0       0       9                                                                  */
    /*     0       0       0       0       0       0      10                                                                  */
    /*     0       0       0       0       0       0      11                                                                  */
    /*     0       0       0       0       1       0      12                                                                  */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */

