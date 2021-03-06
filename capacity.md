# 性能数据

TiDB 可以通过水平扩容的方式提升性能（限制内存实例除外），以下为TiDB在默认配置（3PD*3TiDB*3TiKV）下的性能表现。

## 测试一

- 版本: v4.0.8
- 线程: 512
- 表: 32 * 1000万条数据
- 事务: 1000万
- 测试时间: 1小时
- Sysbench: v1.0.13

## 同可用区实例

| 操作                                   | Delete                       | Insert                       | Oltp                          | Select                       | update index                 |
| -------------------------------------- | ---------------------------- | ---------------------------- | ----------------------------- | ---------------------------- | ---------------------------- |
| read                                   | 0                            | 0                            | 140000000                     | 10000000                     | 0                            |
| write                                  | 2893289                      | 10000000                     | 36733400                      | 0                            | 9819708                      |
| other                                  | 7106711                      | 0                            | 23266600                      | 0                            | 180292                       |
| total                                  | 10000000                     | 10000000                     | 200000000                     | 10000000                     | 10000000                     |
| transactions                           | 10000000 (72353.61 per sec.) | 10000000 (34896.08 per sec.) | 10000000 (3296.22 per sec.)   | 10000000 (145813.92 per sec.)| 10000000 (24928.44 per sec.) |
| queries                                | 10000000 (72353.61 per sec.) | 10000000 (34896.08 per sec.) | 200000000 (65924.32 per sec.) | 10000000 (145813.92 per sec.)| 10000000 (24928.44 per sec.) |
| ignored errors                         | 0                            | 0                            | 0                             | 0                            | 0                            |
| reconnects                             | 0                            | 0                            | 0                             | 0                            | 0                            |
| total time                             | 138.2084s                    | 286.5630s                    | 3033.7809s                    | 68.5796s                     | 401.1473s                    |
| total number of events                 | 10000000                     | 10000000                     | 10000000                      | 10000000                     | 10000000                     |
| 延时latency   min(ms)                  | 1.15                         | 2.11                         | 32.86                         | 0.91                         | 1.16                         |
| 延时latency   avg(ms)                  | 7.07                         | 14.67                        | 155.32                        | 3.51                         | 20.53                        |
| 延时latency   max(ms)                  | 3536.90                      | 1464.20                      | 1977.64                       | 359.59                       | 3173.37                      |
| 延时latency   95th percentile(ms)      | 22.28                        | 29.19                        | 200.47                        | 8.74                         | 38.25                        |
| 延时latency  sum(ms)                   | 70703222.04                  | 146665415.95                 | 1553214351.69                 | 35054605.48                  | 205329856.41                 |
| 线程公平性(events (avg/stddev))        | 19531.2500/486.76            | 19531.2500/1208.70           | 19531.2500/1484.87            | 19531.2500/3230.58           | 19531.2500/578.39            |
| 线程公平性 execution time (avg/stddev) | 138.0922/0.01                | 286.4559/0.01                | 3033.6218/0.03                | 68.4660/0.01                 | 418.8353/0.03                |


## 同可用区类型- 限制TiKV内存60G

| 操作                                   | Delete                       | Insert                       | Oltp                          | Select                       | update index                 |
| -------------------------------------- | ---------------------------- | ---------------------------- | ----------------------------- | ---------------------------- | ---------------------------- |
| read                                   | 0                            | 0                            | 140000000                     | 10000000                     | 0                            |
| write                                  | 3048784                      | 10000000                     | 39480299                      | 0                            | 9934762                      |
| other                                  | 6951216                      | 0                            | 20519701                      | 0                            | 65238                         |
| total                                  | 10000000                     | 10000000                     | 200000000                     | 10000000                     | 10000000                     |
| transactions                           | 10000000 (62398.11 per sec.) | 10000000 (28291.07 per sec.) | 10000000 (2969.87 per sec.)   | 10000000 (143977.26 per sec.)| 10000000 (23345.29 per sec.) |
| queries                                | 10000000 (62398.11 per sec.) | 10000000 (28291.07 per sec.) | 200000000 (59397.39 per sec.) | 10000000 (143977.26 per sec.)| 10000000 (23345.29 per sec.) |
| ignored errors                         | 0                            | 0                            | 0                             | 0                            | 0                            |
| reconnects                             | 0                            | 0                            | 0                             | 0                            | 0                            |
| total time                             | 160.2602s                    | 353.4668s                    | 3367.1501s                    | 69.4539s                     | 428.3505s                    |
| total number of events                 | 10000000                     | 10000000                     | 10000000                      | 10000000                     | 10000000                     |
| 延时latency   min(ms)                  | 1.10                         | 2.23                         | 39.18                         | 0.95                         | 1.18                         |
| 延时latency   avg(ms)                  | 8.20                         | 18.09                        | 172.39                        | 3.55                         | 21.93                        |
| 延时latency   max(ms)                  | 3103.19                      | 1124.73                      | 1276.10                       | 79.71                       | 3179.78                      |
| 延时latency   95th percentile(ms)      | 26.20                        | 40.37                        | 223.34                        | 8.74                         | 41.85                        |
| 延时latency  sum(ms)                   | 81995831.74                  | 180925890.11                 | 1723914093.22                 | 35507624.44                  | 219259487.79                 |
| 线程公平性(events (avg/stddev))        | 19531.2500/1498.17           | 19531.2500/979.08            | 19531.2500/1914.18             | 19531.2500/1326.04           | 19531.2500/335.69            |
| 线程公平性 execution time (avg/stddev) | 160.1481/0.01                | 353.3709/0.01                | 3367.0197/0.04                | 69.3508/0.01                 | 428.2412/0.01                |


