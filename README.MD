# Oceandb Profiling Optimizaiton
## Guide
* https://johnysswlab.com/tune-your-programs-speed-with-profile-guided-optimizations/

## The file to enable PGO
* /path/to/oceanbase-master/deps/oblib/src/CMakeLists.txt
* /path/to/oceanbase-master/src/CMakeLists.txt
* /path/to/oceanbase-master/deps/easy/CMakeLists.txt
* /path/to/oceanbase-master/cmake/Env.cmake

## stop the oceanbase
kill -15 `pgrep observer`

## Sysbench Read Only
### Before PGO
#### --table_size=100000 --tables=30 --threads=64 --report-interval=10 --time=120 run
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

### During PGO
#### --table_size=100000 --tables=30 --threads=64 --report-interval=10 --time=60 run
[ 10s ] thds: 64 tps: 218.55 qps: 3567.25 (r/w/o: 3123.94/0.00/443.31) lat (ms,95%): 337.94 err/s: 0.00 reconn/s: 0.00<br />
[ 20s ] thds: 64 tps: 236.61 qps: 3785.02 (r/w/o: 3311.60/0.00/473.41) lat (ms,95%): 314.45 err/s: 0.00 reconn/s: 0.00<br />
[ 30s ] thds: 64 tps: 238.90 qps: 3824.67 (r/w/o: 3346.97/0.00/477.70) lat (ms,95%): 308.84 err/s: 0.00 reconn/s: 0.00<br />
[ 40s ] thds: 64 tps: 238.70 qps: 3818.16 (r/w/o: 3340.75/0.00/477.41) lat (ms,95%): 308.84 err/s: 0.00 reconn/s: 0.00<br />
[ 50s ] thds: 64 tps: 240.20 qps: 3842.15 (r/w/o: 3361.66/0.00/480.49) lat (ms,95%): 303.33 err/s: 0.00 reconn/s: 0.00<br />
[ 60s ] thds: 64 tps: 239.70 qps: 3830.96 (r/w/o: 3351.55/0.00/479.41) lat (ms,95%): 308.84 err/s: 0.00 reconn/s: 0.00<br />
<br />
SQL statistics: <br />
    queries performed: <br />
        read:                            198674 <br />
        write:                           0 <br />
        other:                           28382 <br />
        total:                           227056 <br />
    transactions:                        14191  (233.49 per sec.) <br />
    queries:                             227056 (3735.87 per sec.) <br />
    ignored errors:                      0      (0.00 per sec.) <br />
    reconnects:                          0      (0.00 per sec.) <br />
<br />
General statistics: <br />
    total time:                          60.7757s <br />
    total number of events:              14191 <br />
<br />
Latency (ms): <br />
         min:                                  170.45 <br />
         avg:                                  272.01 <br />
         max:                                 1703.77 <br />
         95th percentile:                      314.45 <br />
         sum:                              3860040.08 <br />
<br />
Threads fairness: <br />
    events (avg/stddev):           221.7344/2.43 <br />
    execution time (avg/stddev):   60.3131/0.19 <br />
    
### After PGO
#### --table_size=100000 --tables=30 --threads=64 --report-interval=10 --time=60 run

[ 10s ] thds: 64 tps: 15868.45 qps: 253948.12 (r/w/o: 222205.11/0.00/31743.00) lat (ms,95%): 4.82 err/s: 0.00 reconn/s: 0.00<br />
[ 20s ] thds: 64 tps: 16065.72 qps: 257061.14 (r/w/o: 224929.50/0.00/32131.64) lat (ms,95%): 4.65 err/s: 0.00 reconn/s: 0.00<br />
[ 30s ] thds: 64 tps: 15939.99 qps: 255035.99 (r/w/o: 223156.02/0.00/31879.97) lat (ms,95%): 4.74 err/s: 0.00 reconn/s: 0.00<br />
[ 40s ] thds: 64 tps: 16014.62 qps: 256237.08 (r/w/o: 224208.03/0.00/32029.05) lat (ms,95%): 4.65 err/s: 0.00 reconn/s: 0.00<br />
[ 50s ] thds: 64 tps: 15698.60 qps: 251181.93 (r/w/o: 219784.44/0.00/31397.49) lat (ms,95%): 4.82 err/s: 0.00 reconn/s: 0.00<br />
[ 60s ] thds: 64 tps: 16149.59 qps: 258377.99 (r/w/o: 226079.11/0.00/32298.88) lat (ms,95%): 4.65 err/s: 0.00 reconn/s: 0.00<br />
SQL statistics:<br />
    queries performed:<br />
        read:                            13408668<br />
        write:                           0<br />
        other:                           1915524<br />
        total:                           15324192<br />
    transactions:                        957762 (15956.52 per sec.)<br />
    queries:                             15324192 (255304.39 per sec.)<br />
    ignored errors:                      0      (0.00 per sec.)<br />
    reconnects:                          0      (0.00 per sec.)<br />
<br />
General statistics:<br />
    total time:                          60.0216s<br />
    total number of events:              957762<br />
<br />
Latency (ms):
         min:                                    3.00<br />
         avg:                                    4.01<br />
         max:                                   17.49<br />
         95th percentile:                        4.74<br />
         sum:                              3838528.83<br />
<br />
Threads fairness:<br />
    events (avg/stddev):           14965.0312/557.41<br />
    execution time (avg/stddev):   59.9770/0.00<br />


