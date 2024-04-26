# utl-sas-r-inverted-bar-chart-with-lower-curve-plot-rainfall-runoff-plot
    %let pgm=utl-sas-r-inverted-bar-chart-with-lower-curve-plot-rainfall-runoff-plot;

    r inverted bar chart with lower curve plot rainfall runoff plot

       Two Solutions

           1 sas sgplot
           2 r par plot

    github
    https://tinyurl.com/4vmsvnej
    https://github.com/rogerjdeangelis/utl-sas-r-inverted-bar-chart-with-lower-curve-plot-rainfall-runoff-plot

    R plot
    https://tinyurl.com/mr33te8h
    https://github.com/rogerjdeangelis/utl-sas-r-inverted-bar-chart-with-lower-curve-plot-rainfall-runoff-plot/blob/main/rain_r.pdf

    inspired by
    https://goo.gl/2G7kxI
    http://stackoverflow.com/questions/42057832/how-to-draw-rainfall-runoff-graph-in-r-using-ggplot

    /*               _     _
     _ __  _ __ ___ | |__ | | ___ _ __ ___
    | `_ \| `__/ _ \| `_ \| |/ _ \ `_ ` _ \
    | |_) | | | (_) | |_) | |  __/ | | | | |
    | .__/|_|  \___/|_.__/|_|\___|_| |_| |_|
    |_|
    */

    /********************************************************************************************************************************/
    /*                             |                                      |                                                         */
    /*          INPUT              |          PROCESS                     |                        OUTPUT                           */
    /*          =====              |          =======                     |                        ======                           */
    /*                             |                                      |                                                         */
    /* 1 SAS SGPLOT                |                                      |                                                         */
    /* ============                |                                      |                                                         */
    /*                             |                                      |                                                         */
    /*  SD1.HAVE total obs=10      | data neg;                            |                   Rainfall and Runoff                   */
    /*                             |   set "d:/sd1/have.sas7bdat";        |     0        2        4  DAY      6        8     10     */
    /*    DATES    FLOW  RAIN      |   rain=-rain;                        |    -+--------+--------+--------+--------+--------+--+   */
    /*                             | run;quit;                            |    +XX        XX        XX        XX         XX     + 0 */
    /*  01JAN2015    0     0       |                                      |    |XX        XX        XX        XX         XX     |   */
    /*  02JAN2015    0     1       | ods pdf file="d:/pdf/rain_sas.pdf" ; |    |XX        XX        XX        XX         XX     |   */
    /*  03JAN2015    1     2       | ods graphics on / width=9in          |    +XX        XX        XX        XX         XX     +   */
    /*  04JAN2015    3     5       |  height=6in ;                        |    |XX        XX        XX        XX                |   */
    /*  05JAN2015    7     4       |                                      |    |XX        XX        XX        XX                |   */
    /*  06JAN2015   11     0       | proc sgplot data=neg;                |    +XX        XX        XX        XX                + 1 */
    /*  07JAN2015    8     0       |   title "Rainfall";                  |    |XX        XX        XX        XX                |   */
    /*  08JAN2015    6     0       |    vline dates                       |    |XX        XX        XX        XX                | R */
    /*  09JAN2015    4     1       |       / response=flow y2axis;        |    +XX        XX        XX        XX                + A */
    /*  10JAN2015    5     0       |    vbar dates                        |    |XX        XX        XX        XX                | I */
    /*                             |       / response=rain;               |    |XX        XX        XX                          | N */
    /* options validvarname=upcase;|    yaxis values=(0 -1 -2 -3 -4 -5)   |  10+XX        XX                                    + 2 */
    /* libname sd1 "d:/sd1";       |      valuesdisplay=                  |    |XX        XX        *************               |   */
    /* data sd1.have;              |     ('0' '1' '2' '3' '4' '5');       |    |XX        XX      ****          ****            |   */
    /* retain dates "01JAN2015"d;  | run;quit;                            |   9+XX        XX   ***              ***             +   */
    /* format dates date9.;        | ods pdf close;                       |    |XX        XX ***                  ***           |   */
    /* input FLOW RAIN @@;         |                                      | F  |XX        XX**                      ***         |   */
    /* output;                     |                                      | L 8+XX        **                             **     + 3 */
    /* dates=dates+1;              |                                      | O  |XX       ***                               **   |   */
    /* cards4;                     |                                      | W  |XX     ** XX                                **  |   */
    /* 0 0 0 1 1 2 3 5 7 4 11 0    |                                      |   7+XX    **  XX                                  **+   */
    /* 8 0 6 0 4 1 5 0             |                                      |    |XX  **    XX                                   *|   */
    /* ;;;;                        |                                      |    |XX **     XX                                    |   */
    /* run;quit;                   |                                      |    -+--------+--------+--------+--------+--------+---   */
    /*                             |                                      |     0        2        4  DAY      6        8     10     */
    /* 2 R PAR PLOT                |                                      |                                                         */
    /*                             |                                      |                                                         */
    /*                             |                                      |                                                         */
    /*                             |                                      |                   Rainfall and Runoff                   */
    /*                             |                                      |    0        2        4  DAY      6        8     10      */
    /*   SD1.HAVE total obs=10     | %utl_submit_r64x('                   |   -+--------+--------+--------+--------+--------+--+    */
    /*                             | library(haven);                      |   +XX        XX        XX        XX         XX     + 0  */
    /*     DATES    FLOW  RAIN     | have<-read_sas                       |   |XX        XX        XX        XX         XX     |    */
    /*                             |   ("d:/sd1/have.sas7bdat");          |   |XX        XX        XX        XX         XX     |    */
    /*   01JAN2015    0     0      | have$DATES<-as.Date(have$DATES);     |   +XX        XX        XX        XX         XX     +    */
    /*   02JAN2015    0     1      | pdf("d:/pdf/rain_r.pdf",7,6);        |   |XX        XX        XX        XX                |    */
    /*   03JAN2015    1     2      | par(xaxs="i", yaxs="i"               |   |XX        XX        XX        XX                |    */
    /*   04JAN2015    3     5      |  ,mar=c(5,5,5,5));                   |   +XX        XX        XX        XX                + 1  */
    /*   05JAN2015    7     4      | plot(have$DATES                      |   |XX        XX        XX        XX                |    */
    /*   06JAN2015   11     0      |   ,have$RAIN                         |   |XX        XX        XX        XX                | R  */
    /*   07JAN2015    8     0      |   ,type="h"                          |   +XX        XX        XX        XX                + A  */
    /*   08JAN2015    6     0      |   ,ylim=c(max(have$RAIN)*1.5,0),     |   |XX        XX        XX        XX                | I  */
    /*   09JAN2015    4     1      |  axes=FALSE, xlab=NA, ylab=NA        |   |XX        XX        XX                          | N  */
    /*   10JAN2015    5     0      |                         ,col="blue", | 10+XX        XX                                    + 2  */
    /*                             |  lwd=50, lend="square");             |   |XX        XX        *************               |    */
    /*                             | axis(4);                             |   |XX        XX      ****          ****            |    */
    /*                             | mtext("Rainfall", side=4, line=3);   |  9+XX        XX   ***              ***             +    */
    /*                             | par(new=TRUE);                       |   |XX        XX ***                  ***           |    */
    /*                             | plot(have$DATES                      |F  |XX        XX**                      ***         |    */
    /*                             |   ,have$FLOW                         |L 8+XX        **                             **     + 3  */
    /*                             |   ,type="l"                          |O  |XX       ***                               **   |    */
    /*                             |   ,lwd=2                             |W  |XX     ** XX                                **  |    */
    /*                             |   ,ylim=c(0                          |  7+XX    **  XX                                  **+    */
    /*                             |   ,max(have$FLOW)*1.2));             |   |XX  **    XX                                   *|    */
    /*                             | ');                                  |   |XX **     XX                                    |    */
    /*                             |                                      |   -+--------+--------+--------+--------+--------+---    */
    /*                             |                                      |    0        2        4  DAY      6        8     10      */
    /*                             |                                      |                                                         */
    /*                             |                                      |                                                         */
    /******************************|**************************************|**********************************************************/


    https://github.com/rogerjdeangelis/utl-join-yearly-rainfall-in-one-table-with-yearly-temperature-in-another-using-normalization


    Simple and cleaner in SAS messy in R?

    inspired by
    https://goo.gl/2G7kxI
    http://stackoverflow.com/questions/42057832/how-to-draw-rainfall-runoff-graph-in-r-using-ggplot

    /*                             _       _
    / |  ___  __ _ ___   ___ _ __ | | ___ | |_
    | | / __|/ _` / __| / __| `_ \| |/ _ \| __|
    | | \__ \ (_| \__ \ \__ \ |_) | | (_) | |_
    |_| |___/\__,_|___/ |___/ .__/|_|\___/ \__|
     _                   _  |_|
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */

    *create some data;
    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
    retain dates "01JAN2015"d;
    format dates date9.;
    input FLOW RAIN @@;
    output;
    dates=dates+1;
    cards4;
    0 0 0 1 1 2 3 5 7 4 11 0
    8 0 6 0 4 1 5 0
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*                                                                                                                        */
    /* SD1.HAVE  total obs=10                                                                                                 */
    /*                                                                                                                        */
    /* Obs      DATES      FLOW  RAIN                                                                                         */
    /*                                                                                                                        */
    /*   1    01JAN2015      0       0                                                                                        */
    /*   2    02JAN2015      0       1                                                                                        */
    /*   3    03JAN2015      1       2                                                                                        */
    /*   4    04JAN2015      3       5                                                                                        */
    /*   5    05JAN2015      7       4                                                                                        */
    /*   6    06JAN2015     11       0                                                                                        */
    /*   7    07JAN2015      8       0                                                                                        */
    /*   8    08JAN2015      6       0                                                                                        */
    /*   9    09JAN2015      4       1                                                                                        */
    /*  10    10JAN2015      5       0                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*
     _ __  _ __ ___   ___ ___  ___ ___
    | `_ \| `__/ _ \ / __/ _ \/ __/ __|
    | |_) | | | (_) | (_|  __/\__ \__ \
    | .__/|_|  \___/ \___\___||___/___/
    |_|
    */

    data neg;
      set "d:/sd1/have.sas7bdat";
      rain=-rain;
    run;quit;

    ods pdf file="d:/pdf/rain_sas.pdf" ;
    ods graphics on / width=9in
     height=6in ;

    proc sgplot data=neg;
      title "Rainfall";
       vline dates
          / response=flow y2axis;
       vbar dates
          / response=rain;
       yaxis values=(0 -1 -2 -3 -4 -5)
         valuesdisplay=
        ('0' '1' '2' '3' '4' '5');
    run;quit;
    ods pdf close;

    /**************************************************************************************************************************/ /
    /*                                                                                                                        */
    /*                     Rainfall and Runoff                                                                                */
    /*      0        2        4  DAY      6        8     10                                                                   */
    /*     -+--------+--------+--------+--------+--------+--+                                                                 */
    /*     +XX        XX        XX        XX         XX     + 0                                                               */
    /*     |XX        XX        XX        XX         XX     |                                                                 */
    /*     |XX        XX        XX        XX         XX     |                                                                 */
    /*     +XX        XX        XX        XX         XX     +                                                                 */
    /*     |XX        XX        XX        XX                |                                                                 */
    /*     |XX        XX        XX        XX                |                                                                 */
    /*     +XX        XX        XX        XX                + 1                                                               */
    /*     |XX        XX        XX        XX                |                                                                 */
    /*     |XX        XX        XX        XX                | R                                                               */
    /*     +XX        XX        XX        XX                + A                                                               */
    /*     |XX        XX        XX        XX                | I                                                               */
    /*     |XX        XX        XX                          | N                                                               */
    /*   10+XX        XX                                    + 2                                                               */
    /*     |XX        XX        *************               |                                                                 */
    /*     |XX        XX      ****          ****            |                                                                 */
    /*    9+XX        XX   ***              ***             +                                                                 */
    /*     |XX        XX ***                  ***           |                                                                 */
    /*  F  |XX        XX**                      ***         |                                                                 */
    /*  L 8+XX        **                             **     + 3                                                               */
    /*  O  |XX       ***                               **   |                                                                 */
    /*  W  |XX     ** XX                                **  |                                                                 */
    /*    7+XX    **  XX                                  **+                                                                 */
    /*     |XX  **    XX                                   *|                                                                 */
    /*     |XX **     XX                                    |                                                                 */
    /*     -+--------+--------+--------+--------+--------+---                                                                 */
    /*      0        2        4  DAY      6        8     10                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*___                                    _       _
    |___ \   _ __   _ __   __ _ _ __   _ __ | | ___ | |_
      __) | | `__| | `_ \ / _` | `__| | `_ \| |/ _ \| __|
     / __/  | |    | |_) | (_| | |    | |_) | | (_) | |_
    |_____| |_|    | .__/ \__,_|_|    | .__/|_|\___/ \__|
                   |_|   _            |_|
    (_)_ __  _ __  _   _| |_
    | | `_ \| `_ \| | | | __|
    | | | | | |_) | |_| | |_
    |_|_| |_| .__/ \__,_|\__|
            |_|
    */


    *create some data;
    options validvarname=upcase;
    libname sd1 "d:/sd1";
    data sd1.have;
    retain dates "01JAN2015"d;
    format dates date9.;
    input FLOW RAIN @@;
    output;
    dates=dates+1;
    cards4;
    0 0 0 1 1 2 3 5 7 4 11 0
    8 0 6 0 4 1 5 0
    ;;;;
    run;quit;

    /**************************************************************************************************************************/
    /*                                                                                                                        */
    /*                                                                                                                        */
    /* SD1.HAVE  total obs=10                                                                                                 */
    /*                                                                                                                        */
    /* Obs      DATES      FLOW  RAIN                                                                                         */
    /*                                                                                                                        */
    /*   1    01JAN2015      0       0                                                                                        */
    /*   2    02JAN2015      0       1                                                                                        */
    /*   3    03JAN2015      1       2                                                                                        */
    /*   4    04JAN2015      3       5                                                                                        */
    /*   5    05JAN2015      7       4                                                                                        */
    /*   6    06JAN2015     11       0                                                                                        */
    /*   7    07JAN2015      8       0                                                                                        */
    /*   8    08JAN2015      6       0                                                                                        */
    /*   9    09JAN2015      4       1                                                                                        */
    /*  10    10JAN2015      5       0                                                                                        */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    %utl_submit_r64x('
    library(haven);
    have<-read_sas("d:/sd1/have.sas7bdat");
    have$DATES<-as.Date(have$DATES);
    pdf("d:/pdf/rain_r.pdf",7,6);
    par(xaxs="i", yaxs="i"
     ,mar=c(5,5,5,5));
    plot(have$DATES
      ,have$RAIN
      ,type="h"
      ,ylim=c(max(have$RAIN)*1.5,0),
     axes=FALSE, xlab=NA, ylab=NA
      ,col="blue",
     lwd=50, lend="square");
    axis(4);
    mtext("Rainfall", side=4, line=3);
    par(new=TRUE);
    plot(have$DATES
      ,have$FLOW
      ,type="l"
      ,lwd=2
      ,ylim=c(0
      ,max(have$FLOW)*1.2));
    ');


    /**************************************************************************************************************************/ /
    /*                                                                                                                        */
    /*                     Rainfall and Runoff                                                                                */
    /*      0        2        4  DAY      6        8     10                                                                   */
    /*     -+--------+--------+--------+--------+--------+--+                                                                 */
    /*     +XX        XX        XX        XX         XX     + 0                                                               */
    /*     |XX        XX        XX        XX         XX     |                                                                 */
    /*     |XX        XX        XX        XX         XX     |                                                                 */
    /*     +XX        XX        XX        XX         XX     +                                                                 */
    /*     |XX        XX        XX        XX                |                                                                 */
    /*     |XX        XX        XX        XX                |                                                                 */
    /*     +XX        XX        XX        XX                + 1                                                               */
    /*     |XX        XX        XX        XX                |                                                                 */
    /*     |XX        XX        XX        XX                | R                                                               */
    /*     +XX        XX        XX        XX                + A                                                               */
    /*     |XX        XX        XX        XX                | I                                                               */
    /*     |XX        XX        XX                          | N                                                               */
    /*   10+XX        XX                                    + 2                                                               */
    /*     |XX        XX        *************               |                                                                 */
    /*     |XX        XX      ****          ****            |                                                                 */
    /*    9+XX        XX   ***              ***             +                                                                 */
    /*     |XX        XX ***                  ***           |                                                                 */
    /*  F  |XX        XX**                      ***         |                                                                 */
    /*  L 8+XX        **                             **     + 3                                                               */
    /*  O  |XX       ***                               **   |                                                                 */
    /*  W  |XX     ** XX                                **  |                                                                 */
    /*    7+XX    **  XX                                  **+                                                                 */
    /*     |XX  **    XX                                   *|                                                                 */
    /*     |XX **     XX                                    |                                                                 */
    /*     -+--------+--------+--------+--------+--------+---                                                                 */
    /*      0        2        4  DAY      6        8     10                                                                   */
    /*                                                                                                                        */
    /**************************************************************************************************************************/

    /*              _
      ___ _ __   __| |
     / _ \ `_ \ / _` |
    |  __/ | | | (_| |
     \___|_| |_|\__,_|

    */
r inverted bar chart with lower curve plot rainfall runoff plot  