## 同可用区类型- 限制TiKV内存30G

| 操作                                   | Delete                       | Insert                       | Oltp                          | Select                       | update index                 |
| -------------------------------------- | ---------------------------- | ---------------------------- | ----------------------------- | ---------------------------- | ---------------------------- |
| read                                   | 0                            | 0                            | 140000000                     | 10000000                     | 0                            |
| write                                  | 3052004                      | 10000000                     | 37184380                      | 0                            | 9948851                      |
| other                                  | 6947996                      | 0                            | 22815620                      | 0                            | 51149                        |
| total                                  | 10000000                     | 10000000                     | 200000000                     | 10000000                     | 10000000                     |
| transactions                           | 10000000 (62787.77 per sec.) | 10000000 (29295.01 per sec.) | 10000000 (2943.32 per sec.)   | 10000000 (118703.33 per sec.)| 10000000 (21720.84 per sec.) |
| queries                                | 10000000 (62787.77 per sec.) | 10000000 (29295.01 per sec.) | 200000000 (58866.45 per sec.) | 10000000 (118703.33 per sec.)| 10000000 (21720.84 per sec.) |
| ignored errors                         | 0                            | 0                            | 0                             | 0                            | 0                            |
| reconnects                             | 0                            | 0                            | 0                             | 0                            | 0                            |
| total time                             | 159.2659s                    | 341.3541s                    | 3397.5203s                    | 84.2427s                     | 460.3864s                    |
| total number of events                 | 10000000                     | 10000000                     | 10000000                      | 10000000                     | 10000000                     |
| 延时latency   min(ms)                  | 1.17                         | 2.20                         | 36.68                         | 0.97                         | 1.21                         |
| 延时latency   avg(ms)                  | 8.14                         | 17.47                        | 173.94                        | 4.30                         | 23.40                        |
| 延时latency   max(ms)                  | 3070.66                      | 1307.86                      | 517.10                        | 324.67                       | 3583.93                      |
| 延时latency   95th percentile(ms)      | 23.95                        | 34.33                        | 235.74                        | 11.45                         | 45.79                        |
| 延时latency  sum(ms)                   | 81449901.77                  | 174694924.42                 | 1739438308.91                 | 43044129.07                  | 233956807.95                 |
| 线程公平性(events (avg/stddev))        | 19531.2500/1326.14           | 19531.2500/549.20            | 19531.2500/2489.36            | 19531.2500/1659.90           | 19531.2500/824.38            |
| 线程公平性 execution time (avg/stddev) | 159.0818/0.01                | 341.2010/0.01                | 3397.3404/0.05                | 84.0706/0.01                 | 456.9469/0.20                |

## 测试二

- 版本: v3.0.5
- 线程: 512
- 表: 32 * 6000万条数据
- 数据量约1.3T
- 事务: 1000万
- 测试时间: 600(s)
- sysbench: v1.0.13

## 跨可用区类型

