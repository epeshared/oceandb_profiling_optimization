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
