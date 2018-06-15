# sparkMeasure with sparklens output example

### 1. sparkMeasure output:
```
Scheduling mode = FIFO
Spark Contex default degree of parallelism = 2
Aggregated Spark task metrics:
numtasks => 4
elapsedTime => 928 (0.9 s)
sum(duration) => 557 (0.6 s)
sum(schedulerDelay) => 61
sum(executorRunTime) => 331 (0.3 s)
sum(executorCpuTime) => 245 (0.2 s)
sum(executorDeserializeTime) => 159 (0.2 s)
sum(executorDeserializeCpuTime) => 90 (90 ms)
sum(resultSerializationTime) => 6 (6 ms)
sum(jvmGCTime) => 25 (25 ms)
sum(shuffleFetchWaitTime) => 0 (0 ms)
sum(shuffleWriteTime) => 4 (4 ms)
sum(gettingResultTime) => 0 (0 ms)
max(resultSize) => 2051 (2.0 KB)
sum(numUpdatedBlockStatuses) => 0
sum(diskBytesSpilled) => 0 (0 Bytes)
sum(memoryBytesSpilled) => 0 (0 Bytes)
max(peakExecutionMemory) => 0
sum(recordsRead) => 668
sum(bytesRead) => 52185 (50.0 KB)
sum(recordsWritten) => 0
sum(bytesWritten) => 0 (0 Bytes)
sum(shuffleTotalBytesRead) => 118 (118 Bytes)
sum(shuffleTotalBlocksFetched) => 2
sum(shuffleLocalBlocksFetched) => 2
sum(shuffleRemoteBlocksFetched) => 0
sum(shuffleBytesWritten) => 118 (118 Bytes)
sum(shuffleRecordsWritten) => 2

Scheduling mode = FIFO
Spark Context default degree of parallelism = 2
Aggregated Spark stage metrics:
numStages => 4
sum(numTasks) => 4
elapsedTime => 1031 (1 s)
sum(stageDuration) => 714 (0.7 s)
sum(executorRunTime) => 331 (0.3 s)
sum(executorCpuTime) => 245 (0.2 s)
sum(executorDeserializeTime) => 159 (0.2 s)
sum(executorDeserializeCpuTime) => 90 (90 ms)
sum(resultSerializationTime) => 6 (6 ms)
sum(jvmGCTime) => 25 (25 ms)
sum(shuffleFetchWaitTime) => 0 (0 ms)
sum(shuffleWriteTime) => 4 (4 ms)
max(resultSize) => 2051 (2.0 KB)
sum(numUpdatedBlockStatuses) => 0
sum(diskBytesSpilled) => 0 (0 Bytes)
sum(memoryBytesSpilled) => 0 (0 Bytes)
max(peakExecutionMemory) => 0
sum(recordsRead) => 668
sum(bytesRead) => 52185 (50.0 KB)
sum(recordsWritten) => 0
sum(bytesWritten) => 0 (0 Bytes)
sum(shuffleTotalBytesRead) => 118 (118 Bytes)
sum(shuffleTotalBlocksFetched) => 2
sum(shuffleLocalBlocksFetched) => 2
sum(shuffleRemoteBlocksFetched) => 0
sum(shuffleBytesWritten) => 118 (118 Bytes)
sum(shuffleRecordsWritten) => 2
```

