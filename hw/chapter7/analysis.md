# Chapter 7: CPU Scheduling — Analysis

## Q1. Three jobs of length 200: FIFO and SJF

All three jobs have the same length, so FIFO and SJF execute them in the same order.

- Average response time: 200.00
- Average turnaround time: 400.00
- Average wait time: 200.00

## Q2. Jobs of lengths 100, 200, and 300: FIFO and SJF

The jobs are already ordered from shortest to longest, so FIFO and SJF produce the same execution order.

- Average response time: 133.33
- Average turnaround time: 333.33
- Average wait time: 133.33

## Q3. Jobs of lengths 100, 200, and 300: RR with quantum 1

With a time quantum of 1, each job gets an early opportunity to run.

- Average response time: 1.00
- Average turnaround time: 465.67
- Average wait time: 265.67

## Q4. When does SJF have the same turnaround times as FIFO?

SJF and FIFO have the same turnaround times when FIFO already executes jobs in shortest-job-first order. This includes workloads with equal-length jobs or jobs listed from shortest to longest.

## Q5. When can SJF and RR have the same response times?

One sufficient condition is that jobs are initially ordered from shortest to longest and the RR quantum is at least as long as the longest job. Each job then finishes in its first time slice, so RR follows the same execution order as SJF. Other workloads may also produce matching response times.

## Q6. How does response time change as job lengths increase under SJF?

The simulator was tested with three jobs whose lengths were scaled proportionally.

| Job lengths | Average response time | Average turnaround time |
|---|---:|---:|
| 10, 20, 30 | 13.33 | 33.33 |
| 20, 40, 60 | 26.67 | 66.67 |
| 30, 60, 90 | 40.00 | 100.00 |

In these experiments, doubling or tripling all job lengths also doubles or triples the average response time. This demonstrates the trend for the tested workloads.

## Q7. How does response time change as the RR quantum increases?

For jobs of lengths 100, 200, and 300, the simulator produced these results:

| RR quantum | Average response time | Average turnaround time |
|---:|---:|---:|
| 1 | 1.00 | 465.67 |
| 10 | 10.00 | 456.67 |
| 100 | 100.00 | 366.67 |
| 300 | 133.33 | 333.33 |

In these experiments, increasing the quantum increases the average response time. When the quantum is at least as long as every job, each job can finish in one slice, so RR behaves like FIFO for this workload.

For N jobs that are all ready at time zero, the worst-case response time before a job first gets the CPU is (N - 1)q, assuming each of the N - 1 jobs ahead of it uses a full quantum q and context-switch overhead is ignored.
