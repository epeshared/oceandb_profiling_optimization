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
