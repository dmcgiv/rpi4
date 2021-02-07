# Sysbench


## Install

Would not install via sudo 

```
sudo apt install sysbench
```
```
sysbench : Depends: libmariadbclient18 (>= 5.5.36) but it is not installable
```

Downloaded from debian [libmariadbclient18](https://packages.debian.org/stretch/arm64/libmariadbclient18)

```
sudo apt install /home/pi/Downloads/libmariadbclient18_10.1.48-0+deb9u1_arm64.deb
```


## Setup

```
uname -a
```
```
Linux raspberrypi 5.10.11-v8+ #1399 SMP PREEMPT Thu Jan 28 12:14:03 GMT 2021 aarch64 GNU/Linux
```

## Test CPU


```
sysbench --test=cpu run
```
```
sysbench 0.4.12:  multi-threaded system evaluation benchmark

Running the test with following options:
Number of threads: 1

Doing CPU performance benchmark

Threads started!
Done.

Maximum prime number checked in CPU test: 10000


Test execution summary:
    total time:                          6.6806s
    total number of events:              10000
    total time taken by event execution: 6.6778
    per-request statistics:
         min:                                  0.66ms
         avg:                                  0.67ms
         max:                                  1.77ms
         approx.  95 percentile:               0.67ms

Threads fairness:
    events (avg/stddev):           10000.0000/0.00
    execution time (avg/stddev):   6.6778/0.00
```





## Test File IO


Installed on Crucial BX500 120 GB SSD via USB3 Port

```
lsusb
```
```
Bus 002 Device 002: ID 174c:55aa ASMedia Technology Inc. Name: ASM1051E SATA 6Gb/s bridge, ASM1053E SATA 6Gb/s bridge, ASM1153 SATA 3Gb/s bridge, ASM1153E SATA 6Gb/s bridge
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 003: ID 046d:c52b Logitech, Inc. Unifying Receiver
Bus 001 Device 002: ID 2109:3431 VIA Labs, Inc. Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```


### Setup

```
sysbench --test=fileio --file-total-size=1G prepare
```

```
sysbench 0.4.12:  multi-threaded system evaluation benchmark

128 files, 8192Kb each, 1024Mb total
Creating files for the test...
```


### Run

```
sysbench --test=fileio --file-total-size=1G --file-test-mode=rndrw --init-rng=on --max-time=300 --max-requests=0 run
``` 

```
sysbench 0.4.12:  multi-threaded system evaluation benchmark

Running the test with following options:
Number of threads: 1
Initializing random number generator from timer.


Extra file open flags: 0
128 files, 8Mb each
1Gb total file size
Block size 16Kb
Number of random requests for random IO: 0
Read/Write ratio for combined random IO test: 1.50
Periodic FSYNC enabled, calling fsync() each 100 requests.
Calling fsync() at the end of test, Enabled.
Using synchronous I/O mode
Doing random r/w test
Threads started!
Time limit exceeded, exiting...
Done.

Operations performed:  479820 Read, 319880 Write, 1023496 Other = 1823196 Total
Read 7.3215Gb  Written 4.881Gb  Total transferred 12.202Gb  (41.65Mb/sec)
 2665.62 Requests/sec executed

Test execution summary:
    total time:                          300.0050s
    total number of events:              799700
    total time taken by event execution: 22.2271
    per-request statistics:
         min:                                  0.01ms
         avg:                                  0.03ms
         max:                                  4.57ms
         approx.  95 percentile:               0.06ms

Threads fairness:
    events (avg/stddev):           799700.0000/0.00
    execution time (avg/stddev):   22.2271/0.00
```    


### Cleanup

```
sysbench --test=fileio --file-total-size=1G cleanup
```
```
sysbench 0.4.12:  multi-threaded system evaluation benchmark

Removing test files...
```


## Test Memory

4GB Model

```
sysbench --test=memory --memory-block-size=1K --memory-scope=global --memory-total-size=10G --memory-oper=write run
```

```
sysbench 0.4.12:  multi-threaded system evaluation benchmark

Running the test with following options:
Number of threads: 1

Doing memory operations speed test
Memory block size: 1K

Memory transfer size: 10240M

Memory operations type: write
Memory scope type: global
Threads started!
Done.

Operations performed: 10485760 (1022031.23 ops/sec)

10240.00 MB transferred (998.08 MB/sec)


Test execution summary:
    total time:                          10.2597s
    total number of events:              10485760
    total time taken by event execution: 8.3815
    per-request statistics:
         min:                                  0.00ms
         avg:                                  0.00ms
         max:                                  4.13ms
         approx.  95 percentile:               0.00ms

Threads fairness:
    events (avg/stddev):           10485760.0000/0.00
    execution time (avg/stddev):   8.3815/0.00
```


## Test Threads

RPi4 has 4 Cores

```
sysbench --test=threads --num-threads=2 --thread-yields=8 --max-requests=1000000 --thread-locks=1 run
```
```
sysbench 0.4.12:  multi-threaded system evaluation benchmark

Running the test with following options:
Number of threads: 2

Doing thread subsystem performance test
Thread yields per test: 8 Locks used: 1
Threads started!
Done.


Test execution summary:
    total time:                          38.0866s
    total number of events:              1000000
    total time taken by event execution: 75.9732
    per-request statistics:
         min:                                  0.03ms
         avg:                                  0.08ms
         max:                                 25.69ms
         approx.  95 percentile:               0.26ms

Threads fairness:
    events (avg/stddev):           500000.0000/72798.00
    execution time (avg/stddev):   37.9866/0.01
```




```
sysbench --test=threads --num-threads=8 --thread-yields=8 --max-requests=1000000 --thread-locks=1 run
```
```
sysbench 0.4.12:  multi-threaded system evaluation benchmark

Running the test with following options:
Number of threads: 8

Doing thread subsystem performance test
Thread yields per test: 8 Locks used: 1
Threads started!
Done.


Test execution summary:
    total time:                          52.0210s
    total number of events:              1000000
    total time taken by event execution: 415.9277
    per-request statistics:
         min:                                  0.03ms
         avg:                                  0.42ms
         max:                                 24.76ms
         approx.  95 percentile:               1.34ms

Threads fairness:
    events (avg/stddev):           125000.0000/1530.86
    execution time (avg/stddev):   51.9910/0.00
```    
    

