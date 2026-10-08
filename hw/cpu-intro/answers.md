# OSTEP Ch.4 Homework Answers

## Q1
- Prediction / 预测:
CPU 不会空闲。两个进程都只使用 CPU，没有 I/O，因此只要还有进程没有完成，CPU 就会一直工作。

- Reasoning / 理由:
两个进程分别都有 5 条 CPU 指令。一个进程运行时，另一个进程处于 READY 状态，因此 CPU 可以持续执行，不需要等待 I/O。
- Verified result / 验证结果:
  实际执行结果显示，总时间为 10 ticks，CPU Busy 为 10（100.00%），IO Busy 为 0（0.00%）。PID 0 先执行 5 个 CPU 指令，随后 PID 1 执行 5 个 CPU 指令，CPU 全程没有空闲。

- Analysis / 分析:
  实际结果与预测一致。两个进程都只包含 CPU 指令，没有 I/O，因此 CPU 从第 1 个 tick 到第 10 个 tick 始终处于运行状态，CPU 利用率达到 100%。


## Q2
- Prediction / 预测:
CPU 不会一直空闲。PID 0 有 4 条 CPU 指令，PID 1 只有 1 次 I/O。PID 0 结束之前，PID 1 会发起 I/O 并进入 BLOCKED 状态；在 I/O 等待期间，如果 PID 0 仍然可以运行，CPU 可以继续执行 PID 0。

- Reasoning / 理由:
PID 1 的 I/O 会使它进入 BLOCKED 状态，但 PID 0 仍然可以使用 CPU。因此 I/O 等待不会让 CPU 必然空闲。

- Verified result / 验证结果:
  实际执行结果显示，总时间为 11 ticks，CPU Busy 为 6（54.55%），IO Busy 为 5（45.45%）。PID 0 执行 4 个 CPU 指令后完成，PID 1 在第 5 个 tick 发起 I/O，并在第 6～10 个 tick 处于 BLOCKED 状态，第 11 个 tick 执行 io_done。

- Analysis / 分析:
  实际结果与预测基本一致。PID 1 发起 I/O 后需要等待 5 个 tick，在这段时间 CPU 没有其他可运行的进程，因此 CPU 处于空闲状态。I/O 完成后 PID 1 执行 io_done，整个过程共 11 ticks。


## Q3
- Prediction / 预测:
  PID 0 先执行 I/O 并进入 BLOCKED 状态。在 I/O 等待期间，CPU 不会空闲，而是运行 PID 1。I/O 完成后，PID 0 会再次获得 CPU 并执行 `io_done`。

- Reasoning / 理由:
  Q3 与 Q2 只是两个进程的顺序发生了变化。当 PID 0 执行 I/O 后会被阻塞，因此系统可以切换到 PID 1，让 PID 1 使用 CPU。I/O 完成后，PID 0 再继续运行。

- Verified result / 验证结果:
  实际执行结果显示，总时间为 7 ticks，CPU Busy 为 6（85.71%），IO Busy 为 5（71.43%）。PID 0 在第 1 个 tick 发起 I/O，PID 1 在等待期间执行 4 个 CPU 指令，第 7 个 tick PID 0 执行 io_done。

- Analysis / 分析:
  实际结果与预测一致。PID 0 等待 I/O 时，CPU 可以运行 PID 1，因此 I/O 等待不会使 CPU 完全空闲。PID 1 完成后，PID 0 的 I/O 也完成并继续执行。


## Q4
- Prediction / 预测:
  PID 0 先执行 I/O。由于使用 `SWITCH_ON_END`，系统只有在当前进程结束时才进行切换，因此 PID 0 发起 I/O 后，调度行为会与 Q3 不同。CPU 不会按照 `SWITCH_ON_IO` 的方式立即切换到 PID 1。

- Reasoning / 理由:
  Q4 使用的是 `SWITCH_ON_END`，而 Q3 使用默认的 `SWITCH_ON_IO`。两者的区别在于发生 I/O 时是否立即进行进程切换。`SWITCH_ON_END` 只在当前进程完成时切换。

- Verified result / 验证结果:
  实际执行结果显示，总时间为 11 ticks，CPU Busy 为 6（54.55%），IO Busy 为 5（45.45%）。PID 0 在第 1 个 tick 发起 I/O，第 7 个 tick 执行 io_done；PID 1 从第 8 个 tick 开始执行 4 个 CPU 指令。

- Analysis / 分析:
  实际结果与预测一致。使用 SWITCH_ON_END 时，系统不会因为 PID 0 发起 I/O 就立即切换到 PID 1，因此 PID 1 在 PID 0 等待 I/O 时一直处于 READY 状态。与 Q3 的 SWITCH_ON_IO 相比，总时间从 7 ticks 增加到 11 ticks。


## Q5
- Prediction / 预测:
  PID 0 先执行 I/O 并进入 BLOCKED 状态，系统在 I/O 发生时切换到 PID 1。PID 1 执行自己的 4 条 CPU 指令。之后 PID 0 的 I/O 完成，再执行 `io_done`。

- Reasoning / 理由:
  Q5 使用 `SWITCH_ON_IO`，系统会在当前进程发出 I/O 时进行切换。因此 PID 0 等待 I/O 时，CPU 可以运行 PID 1。I/O 完成后，PID 0 会在之后重新获得 CPU。

- Verified result / 验证结果:
  实际执行结果显示，总时间为 7 ticks，CPU Busy 为 6（85.71%），IO Busy 为 5（71.43%）。PID 0 在第 1 个 tick 发起 I/O，PID 1 随后执行 4 个 CPU 指令，第 7 个 tick PID 0 执行 io_done。