### 2. sparklens output:
```
Total tasks in all stages 4
Per Stage  Utilization
Stage-ID   Wall    Task      Task     IO%    Input     Output    ----Shuffle-----    -WallClockTime-    --OneCoreComputeHours---   MaxTaskMem
          Clock%  Runtime%   Count                               Input  |  Output    Measured | Ideal   Available| Used%|Wasted%                                
       0   90.00   90.65         1   47.5   24.3 KB    0.0 KB    0.0 KB    0.1 KB    00m 00s   00m 00s    00h 00m   32.4   67.6    0.0 KB
       1    6.00    7.53         1    0.0    0.0 KB    0.0 KB    0.1 KB    0.0 KB    00m 00s   00m 00s    00h 00m   40.3   59.7    0.0 KB
       2    2.00    1.30         1   52.0   26.6 KB    0.0 KB    0.0 KB    0.1 KB    00m 00s   00m 00s    00h 00m   20.8   79.2    0.0 KB
       3    1.00    0.52         1    0.0    0.0 KB    0.0 KB    0.1 KB    0.0 KB    00m 00s   00m 00s    00h 00m   14.3   85.7    0.0 KB


 Stage-ID WallClock  OneCore       Task   PRatio    -----Task------   OIRatio  |* ShuffleWrite% ReadFetch%   GC%  *|
          Stage%     ComputeHours  Count            Skew   StageSkew            
      0   90.73         00h 00m       1    0.50     1.00     0.65     0.00     |*   1.31           0.00    10.32  *|
      1    6.07         00h 00m       1    0.50     1.00     0.81     0.00     |*   0.00           0.00     0.00  *|
      2    2.02         00h 00m       1    0.50     1.00     0.42     0.00     |*   6.64           0.00     0.00  *|
      3    1.18         00h 00m       1    0.50     1.00     0.29     0.00     |*   0.00           0.00     0.00  *|

PRatio:        Number of tasks in stage divided by number of cores. Represents degree of
               parallelism in the stage
TaskSkew:      Duration of largest task in stage divided by duration of median task.
               Represents degree of skew in the stage
TaskStageSkew: Duration of largest task in stage divided by total duration of the stage.
               Represents the impact of the largest task on stage time.
OIRatio:       Output to input ration. Total output of the stage (results + shuffle write)
               divided by total input (input data + shuffle read)

These metrics below represent distribution of time within the stage

ShuffleWrite:  Amount of time spent in shuffle writes across all tasks in the given
               stage as a percentage
ReadFetch:     Amount of time spent in shuffle read across all tasks in the given
               stage as a percentage
GC:            Amount of time spent in GC across all tasks in the given stage as a
               percentage

If the stage contributes large percentage to overall application time, we could look into
these metrics to check which part (Shuffle write, read fetch or GC is responsible)




 App completion time and cluster utilization estimates with different executor counts

 Real App Duration 00m 06s
 Model Estimation  00m 06s
 Model Error       6%

 NOTE: 1) Model error could be large when auto-scaling is enabled.
       2) Model doesn't handles multiple jobs run via thread-pool. For better insights into
          application scalability, please try such jobs one by one without thread-pool.


 Executor count     1  (100%) estimated time 00m 06s and estimated cluster utilization 3.00%
 Executor count     1  (110%) estimated time 00m 06s and estimated cluster utilization 3.00%
 Executor count     1  (120%) estimated time 00m 06s and estimated cluster utilization 3.00%
 Executor count     1  (150%) estimated time 00m 06s and estimated cluster utilization 3.00%
 Executor count     2  (200%) estimated time 00m 06s and estimated cluster utilization 1.50%
 Executor count     3  (300%) estimated time 00m 06s and estimated cluster utilization 1.00%
 Executor count     4  (400%) estimated time 00m 06s and estimated cluster utilization 0.75%
 Executor count     5  (500%) estimated time 00m 06s and estimated cluster utilization 0.60%


 Time spent in Driver vs Executors
 Driver WallClock Time    00m 06s   88.29%
 Executor WallClock Time  00m 00s   11.71%
 Total WallClock Time     00m 06s



Minimum possible time for the app based on the critical path (with infinite resources)   00m 06s
Minimum possible time for the app with same executors, perfect parallelism and zero skew 00m 06s
If we were to run this app with single executor and single core                          00h 00m


 Total cores available to the app 2

 OneCoreComputeHours: Measure of total compute power available from cluster. One core in the executor, running
                      for one hour, counts as one OneCoreComputeHour. Executors with 4 cores, will have 4 times
                      the OneCoreComputeHours compared to one with just one core. Similarly, one core executor
                      running for 4 hours will OnCoreComputeHours equal to 4 core executor running for 1 hour.

 Driver Utilization (Cluster idle because of driver)

 Total OneCoreComputeHours available                             00h 00m
 Total OneCoreComputeHours available (AutoScale Aware)           00h 00m
 OneCoreComputeHours wasted by driver                            00h 00m

 AutoScale Aware: Most of the calculations by this tool will assume that all executors are available throughout
                  the runtime of the application. The number above is printed to show possible caution to be
                  taken in interpreting the efficiency metrics.

 Cluster Utilization (Executors idle because of lack of tasks or skew)

 Executor OneCoreComputeHours available                  00h 00m
 Executor OneCoreComputeHours used                       00h 00m        24.03%
 OneCoreComputeHours wasted                              00h 00m        75.97%

 App Level Wastage Metrics (Driver + Executor)

 OneCoreComputeHours wasted Driver               88.29%
 OneCoreComputeHours wasted Executor             8.89%
 OneCoreComputeHours wasted Total                97.19%



Printing application meterics.....

 AggregateMetrics (Application Metrics) total measurements 4
                NAME                        SUM                MIN           MAX                MEAN
 diskBytesSpilled                            0.0 KB         0.0 KB         0.0 KB              0.0 KB
 executorRuntime                           385.0 ms         2.0 ms       349.0 ms             96.0 ms
 inputBytesRead                             51.0 KB         0.0 KB        26.6 KB             12.7 KB
 jvmGCTime                                  36.0 ms         0.0 ms        36.0 ms              9.0 ms
 memoryBytesSpilled                          0.0 KB         0.0 KB         0.0 KB              0.0 KB
 outputBytesWritten                          0.0 KB         0.0 KB         0.0 KB              0.0 KB
 peakExecutionMemory                         0.0 KB         0.0 KB         0.0 KB              0.0 KB
 resultSize                                  7.4 KB         1.7 KB         2.0 KB              1.9 KB
 shuffleReadBytesRead                        0.1 KB         0.0 KB         0.1 KB              0.0 KB
 shuffleReadFetchWaitTime                    0.0 ms         0.0 ms         0.0 ms              0.0 ms
 shuffleReadLocalBlocks                           2              0              1                   0
 shuffleReadRecordsRead                           2              0              1                   0
 shuffleReadRemoteBlocks                          0              0              0                   0
 shuffleWriteBytesWritten                    0.1 KB         0.0 KB         0.1 KB              0.0 KB
 shuffleWriteRecordsWritten                       2              0              1                   0
 shuffleWriteTime                            4.9 ms         0.0 ms         4.6 ms              1.2 ms
 taskDuration                              593.0 ms         7.0 ms       538.0 ms            148.0 ms



Done printing host timeline

Total Hosts 1


Host localhost startTime 04:11:06:914 executors count 1
Done printing host timeline


Printing executors timeline....

Total Hosts 1
Total Executors 1
At 04:11 executors added 1 & removed  0 currently available 1

Done printing executors timeline...



Printing Application timeline

04:11:06:189 app started
04:11:16:277 JOB 0 started : duration 00m 00s
[      0                ||||||||||||||||||||||||||||||||||||||||||||||||||||||||         ]
[      1                                                                            |||| ]
04:11:16:422      Stage 0 started : duration 00m 00s
04:11:16:960      Stage 0 ended : maxTaskTime 349 taskCount 1
04:11:17:000      Stage 1 started : duration 00m 00s
04:11:17:036      Stage 1 ended : maxTaskTime 29 taskCount 1
04:11:17:045 JOB 0 ended
04:11:17:117 JOB 1 started : duration 00m 00s
[      2          ||||||||||||                                                           ]
[      3                          |||||||                                                ]
04:11:17:126      Stage 2 started : duration 00m 00s
04:11:17:138      Stage 2 ended : maxTaskTime 5 taskCount 1
04:11:17:142      Stage 3 started : duration 00m 00s
04:11:17:149      Stage 3 ended : maxTaskTime 2 taskCount 1
04:11:17:150 JOB 1 ended
04:11:22:627 app ended



Checking for job overlap...


No overlapping jobs found. Good




 Time spent in Driver vs Executors
 Driver WallClock Time    00m 15s   95.13%
 Executor WallClock Time  00m 00s   4.87%
 Total WallClock Time     00m 16s



Minimum possible time for the app based on the critical path (with infinite resources)   00m 16s
Minimum possible time for the app with same executors, perfect parallelism and zero skew 00m 15s
If we were to run this app with single executor and single core                          00h 00m


 Total cores available to the app 2

 OneCoreComputeHours: Measure of total compute power available from cluster. One core in the executor, running
                      for one hour, counts as one OneCoreComputeHour. Executors with 4 cores, will have 4 times
                      the OneCoreComputeHours compared to one with just one core. Similarly, one core executor
                      running for 4 hours will OnCoreComputeHours equal to 4 core executor running for 1 hour.

 Driver Utilization (Cluster idle because of driver)

 Total OneCoreComputeHours available                             00h 00m
 Total OneCoreComputeHours available (AutoScale Aware)           00h 00m
 OneCoreComputeHours wasted by driver                            00h 00m

 AutoScale Aware: Most of the calculations by this tool will assume that all executors are available throughout
                  the runtime of the application. The number above is printed to show possible caution to be
                  taken in interpreting the efficiency metrics.

 Cluster Utilization (Executors idle because of lack of tasks or skew)

 Executor OneCoreComputeHours available                  00h 00m
 Executor OneCoreComputeHours used                       00h 00m        24.03%
 OneCoreComputeHours wasted                              00h 00m        75.97%

 App Level Wastage Metrics (Driver + Executor)

 OneCoreComputeHours wasted Driver               95.13%
 OneCoreComputeHours wasted Executor             3.70%
 OneCoreComputeHours wasted Total                98.83%




 App completion time and cluster utilization estimates with different executor counts

 Real App Duration 00m 16s
 Model Estimation  00m 16s
 Model Error       2%

 NOTE: 1) Model error could be large when auto-scaling is enabled.
       2) Model doesn't handles multiple jobs run via thread-pool. For better insights into
          application scalability, please try such jobs one by one without thread-pool.


 Executor count     1  (100%) estimated time 00m 16s and estimated cluster utilization 1.20%
 Executor count     1  (110%) estimated time 00m 16s and estimated cluster utilization 1.20%
 Executor count     1  (120%) estimated time 00m 16s and estimated cluster utilization 1.20%
 Executor count     1  (150%) estimated time 00m 16s and estimated cluster utilization 1.20%
 Executor count     2  (200%) estimated time 00m 16s and estimated cluster utilization 0.60%
 Executor count     3  (300%) estimated time 00m 16s and estimated cluster utilization 0.40%
 Executor count     4  (400%) estimated time 00m 16s and estimated cluster utilization 0.30%
 Executor count     5  (500%) estimated time 00m 16s and estimated cluster utilization 0.24%



Total tasks in all stages 4
Per Stage  Utilization
Stage-ID   Wall    Task      Task     IO%    Input     Output    ----Shuffle-----    -WallClockTime-    --OneCoreComputeHours---   MaxTaskMem
          Clock%  Runtime%   Count                               Input  |  Output    Measured | Ideal   Available| Used%|Wasted%                                
       0   90.00   90.65         1   47.5   24.3 KB    0.0 KB    0.0 KB    0.1 KB    00m 00s   00m 00s    00h 00m   32.4   67.6    0.0 KB
       1    6.00    7.53         1    0.0    0.0 KB    0.0 KB    0.1 KB    0.0 KB    00m 00s   00m 00s    00h 00m   40.3   59.7    0.0 KB
       2    2.00    1.30         1   52.0   26.6 KB    0.0 KB    0.0 KB    0.1 KB    00m 00s   00m 00s    00h 00m   20.8   79.2    0.0 KB
       3    1.00    0.52         1    0.0    0.0 KB    0.0 KB    0.1 KB    0.0 KB    00m 00s   00m 00s    00h 00m   14.3   85.7    0.0 KB


 Stage-ID WallClock  OneCore       Task   PRatio    -----Task------   OIRatio  |* ShuffleWrite% ReadFetch%   GC%  *|
          Stage%     ComputeHours  Count            Skew   StageSkew            
      0   90.73         00h 00m       1    0.50     1.00     0.65     0.00     |*   1.31           0.00    10.32  *|
      1    6.07         00h 00m       1    0.50     1.00     0.81     0.00     |*   0.00           0.00     0.00  *|
      2    2.02         00h 00m       1    0.50     1.00     0.42     0.00     |*   6.64           0.00     0.00  *|
      3    1.18         00h 00m       1    0.50     1.00     0.29     0.00     |*   0.00           0.00     0.00  *|

PRatio:        Number of tasks in stage divided by number of cores. Represents degree of
               parallelism in the stage
TaskSkew:      Duration of largest task in stage divided by duration of median task.
               Represents degree of skew in the stage
TaskStageSkew: Duration of largest task in stage divided by total duration of the stage.
               Represents the impact of the largest task on stage time.
OIRatio:       Output to input ration. Total output of the stage (results + shuffle write)
               divided by total input (input data + shuffle read)

These metrics below represent distribution of time within the stage

ShuffleWrite:  Amount of time spent in shuffle writes across all tasks in the given
               stage as a percentage
ReadFetch:     Amount of time spent in shuffle read across all tasks in the given
               stage as a percentage
GC:            Amount of time spent in GC across all tasks in the given stage as a
               percentage

If the stage contributes large percentage to overall application time, we could look into
these metrics to check which part (Shuffle write, read fetch or GC is responsible)
```

## Built jars:
   - [sparkMeasure](https://github.com/alitet01/test_readme/blob/master/spark-measure_2.11-0.12-SNAPSHOT.jar)
   - [sparklens](https://github.com/alitet01/test_readme/blob/master/sparklens_2.11-0.1.2.jar)

## Run example:
   - sparkMeasure

```
spark-shell --jars spark-measure_2.11-0.12-SNAPSHOT.jar

val taskMetrics = ch.cern.sparkmeasure.TaskMetrics(spark)
val stageMetrics = ch.cern.sparkmeasure.StageMetrics(spark)
taskMetrics.begin()
stageMetrics.begin()

//Your code here

taskMetrics.end()
stageMetrics.end()
taskMetrics.printReport()
stageMetrics.printReport()

```

   - sparklens

```
spark-shell --jars sparklens_2.11-0.1.2.jar --conf spark.extraListeners=com.qubole.sparklens.QuboleJobListener

import com.qubole.sparklens.QuboleNotebookListener
val QNL = new QuboleNotebookListener(sc.getConf)
sc.addSparkListener(QNL)

QNL.profileIt {
    //Your code here
}
```

