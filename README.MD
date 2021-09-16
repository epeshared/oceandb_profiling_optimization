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

### --table_size=1000 --tables=3 --threads=3 --report-interval=3 --time=30 run
SQL statistics:
    queries performed:
        read:                            464464
        write:                           0
        other:                           66352
        total:                           530816
    transactions:                        33176  (1105.68 per sec.)
    queries:                             530816 (17690.84 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          30.0035s
    total number of events:              33176

Latency (ms):
         min:                                    2.17
         avg:                                    2.71
         max:                                   15.36
         95th percentile:                        3.19
         sum:                                89964.50

Threads fairness:
    events (avg/stddev):           11058.6667/165.71
    execution time (avg/stddev):   29.9882/0.00


### Before PGO
SQL statistics:<br />
    queries performed:<br />
        read:                            13004740<br />
        write:                           0<br />
        other:                           1857820<br />
        total:                           14862560<br />
    transactions:                        928910 (15475.09 per sec.)<br />
    queries:                             14862560 (247601.52 per sec.)<br />
    ignored errors:                      0      (0.00 per sec.)<br />
    reconnects:                          0      (0.00 per sec.)<br />

General statistics:<br />
    total time:                          60.0240s<br />
    total number of events:              928910<br />

Latency (ms):<br />
         min:                                    3.12<br />
         avg:                                    4.13<br />
         max:                                   23.51<br />
         95th percentile:                        4.91<br />
         sum:                              3838483.15<br />

Threads fairness:<br />
    events (avg/stddev):           14514.2188/524.30<br />
    execution time (avg/stddev):   59.9763/0.00<br />
