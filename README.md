# utl_create_two_dimesional_array_statement
Generate a two dimensional datastep array statement from a proc freq crosstab output along with column and row text.

    ```  Create datastep array statement {6,3} ( 2 0 0 25 10 24 94 70 90 15 22 8 8 0 16 11 11 7 ) programaticaly                                                      ```
    ```                                                                                                                                                               ```
    ```    There are many very simple non-array solutions?                                                                                                            ```
    ```    However an array can do row and column statistics simultaneously.                                                                                          ```
    ```                                                                                                                                                               ```
    ```    This can be done more elegantly with DOSUBL at compile time, but I wrote this macro many years ago.                                                        ```
    ```    Compile time array and retain statements are a  better way to pass data along with some macro variables?                                                   ```
    ```                                                                                                                                                               ```
    ```     Compute rowsums                                                                                                                                           ```
    ```                                                                                                                                                               ```
    ```     Given                                                                                                                                                     ```
    ```                                                                                                                                                               ```
    ```         LABEL     ASIA    EUROPE    USA                                                                                                                       ```
    ```                                                                                                                                                               ```
    ```         Hybrid      2        0        0                                                                                                                       ```
    ```         SUV        25       10       24                                                                                                                       ```
    ```         Sedan      94       70       90                                                                                                                       ```
    ```         Sports     15       22        8                                                                                                                       ```
    ```         Truck       8        0       16                                                                                                                       ```
    ```         Wagon      11       11        7                                                                                                                       ```
    ```                                                                                                                                                               ```
    ```      Want                                                                                                                                                     ```
    ```                                                                                                                                                               ```
    ```        {6,3} ( 2 0 0 25 10 24 94 70 90 15 22 8 8 0 16 11 11 7 )                                                                                               ```
    ```                                                                                                                                                               ```
    ```        and                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```        WORK.WANT total obs=6                                                                                                                                  ```
    ```                                                                                                                                                               ```
    ```        Obs    ROWNAM    ROWTOT                                                                                                                                ```
    ```                                                                                                                                                               ```
    ```         1     Hybrid        2                                                                                                                                 ```
    ```         2     SUV          59                                                                                                                                 ```
    ```         3     Sedan       254                                                                                                                                 ```
    ```         4     Sports       45                                                                                                                                 ```
    ```         5     Truck        24                                                                                                                                 ```
    ```         6     Wagon        29                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```       WORKING CODE                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```        %Utl_MkeAry                                                                                                                                            ```
    ```          (                                                                                                                                                    ```
    ```           Utl_InDsn     = cars,                                                                                                                               ```
    ```           Utl_InRowVar  = type,                                                                                                                               ```
    ```           Utl_InColVar  = origin,                                                                                                                             ```
    ```           Utl_MacRetVar = numLst                                                                                                                              ```
    ```          );                                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```        /* macro return variable contains                                                                                                                      ```
    ```        {6,3} ( 2 0 0 25 10 24 94 70 90 15 22 8 8 0 16 11 11 7 )                                                                                               ```
    ```        */                                                                                                                                                     ```
    ```                                                                                                                                                               ```
    ```        data want;                                                                                                                                             ```
    ```                                                                                                                                                               ```
    ```          array mat&numLst ;                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```          do row=1 to dim(mat,1);                                                                                                                              ```
    ```                                                                                                                                                               ```
    ```            * retrieve the rownames from utl_mkeary - not good - DOSUBL compile time array better;                                                             ```
    ```            rowNam=symget(cats('row',put(row,1.)));                                                                                                            ```
    ```                                                                                                                                                               ```
    ```            do col=1 to dim(mat,2);                                                                                                                            ```
    ```               rowTot=sum(rowTot,mat[row,col]);                                                                                                                ```
    ```            end;                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```            keep rowNam rowTot;                                                                                                                                ```
    ```            output;                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```            rowTot=0;                                                                                                                                          ```
    ```                                                                                                                                                               ```
    ```          end;                                                                                                                                                 ```
    ```        run;quit;                                                                                                                                              ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  HAVE                                                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```     WORK.CARS total obs=413                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```       Obs    TYPE      ORIGIN                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```         1    SUV       Asia                                                                                                                                   ```
    ```         2    Sedan     Asia                                                                                                                                   ```
    ```         3    Sedan     Asia                                                                                                                                   ```
    ```         4    Sedan     Asia                                                                                                                                   ```
    ```         5    Sedan     Asia                                                                                                                                   ```
    ```         6    Sedan     Asia                                                                                                                                   ```
    ```         7    Sports    Asia                                                                                                                                   ```
    ```         8    Sedan     Europe                                                                                                                                 ```
    ```         9    Sedan     Europe                                                                                                                                 ```
    ```        10    Sedan     Europe                                                                                                                                 ```
    ```       ...    ...       ...                                                                                                                                    ```
    ```       409    SUV       Europe                                                                                                                                 ```
    ```       410    Sedan     Europe                                                                                                                                 ```
    ```       411    Sedan     Europe                                                                                                                                 ```
    ```       412    Sedan     Europe                                                                                                                                 ```
    ```       413    Wagon     Europe                                                                                                                                 ```
    ```                                                                                                                                                               ```
    ```  WANT                                                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```    WORK.WANT total obs=6                                                                                                                                      ```
    ```                                                                                                                                                               ```
    ```    Obs    ROWNAM    ROWTOT                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```     1     Hybrid        2                                                                                                                                     ```
    ```     2     SUV          59                                                                                                                                     ```
    ```     3     Sedan       254                                                                                                                                     ```
    ```     4     Sports       45                                                                                                                                     ```
    ```     5     Truck        24                                                                                                                                     ```
    ```     6     Wagon        29                                                                                                                                     ```
    ```                                                                                                                                                               ```
    ```  *                _               _       _                                                                                                                   ```
    ```   _ __ ___   __ _| | _____     __| | __ _| |_ __ _                                                                                                            ```
    ```  | '_ ` _ \ / _` | |/ / _ \   / _` |/ _` | __/ _` |                                                                                                           ```
    ```  | | | | | | (_| |   <  __/  | (_| | (_| | || (_| |                                                                                                           ```
    ```  |_| |_| |_|\__,_|_|\_\___|   \__,_|\__,_|\__\__,_|                                                                                                           ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  proc datasets lib=work kill;                                                                                                                                 ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  %symdel numLst row1 row2 row3 row4 row5 row6  col1 col2 col3 / nowarn;                                                                                       ```
    ```                                                                                                                                                               ```
    ```  data cars(keep=type origin);                                                                                                                                 ```
    ```    set sashelp.cars(where=(cylinders in (4,6,8)));                                                                                                            ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  *          _       _   _                                                                                                                                     ```
    ```   ___  ___ | |_   _| |_(_) ___  _ __                                                                                                                          ```
    ```  / __|/ _ \| | | | | __| |/ _ \| '_ \                                                                                                                         ```
    ```  \__ \ (_) | | |_| | |_| | (_) | | | |                                                                                                                        ```
    ```  |___/\___/|_|\__,_|\__|_|\___/|_| |_|                                                                                                                        ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  %Utl_MkeAry                                                                                                                                                  ```
    ```    (                                                                                                                                                          ```
    ```      Utl_InDsn     = cars                                                                                                                                     ```
    ```     ,Utl_InRowVar  = type                                                                                                                                     ```
    ```     ,Utl_InColVar  = origin                                                                                                                                   ```
    ```     ,Utl_MacRetVar = numLst                                                                                                                                   ```
    ```    );                                                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```  /* macro return variable contains                                                                                                                            ```
    ```  {6,3} ( 2 0 0 25 10 24 94 70 90 15 22 8 8 0 16 11 11 7 )                                                                                                     ```
    ```  */                                                                                                                                                           ```
    ```                                                                                                                                                               ```
    ```  data want;                                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```    array mat&numLst ;                                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```    do row=1 to dim(mat,1);                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```      * retrieve the rownames from utl_mkeary - not good - DOSUBL compile time array better;                                                                   ```
    ```      rowNam=symget(cats('row',put(row,1.)));                                                                                                                  ```
    ```                                                                                                                                                               ```
    ```      do col=1 to dim(mat,2);                                                                                                                                  ```
    ```         rowTot=sum(rowTot,mat[row,col]);                                                                                                                      ```
    ```      end;                                                                                                                                                     ```
    ```                                                                                                                                                               ```
    ```      keep rowNam rowTot;                                                                                                                                      ```
    ```      output;                                                                                                                                                  ```
    ```                                                                                                                                                               ```
    ```      rowTot=0;                                                                                                                                                ```
    ```                                                                                                                                                               ```
    ```    end;                                                                                                                                                       ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  1085  data want;                                                                                                                                             ```
    ```  SYMBOLGEN:  Macro variable NUMLST resolves to {6,3} ( 2 0 0 25 10 24 94 70 90 15 22 8 8 0 16 11 11 7 )                                                       ```
    ```  1086    array mat&numLst ;                                                                                                                                   ```
    ```  1087    do row=1 to dim(mat,1);                                                                                                                              ```
    ```  1088      * retrieve the rownames from utl_mkeary - not good - DOSUBL compile time array better;                                                             ```
    ```  1089      rowNam=symget(cats('row',put(row,1.)));                                                                                                            ```
    ```  1090      do col=1 to dim(mat,2);                                                                                                                            ```
    ```  1091         rowTot=sum(rowTot,mat[row,col]);                                                                                                                ```
    ```  1092      end;                                                                                                                                               ```
    ```  1093      keep rowNam rowTot;                                                                                                                                ```
    ```  1094      output;                                                                                                                                            ```
    ```  1095      rowTot=0;                                                                                                                                          ```
    ```  1096    end;                                                                                                                                                 ```
    ```  1097  run;                                                                                                                                                   ```
    ```                                                                                                                                                               ```
    ```  NOTE: The data set WORK.WANT has 6 observations and 2 variables.                                                                                             ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```
    ```  *                                                                                                                                                            ```
    ```   _ __ ___   __ _  ___ _ __ ___                                                                                                                               ```
    ```  | '_ ` _ \ / _` |/ __| '__/ _ \                                                                                                                              ```
    ```  | | | | | | (_| | (__| | | (_) |                                                                                                                             ```
    ```  |_| |_| |_|\__,_|\___|_|  \___/                                                                                                                              ```
    ```                                                                                                                                                               ```
    ```  ;                                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  %macro Utl_MkeAry                                                                                                                                            ```
    ```     (                                                                                                                                                         ```
    ```      Utl_InDsn,                                                                                                                                               ```
    ```      Utl_InRowVar,                                                                                                                                            ```
    ```      Utl_InColVar,                                                                                                                                            ```
    ```      Utl_MacRetVar=Utl_MkeAry                                                                                                                                 ```
    ```     ) / Des="Create an Array Statement with Freq counts";                                                                                                     ```
    ```                                                                                                                                                               ```
    ```    Ods Exclude All;                                                                                                                                           ```
    ```    Ods Output Observed=Utl_MkeAry01;                                                                                                                          ```
    ```    Proc Corresp Data=&Utl_InDsn                                                                                                                               ```
    ```                 Observed                                                                                                                                      ```
    ```                 dim=1;                                                                                                                                        ```
    ```         Table                                                                                                                                                 ```
    ```                 &Utl_InRowVar., &Utl_InColVar.;                                                                                                               ```
    ```    run;quit;                                                                                                                                                  ```
    ```    Ods Select All;                                                                                                                                            ```
    ```    Proc Print Data=Utl_MkeAry01;                                                                                                                              ```
    ```    Run;                                                                                                                                                       ```
    ```                                                                                                                                                               ```
    ```  * put row and column nams into macro variables;                                                                                                              ```
    ```  data _null_;                                                                                                                                                 ```
    ```     set Utl_MkeAry01;                                                                                                                                         ```
    ```     array nums _numeric_;                                                                                                                                     ```
    ```     call symputx(cats('row',put(_n_,5.)),label,'G');                                                                                                          ```
    ```     do _i_=1 to dim(nums);                                                                                                                                    ```
    ```       call symputx(cats('col',put(_i_,5.)),vname(nums[_i_]),'G');                                                                                             ```
    ```     end;                                                                                                                                                      ```
    ```  run;quit;                                                                                                                                                    ```
    ```                                                                                                                                                               ```
    ```  %Let Cols=%str();                                                                                                                                            ```
    ```  %Let Rows=%Str();                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```  Data _Null_;   /* Get Number of ROws and Columns */                                                                                                          ```
    ```    Set Work.Utl_MkeAry01 Nobs=Mobs;                                                                                                                           ```
    ```    Array Cols{*} _Numeric_;                                                                                                                                   ```
    ```    Call SymPut('Rows',Compress (Put(Mobs-1,9. )));                                                                                                            ```
    ```    Call SymPut('Cols',Compress (Put(Dim(Cols)-1,5.)));                                                                                                        ```
    ```    Stop;                                                                                                                                                      ```
    ```  Run;                                                                                                                                                         ```
    ```                                                                                                                                                               ```
    ```  %Put Rows=&Rows;                                                                                                                                             ```
    ```  %Put Cols=&Cols;                                                                                                                                             ```
    ```                                                                                                                                                               ```
    ```  Data _Null_;                                                                                                                                                 ```
    ```    Length MacStr $32700;                                                                                                                                      ```
    ```    Retain MacStr;                                                                                                                                             ```
    ```    Set Work.Utl_MkeAry01;                                                                                                                                     ```
    ```    Array Cols{*} _Numeric_;                                                                                                                                   ```
    ```    If _n_ =1 Then MacStr ="{&Rows.,&Cols.} ( ";                                                                                                               ```
    ```     Do J= 1 To Dim(Cols)-1;                                                                                                                                   ```
    ```      MacStr=Trim(MacStr)!!' '!!Compress(Put(Cols{J},9.));                                                                                                     ```
    ```     End;                                                                                                                                                      ```
    ```    If _n_ =&Rows. Then Do;                                                                                                                                    ```
    ```      MacStr=Trim(MacStr)!!' )';                                                                                                                               ```
    ```      /* do not change anything below this line */                                                                                                             ```
    ```      Call Symput(%UnQuote(%Bquote('&Utl_MacRetVar.')),Trim(MacStr));                                                                                          ```
    ```      Stop;                                                                                                                                                    ```
    ```    End;                                                                                                                                                       ```
    ```    run /* do not put a semicoln here */                                                                                                                       ```
    ```  %Exit:                                                                                                                                                       ```
    ```  %Mend Utl_MkeAry;                                                                                                                                            ```
    ```                                                                                                                                                               ```
    ```                                                                                                                                                               ```

