[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: LI Jiaxu
### Student Id: 21073153
### Email: jlila@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > Tool: The measurement tool I used is sysbench.
    >
    > CPU Test Configuration:
    > Command: sysbench cpu --cpu-max-prime=20000 run
    > Description: This command tests the CPU performance by calculating prime numbers up to 20000. It will give you the total time taken to compute the primes and the rate of computation (prime numbers per second).
    > Why this configuration: The --cpu-max-prime parameter is set to 20000 to stress the CPU sufficiently without overwhelming it. A higher number will stress the CPU more, but 20000 is a balanced choice for general performance measurement.
    > Memory Test Configuration:
    > Command: sysbench memory --memory-block-size=1M --memory-total-size=10G run
    > Description: This command tests the memory performance by reading and writing 10 GB of data using a block size of 1 MB. It measures the memory transfer speed and latency.
    > Why this configuration: The block size is set to 1 MB to balance between memory read/write speed and testing overhead. The total memory size is set to 10 GB to test the system's ability to handle larger memory operations.
    >
    > Measurement Results Explanation:
    > CPU Performance: The result from sysbench cpu will report the time taken to compute prime numbers and the rate at which the calculations were completed. A lower time means better CPU performance, and a higher rate indicates better CPU efficiency.
    > Memory Performance: The result from sysbench memory will show the transfer speed and the time taken for the memory operation. Higher speed values indicate better memory throughput.

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` | 883.37 events per second | 19021.64 MiB/sec |
    | `t2.medium`  | 885.50 events per second | 19051.94 MiB/sec |
    | `c5d.large` | 447.18 events per second | 19615.86 MiB/sec |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.
    > Yes, in general, the performance of EC2 instances does increase commensurate with the increase in the number of vCPUs and memory resources. However, other factors such as the instance's architecture, hypervisor overhead, and the nature of the benchmark test can also have an impact on the results. For example, t2 instances are burstable, so their performance can vary depending on the CPU credit balance.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` | 4000 Mbits/sec | 0.353 ms |
    | `m5.large` - `m5.large`   | 4270 Mbits/sec | 0.265 ms |
    | `c5n.large` - `c5n.large` | 8830 Mbits/sec | 0.108 ms |
    | `t3.medium` - `c5n.large` | 2460 Mbits/sec | 0.632 ms |
    | `m5.large` - `c5n.large`  | 2900 Mbits/sec | 0.592 ms |
    | `m5.large` - `t3.medium`  | 4170 Mbits/sec | 0.280 ms |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.
    > Same-Type Instances:
    > Instances of the same type show higher and more stable TCP bandwidth and lower RTT. This suggests that same-type instances can achieve better network performance when communicating with each other, as they are optimized to work within similar configurations and resources. Among them, c5n.large instances show the highest TCP bandwidth, followed by m5.large and t3.medium. And c5n.large instances show the lowest RTT, followed by m5.large and t3.medium.
    > Different-Type Instances:
    > Communication between instances of different types results in lower TCP bandwidth and higher RTT due to differences in instance types and their corresponding networking optimizations.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      | 35.0 Mbits/sec | 60.677 ms |
    | N. Virginia - N. Virginia | 4960 Mbits/sec | 0.204 ms |
    | Oregon - Oregon           | 4920 Mbits/sec | 0.156 ms |
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
    > Cross-region bandwidth is much lower due to inter-region latency and network constraints. RTT between different regions is much higher due to the physical distance between data centers.
