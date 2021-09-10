# Oceandb Profiling Optimizaiton
## Guide
* https://johnysswlab.com/tune-your-programs-speed-with-profile-guided-optimizations/

## The file to enable PGO
* /path/to/oceanbase-master/deps/oblib/src/CMakeLists.txt
* /path/to/oceanbase-master/src/CMakeLists.txt
* /path/to/oceanbase-master/cmake/Env.cmake

## Sysbench Read Only
### Before PGO
SQL statistics:
    queries performed:
        read:                            13004740
        write:                           0
        other:                           1857820
        total:                           14862560
    transactions:                        928910 (15475.09 per sec.)
    queries:                             14862560 (247601.52 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          60.0240s
    total number of events:              928910

Latency (ms):
         min:                                    3.12
         avg:                                    4.13
         max:                                   23.51
         95th percentile:                        4.91
         sum:                              3838483.15

Threads fairness:
    events (avg/stddev):           14514.2188/524.30
    execution time (avg/stddev):   59.9763/0.00