- Analysis / 分析:
  实际结果与预测一致。SWITCH_ON_IO 会在 PID 0 发起 I/O 时立即切换到 PID 1，使 CPU 能够在 I/O 等待期间继续执行其他进程。因此总时间比 SWITCH_ON_END 少 4 ticks。


## Q6
- Prediction / 预测:
  PID 0 会先执行 I/O。由于使用 `IO_RUN_LATER`，每次 I/O 完成后，PID 0 不会立即重新运行，而是让其他进程继续使用 CPU。PID 0 会在之后再次轮到自己时执行 `io_done`，然后继续下一次 I/O。整个过程中 PID 1、PID 2 和 PID 3 都会执行各自的 5 条 CPU 指令。

- Reasoning / 理由:
  `SWITCH_ON_IO` 会在 PID 0 发出 I/O 时进行进程切换，而 `IO_RUN_LATER` 表示 I/O 完成后，发起 I/O 的进程不会立即获得 CPU。因此 CPU 可以继续运行其他进程。PID 0 一共进行了 3 次 I/O，每次完成后都要等待之后的调度机会。

- Verified result / 验证结果:
  实际执行结果显示，总时间为 31 ticks，CPU Busy 为 21（67.74%），IO Busy 为 15（48.39%）。PID 0 分别在第 1、18、25 个 tick 发起 I/O，并在第 17、24、31 个 tick 执行对应的 io_done。PID 1、PID 2 和 PID 3 分别完成 5 个 CPU 指令。

- Analysis / 分析:
  实际结果与预测一致。使用 IO_RUN_LATER 时，PID 0 的 I/O 完成后不会立即重新运行，而是等待其他进程执行完成后再继续。因此 PID 1、PID 2 和 PID 3 可以先完成各自的 CPU 工作，之后 PID 0 再继续执行。


## Q7
- Prediction / 预测:
  PID 0 会先执行 I/O。由于使用 `IO_RUN_IMMEDIATE`，每次 I/O 完成后，PID 0 会立即重新获得 CPU 并执行 `io_done`，然后继续下一次 I/O。PID 1、PID 2 和 PID 3 则在 PID 0 的 I/O 处理过程中获得 CPU。

- Reasoning / 理由:
  Q7 与 Q6 的区别是 I/O 完成后的调度方式。Q6 使用 `IO_RUN_LATER`，PID 0 需要等待之后再运行；Q7 使用 `IO_RUN_IMMEDIATE`，所以 I/O 完成后 PID 0 会立即运行。

- Verified result / 验证结果:
  实际执行结果显示，总时间为 21 ticks，CPU Busy 为 21（100.00%），IO Busy 为 15（71.43%）。PID 0 分别在第 1、8、15 个 tick 发起 I/O，并在第 7、14、21 个 tick 执行对应的 io_done。PID 1、PID 2 和 PID 3 分别完成 5 个 CPU 指令。

- Analysis / 分析:
  实际结果与预测一致。使用 IO_RUN_IMMEDIATE 时，PID 0 每次 I/O 完成后都会立即继续运行，因此 I/O 密集型进程能够快速完成自己的工作。相比 Q6 的 IO_RUN_LATER，总时间从 31 ticks 降低到 21 ticks，CPU 利用率也达到 100%。


## Q8
- Prediction / 预测:
  使用 `-s 1`、`-s 2` 和 `-s 3` 分别生成不同的随机指令序列，并比较默认设置、`IO_RUN_IMMEDIATE` 和 `SWITCH_ON_END` 三种情况下的执行行为。预测不同的 seed 会产生不同的 CPU/I/O 指令组合，而调度参数会影响 I/O 发生后的进程切换方式。

- Reasoning / 理由:
  seed 决定两个进程随机生成的指令序列，因此不同 seed 会产生不同的 CPU 和 I/O 分布。`SWITCH_ON_IO` 会在进程发出 I/O 时切换；`IO_RUN_LATER` 会让 I/O 发起进程之后再运行，而 `IO_RUN_IMMEDIATE` 会让它在 I/O 完成后立即运行。`SWITCH_ON_END` 则只有当前进程结束时才进行切换。

- Verified result / 验证结果:
  Seed 1:
  - Default: Total Time 15，CPU Busy 53.33%，IO Busy 66.67%
  - IO_RUN_IMMEDIATE: Total Time 15，CPU Busy 53.33%，IO Busy 66.67%
  - SWITCH_ON_END: Total Time 18，CPU Busy 44.44%，IO Busy 55.56%

  Seed 2:
  - Default: Total Time 16，CPU Busy 62.50%，IO Busy 87.50%
  - IO_RUN_IMMEDIATE: Total Time 16，CPU Busy 62.50%，IO Busy 87.50%
  - SWITCH_ON_END: Total Time 30，CPU Busy 33.33%，IO Busy 66.67%

  Seed 3:
  - Default: Total Time 18，CPU Busy 50.00%，IO Busy 61.11%
  - IO_RUN_IMMEDIATE: Total Time 17，CPU Busy 52.94%，IO Busy 64.71%
  - SWITCH_ON_END: Total Time 24，CPU Busy 37.50%，IO Busy 62.50%

- Analysis / 分析:
  实际结果表明，不同的随机 seed 会产生不同的 CPU/I/O 指令组合，因此总执行时间和 CPU、I/O 利用率也会不同。Seed 1 和 Seed 2 中，Default 与 IO_RUN_IMMEDIATE 的结果相同；Seed 3 中，IO_RUN_IMMEDIATE 的总时间比 Default 少 1 tick。相比之下，SWITCH_ON_END 在三个 seed 中的总时间都更长，分别为 18、30 和 24 ticks。这说明调度策略会明显影响进程执行时间和 CPU 利用率。