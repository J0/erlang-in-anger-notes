# 8 CPU Scheduler Hogs

## Review Questions (Theory)
1. What are the two main approaches to pin issues about CPU usages?
 
- You can use Profiling and reduction counts or monitoring the schedulers work

2. Name some of the profiling tools available. What approaches are preferable for production use? Why?

- ou can use `eprof`, `fprof`, `eflame`. It seems like `eflame` is a good choice because it allows one to quickly find issues with a single look at the final result.

3. Why can long scheduling monitors be useful to find CPU or scheduler over-consumption?
- Work done by NIFs, garbage collection, and so on don't always increment reduction count correctly so they don't always show up through profiling. `long_schedule` will likely catch issues with busy processes.


## Open-ended Questions(Practical)
1. If you find that a process doing very little work with reductions ends up being scheduled for long periods of time, what can you guess about it or the code it runs?
@TODO
Probably suggests that it is a function call which runs for a long time. Not too sure [this is a suggested reading](https://hamidreza-s.github.io/erlang/scheduling/real-time/preemptive/migration/2016/02/09/erlang-scheduler-details.html) will come back and answer this together with 2.

2. Can you set up a system monitor and then trigger it with regular Erlang code? Can you use it to find out for how long processes seem to be scheduled on average.
 @TODO
