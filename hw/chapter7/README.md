# Chapter 7: CPU Scheduling

This assignment explores FIFO, SJF, and Round Robin (RR) scheduling.

## Files
- analysis.md: Answers and analysis for Questions 1–7.
- data/: Saved simulator outputs.

## Run the Simulator
Run commands from the repository root.
python3 ./ext/ostep-homework/cpu-sched/scheduler.py -l 100,200,300 -p FIFO -c
python3 ./ext/ostep-homework/cpu-sched/scheduler.py -l 100,200,300 -p SJF -c
python3 ./ext/ostep-homework/cpu-sched/scheduler.py -l 100,200,300 -p RR -q 1 -c
