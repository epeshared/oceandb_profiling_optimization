# OceanBase Profiling Optimizaiton
## Guide
* https://johnysswlab.com/tune-your-programs-speed-with-profile-guided-optimizations/

## The file to enable PGO
* /path/to/oceanbase-master/deps/oblib/src/CMakeLists.txt
* /path/to/oceanbase-master/src/CMakeLists.txt
* /path/to/oceanbase-master/deps/easy/CMakeLists.txt
* /path/to/oceanbase-master/cmake/Env.cmake

## stop the oceanbase
<pre>kill -15 `pgrep observer`</pre>

## Sysbench Read Only
### Before PGO
#### --table_size=100000 --tables=30 --threads=64 --report-interval=10 --time=120 run
<pre>
[ 10s ] thds: 64 tps: 14606.87 qps: 233769.08 (r/w/o: 204549.25/0.00/29219.84) lat (ms,95%): 5.09 err/s: 0.00 reconn/s: 0.00
[ 20s ] thds: 64 tps: 14507.08 qps: 232118.00 (r/w/o: 203103.84/0.00/29014.16) lat (ms,95%): 5.09 err/s: 0.00 reconn/s: 0.00
[ 30s ] thds: 64 tps: 14314.29 qps: 229016.48 (r/w/o: 200388.00/0.00/28628.49) lat (ms,95%): 5.18 err/s: 0.00 reconn/s: 0.00
[ 40s ] thds: 64 tps: 14549.59 qps: 232803.66 (r/w/o: 203704.18/0.00/29099.48) lat (ms,95%): 5.09 err/s: 0.00 reconn/s: 0.00
[ 50s ] thds: 64 tps: 14465.98 qps: 231451.32 (r/w/o: 202519.57/0.00/28931.75) lat (ms,95%): 5.18 err/s: 0.00 reconn/s: 0.00
[ 60s ] thds: 64 tps: 14412.80 qps: 230605.96 (r/w/o: 201780.05/0.00/28825.91) lat (ms,95%): 5.18 err/s: 0.00 reconn/s: 0.00
SQL statistics:
    queries performed:
        read:                            12161212
        write:                           0
        other:                           1737316
        total:                           13898528
    transactions:                        868658 (14472.15 per sec.)
    queries:                             13898528 (231554.42 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          60.0211s
    total number of events:              868658

Latency (ms):
         min:                                    3.34
         avg:                                    4.42
         max:                                   49.94
         95th percentile:                        5.18
         sum:                              3838660.33

Threads fairness:
    events (avg/stddev):           13572.7812/362.85
    execution time (avg/stddev):   59.9791/0.00
</pre>
### During PGO
#### --table_size=100000 --tables=30 --threads=64 --report-interval=10 --time=60 run
<pre>
[ 10s ] thds: 64 tps: 218.55 qps: 3567.25 (r/w/o: 3123.94/0.00/443.31) lat (ms,95%): 337.94 err/s: 0.00 reconn/s: 0.00
[ 20s ] thds: 64 tps: 236.61 qps: 3785.02 (r/w/o: 3311.60/0.00/473.41) lat (ms,95%): 314.45 err/s: 0.00 reconn/s: 0.00
[ 30s ] thds: 64 tps: 238.90 qps: 3824.67 (r/w/o: 3346.97/0.00/477.70) lat (ms,95%): 308.84 err/s: 0.00 reconn/s: 0.00
[ 40s ] thds: 64 tps: 238.70 qps: 3818.16 (r/w/o: 3340.75/0.00/477.41) lat (ms,95%): 308.84 err/s: 0.00 reconn/s: 0.00
[ 50s ] thds: 64 tps: 240.20 qps: 3842.15 (r/w/o: 3361.66/0.00/480.49) lat (ms,95%): 303.33 err/s: 0.00 reconn/s: 0.00
[ 60s ] thds: 64 tps: 239.70 qps: 3830.96 (r/w/o: 3351.55/0.00/479.41) lat (ms,95%): 308.84 err/s: 0.00 reconn/s: 0.00

SQL statistics: 
    queries performed: 
        read:                            198674 
        write:                           0 
        other:                           28382 
        total:                           227056 
    transactions:                        14191  (233.49 per sec.) 
    queries:                             227056 (3735.87 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          60.7757s
    total number of events:              14191 

Latency (ms): 
         min:                                  170.45
         avg:                                  272.01
         max:                                 1703.77
         95th percentile:                      314.45
         sum:                              3860040.08

Threads fairness:
    events (avg/stddev):           221.7344/2.43 
    execution time (avg/stddev):   60.3131/0.19
</pre>    
### After PGO
#### --table_size=100000 --tables=30 --threads=64 --report-interval=10 --time=60 run
<pre>
[ 10s ] thds: 64 tps: 15868.45 qps: 253948.12 (r/w/o: 222205.11/0.00/31743.00) lat (ms,95%): 4.82 err/s: 0.00 reconn/s: 0.00
[ 20s ] thds: 64 tps: 16065.72 qps: 257061.14 (r/w/o: 224929.50/0.00/32131.64) lat (ms,95%): 4.65 err/s: 0.00 reconn/s: 0.00
[ 30s ] thds: 64 tps: 15939.99 qps: 255035.99 (r/w/o: 223156.02/0.00/31879.97) lat (ms,95%): 4.74 err/s: 0.00 reconn/s: 0.00
[ 40s ] thds: 64 tps: 16014.62 qps: 256237.08 (r/w/o: 224208.03/0.00/32029.05) lat (ms,95%): 4.65 err/s: 0.00 reconn/s: 0.00
[ 50s ] thds: 64 tps: 15698.60 qps: 251181.93 (r/w/o: 219784.44/0.00/31397.49) lat (ms,95%): 4.82 err/s: 0.00 reconn/s: 0.00
[ 60s ] thds: 64 tps: 16149.59 qps: 258377.99 (r/w/o: 226079.11/0.00/32298.88) lat (ms,95%): 4.65 err/s: 0.00 reconn/s: 0.00
SQL statistics:
    queries performed:
        read:                            13408668
        write:                           0
        other:                           1915524
        total:                           15324192
    transactions:                        957762 (15956.52 per sec.)
    queries:                             15324192 (255304.39 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          60.0216s
    total number of events:              957762

Latency (ms):
         min:                                    3.00
         avg:                                    4.01
         max:                                   17.49
         95th percentile:                        4.74
         sum:                              3838528.83

Threads fairness:
    events (avg/stddev):           14965.0312/557.41
    execution time (avg/stddev):   59.9770/0.00
</pre>    
#### --table_size=100000 --tables=30 --threads=64 --report-interval=10 --time=180 run
<pre>
[ 10s ] thds: 64 tps: 18160.00 qps: 290624.56 (r/w/o: 254298.26/0.00/36326.30) lat (ms,95%): 4.10 err/s: 0.00 reconn/s: 0.00
[ 20s ] thds: 64 tps: 18545.44 qps: 296710.87 (r/w/o: 259620.10/0.00/37090.77) lat (ms,95%): 3.96 err/s: 0.00 reconn/s: 0.00
[ 30s ] thds: 64 tps: 18575.04 qps: 297206.19 (r/w/o: 260056.51/0.00/37149.67) lat (ms,95%): 3.96 err/s: 0.00 reconn/s: 0.00
[ 40s ] thds: 64 tps: 18615.83 qps: 297854.43 (r/w/o: 260622.47/0.00/37231.97) lat (ms,95%): 3.96 err/s: 0.00 reconn/s: 0.00
[ 50s ] thds: 64 tps: 18639.68 qps: 298235.33 (r/w/o: 260955.67/0.00/37279.67) lat (ms,95%): 3.96 err/s: 0.00 reconn/s: 0.00
[ 60s ] thds: 64 tps: 18671.72 qps: 298747.40 (r/w/o: 261404.25/0.00/37343.15) lat (ms,95%): 3.96 err/s: 0.00 reconn/s: 0.00
[ 70s ] thds: 64 tps: 18635.40 qps: 298167.88 (r/w/o: 260897.19/0.00/37270.70) lat (ms,95%): 3.96 err/s: 0.00 reconn/s: 0.00
[ 80s ] thds: 64 tps: 18622.51 qps: 297962.78 (r/w/o: 260717.46/0.00/37245.32) lat (ms,95%): 3.96 err/s: 0.00 reconn/s: 0.00
[ 90s ] thds: 64 tps: 18556.84 qps: 296901.80 (r/w/o: 259788.22/0.00/37113.57) lat (ms,95%): 4.03 err/s: 0.00 reconn/s: 0.00
[ 100s ] thds: 64 tps: 18563.41 qps: 297021.43 (r/w/o: 259894.70/0.00/37126.73) lat (ms,95%): 3.96 err/s: 0.00 reconn/s: 0.00
[ 110s ] thds: 64 tps: 18573.22 qps: 297165.81 (r/w/o: 260019.67/0.00/37146.14) lat (ms,95%): 3.96 err/s: 0.00 reconn/s: 0.00
[ 120s ] thds: 64 tps: 18647.29 qps: 298358.47 (r/w/o: 261063.49/0.00/37294.98) lat (ms,95%): 3.96 err/s: 0.00 reconn/s: 0.00
[ 130s ] thds: 64 tps: 18593.56 qps: 297496.61 (r/w/o: 260309.48/0.00/37187.13) lat (ms,95%): 3.96 err/s: 0.00 reconn/s: 0.00
[ 140s ] thds: 64 tps: 18572.31 qps: 297162.86 (r/w/o: 260018.14/0.00/37144.72) lat (ms,95%): 3.96 err/s: 0.00 reconn/s: 0.00
[ 150s ] thds: 64 tps: 18590.42 qps: 297446.78 (r/w/o: 260265.95/0.00/37180.84) lat (ms,95%): 3.96 err/s: 0.00 reconn/s: 0.00
[ 160s ] thds: 64 tps: 18596.54 qps: 297536.99 (r/w/o: 260344.40/0.00/37192.59) lat (ms,95%): 3.96 err/s: 0.00 reconn/s: 0.00
[ 170s ] thds: 64 tps: 18613.44 qps: 297814.57 (r/w/o: 260587.29/0.00/37227.28) lat (ms,95%): 3.96 err/s: 0.00 reconn/s: 0.00
[ 180s ] thds: 64 tps: 18594.60 qps: 297520.47 (r/w/o: 260331.07/0.00/37189.40) lat (ms,95%): 3.96 err/s: 0.00 reconn/s: 0.00
SQL statistics:
    queries performed:
        read:                            46812808
        write:                           0
        other:                           6687544
        total:                           53500352
    transactions:                        3343772 (18574.90 per sec.)
    queries:                             53500352 (297198.43 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          180.0140s
    total number of events:              3343772

Latency (ms):
         min:                                    2.56
         avg:                                    3.44
         max:                                   38.18
         95th percentile:                        3.96
         sum:                             11514792.69

Threads fairness:
    events (avg/stddev):           52246.4375/2535.84
    execution time (avg/stddev):   179.9186/0.01
</pre>



