# utl-create-a-dataset-from-a-list-of-row-and-column-position
Create a dataset from a list of row and column position
    %let pgm=utl-create-a-dataset-from-a-list-of-row-and-column-position;

    %stop_submission;

    This is problem best done with a marix language

    Create a dataset from a list of row and column position

    Op says data too large for IML. I have 128gb ram so I wonder what large means?

    Note, I think the op wantsthe output matrix to be 3 x 4 not 4 x 4.
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
    /* cards4;                |                                    |                                                          */
    /* 1 1                    | proc datasets lib=sd1              | R                                                        */
    /* 2 3                    | nolist nodetails;                  |      [,1] [,2] [,3] [,4]                                 */
    /* 3 4                    |  delete want;                      | [1,]    1    0    0    0                                 */
    /* ;;;;                   | run;quit;                          | [2,]    0    0    1    0                                 */
    /* run;quit;              |                                    | [3,]    0    0    0    1                                 */
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
    /**************************************************************************************************************************/

    /*                   _
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

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
