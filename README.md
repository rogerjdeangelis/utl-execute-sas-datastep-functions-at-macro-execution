# utl-execute-sas-datastep-functions-at-macro-execution
Execute sas datastep functions at macro execution time

Execute sas datastep functions at macro execution time

Sum the values of a datastep array and put results into a macro variable at macro execution time

There is no sum(of x[*}) in the macro language

github
https://tinyurl.com/yybeqe5a
https://github.com/rogerjdeangelis/utl-execute-sas-datastep-functions-at-macro-execution

SAS Forum
https://tinyurl.com/y3szjy39
https://communities.sas.com/t5/SASware-Ballot-Ideas/Execute-SAS-functions-without-sysfunc/idi-p/553533

*_                   _
(_)_ __  _ __  _   _| |_
| | '_ \| '_ \| | | | __|
| | | | | |_) | |_| | |_
|_|_| |_| .__/ \__,_|\__|
        |_|
;

data have;
  array xs(3) x1-x3 (1,2,3);
run;quit;


Up to 40 obs WORK.HAVE total obs=1

Obs    X1    X2    X3     |  macro variable ans
                          |
 1      1     2     3     |  6 = sum(x1,x2,x3)

*            _               _
  ___  _   _| |_ _ __  _   _| |_
 / _ \| | | | __| '_ \| | | | __|
| (_) | |_| | |_| |_) | |_| | |_
 \___/ \__,_|\__| .__/ \__,_|\__|
                |_|
;

* Nacro variable tot;

%put &=ans;

ANS=6

*
 _ __  _ __ ___   ___ ___  ___ ___
| '_ \| '__/ _ \ / __/ _ \/ __/ __|
| |_) | | | (_) | (_|  __/\__ \__ \
| .__/|_|  \___/ \___\___||___/___/
|_|
;

%symdel tot / nowarn;
%macro tot(inp);

  %let rc=%sysfunc(dosubl('
      data ab;
         set have;
         array xs(3) x1-x3 (1,2,3);
         call symputx("tot",sum(of xs[*]));
  '));
   &tot
%mend tot;


%let ans=%tot(have);
%put &=ans;


457   %symdel tot / nowarn;
458   %macro tot(inp);
459     %let rc=%sysfunc(dosubl('
460         data ab;
461            set have;
462            array xs(3) x1-x3 (1,2,3);
463            call symputx("tot",sum(of xs[*]));
464     '));
465      &tot
466   %mend tot;
467   %let ans=%tot(have);
MLOGIC(TOT):  Beginning execution.
NOTE: There were 1 observations read from the data set WORK.HAVE.
NOTE: The data set WORK.AB has 1 observations and 3 variables.
NOTE: DATA statement used (Total process time):
      real time           0.00 seconds
      user cpu time       0.00 seconds
      system cpu time     0.00 seconds
      memory              503.71k
      OS Memory           17640.00k
      Timestamp           04/24/2019 01:49:52 PM
      Step Count                        170  Switch Count  0


MLOGIC(TOT):  Parameter INP has value have
MLOGIC(TOT):  %LET (variable name is RC)
SYMBOLGEN:  Macro variable TOT resolves to 6
MLOGIC(TOT):  Ending execution.


468   %put &=ans;
SYMBOLGEN:  Macro variable ANS resolves to 6
ANS=6


