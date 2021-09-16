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
### After PGO
#### --table_size=1000 --tables=3 --threads=3 --report-interval=3 --time=30 run
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