| 操作                                   | Delete                       | Insert                      | Oltp                         | Select                       | update index                 |
| -------------------------------------- | ---------------------------- | --------------------------- | ---------------------------- | ---------------------------- | ---------------------------- |
| read                                   | 0                            | 0                           | 29160418                     | 10000000                     | 0                            |
| write                                  | 5399058                      | 8766893                     | 5176956                      | 0                            | 6271289                      |
| other                                  | 4600942                      | 0                           | 7320366                      | 0                            | 3728711                      |
| total                                  | 10000000                     | 8766893                     | 41657740                     | 10000000                     | 10000000                     |
| transactions                           | 10000000 (22371.77 per sec.) | 8766893 (14610.32 per sec.) | 2082887 (3470.58 per sec.)   | 10000000 (63480.92 per sec.) | 10000000 (18292.54 per sec.) |
| queries                                | 10000000 (22371.77 per sec.) | 8766893 (14610.32 per sec.) | 41657740 (69411.50 per sec.) | 10000000 (63480.92 per sec.) | 10000000 (18292.54 per sec.) |
| ignored errors                         | 0      (0.00 per sec.)       | 0      (0.00 per sec.)      | 0      (0.00 per sec.)       | 0      (0.00 per sec.)       | 0      (0.00 per sec.)       |
| reconnects                             | 0      (0.00 per sec.)       | 0      (0.00 per sec.)      | 0      (0.00 per sec.)       | 0      (0.00 per sec.)       | 0      (0.00 per sec.)       |
| total time                             | 446.9905s                    | 600.0467s                   | 600.1546s                    | 157.5262s                    | 546.6693s                    |
| total number of events                 | 10000000                     | 8766893                     | 2082887                      | 10000000                     | 10000000                     |
| 延时latency   min(ms)                  | 1.91                         | 7.49                        | 54.98                        | 0.61                         | 1.92                         |
| 延时latency   avg(ms)                  | 22.88                        | 35.04                       | 147.50                       | 8.06                         | 27.99                        |
| 延时latency   max(ms)                  | 5332.54                      | 285.32                      | 4198.65                      | 239.99                       | 9851.13                      |
| 延时latency   95th percentile(ms)      | 54.83                        | 56.84                       | 186.54                       | 21.11                        | 62.19                        |
| 延时latency  sum(ms)                   | 228819822.49                 | 307187149.43                | 307227921.15                 | 80609016.31                  | 279850391.93                 |
| 线程公平性(events (avg/stddev))        | 19531.2500/746.24            | 17122.8379/512.84           | 4068.1387/278.93             | 80609016.31                  | 19531.2500/610.81            |
| 线程公平性 execution time (avg/stddev) | 446.9137/0.01                | 599.9749/0.01               | 600.0545/0.04                | 157.4395/0.02                | 546.5828/0.01                |

## 同可用区类型

| 操作                                   | Delete                       | Insert                       | Oltp                         | Select                       | update index                 |
| -------------------------------------- | ---------------------------- | ---------------------------- | ---------------------------- | ---------------------------- | ---------------------------- |
| read                                   | 0                            | 0                            | 35104384                     | 10000000                     | 0                            |
| write                                  | 4967836                      | 10000000                     | 6039939                      | 0                            | 5715841                      |
| other                                  | 5032164                      | 0                            | 9004797                      | 0                            | 4284159                      |
| total                                  | 10000000                     | 10000000                     | 50149120                     | 10000000                     | 10000000                     |
| transactions                           | 10000000 (31633.96 per sec.) | 10000000 (19810.12 per sec.) | 2507456 (4178.42 per sec.)   | 10000000 (72581.93 per sec.) | 10000000 (24383.18 per sec.) |
| queries                                | 10000000 (31633.96 per sec.) | 10000000 (19810.12 per sec.) | 50149120 (83568.32 per sec.) | 10000000 (72581.93 per sec.) | 10000000 (24383.18 per sec.) |
| ignored errors                         | 0      (0.00 per sec.)       | 0      (0.00 per sec.)       | 0      (0.00 per sec.)       | 0      (0.00 per sec.)       | 0      (0.00 per sec.)       |
| reconnects                             | 0      (0.00 per sec.)       | 0      (0.00 per sec.)       | 0      (0.00 per sec.)       | 0      (0.00 per sec.)       | 0      (0.00 per sec.)       |
| total time                             | 316.1144s                    | 504.7909s                    | 600.0958s                    | 137.7739s                    | 410.1173s                    |
| total number of events                 | 10000000                     | 10000000                     | 2507456                      | 10000000                     | 10000000                     |
| 延时latency   min(ms)                  | 0.54                         | 1.89                         | 16.70                        | 0.38                         | 0.53                         |
| 延时latency   avg(ms)                  | 16.18                        | 25.84                        | 122.52                       | 7.05                         | 20.99                        |
| 延时latency   max(ms)                  | 7359.57                      | 1119.64                      | 687.53                       | 691.31                       | 4990.75                      |
| 延时latency   95th percentile(ms)      | 51.94                        | 47.47                        | 193.38                       | 18.61                        | 62.19                        |
| 延时latency  sum(ms)                   | 161821250.26                 | 258417879.28                 | 307218046.84                 | 70500537.78                  | 209946556.81                 |
| 线程公平性(events (avg/stddev))        | 19531.2500/182.09            | 19531.2500/151.34            | 4897.3750/116.01             | 19531.2500/183.19            | 19531.2500/178.73            |
| 线程公平性 execution time (avg/stddev) | 316.0571/0.01                | 504.7224/0.01                | 600.0352/0.03                | 137.6964/0.02                | 410.0519/0.01                |
