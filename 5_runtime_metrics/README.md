# 5 Runtime Metrics

## Review Questions (Theory)

1. What kind of values are reported for Erlang's memory?
- You can get an overview of `processes`, `processes_used`, memory used by ets tables, atoms, refc binaries(`system`)
2. What's a valuable process-related metric for a global view?
- Number of processes on a node. It can be used as a metric for load. Tracking this value over time can characterize load or detect process leaks 
3. What's a port, and how should it be monitored globally?
- Ports are a datatype that encompasses all kinds of connections and sockets opened to the outside world: TCP sockets, UDP sockets, SCTP sockets, file descriptors and so on.
4. Why can't you trust `top` or `htop` for CPU usage with Erlang systems? What's the alternative?
5. Name two types of signal-related information available for processes
- You can get `links` which shows a list of all links a process has towards other processes as well as `monitored_by` which gives a list of processes that are monitoring the current process.
6. How can you find what code a specific process is running?
 Check the `current_function` field
7. What are the different kinds of memory information available for a specific process?
- `binary`, `garbage_collection`, `heap_size`, `memory`, `message_queue_length`, `messages`, `total_heap_size`
8. How can you know if a process is doing a lot of work?
- Check the number of reductions it is performing
9. Name a few of the values that are dangerous to fetch when inspecting processes in a production system.
- Don't fetch `messages` as the mailbox can hold up to millions of messages. Don't fetch `binary` either as it can be unsafe if a process has a lot of them allocated
10. What are some features provided to OTP processes through the sys module?
- It allows you to log all messages and state transition, gather statistics, fetch the status of a process, fetch the state of a process, replace the state, and add custom debugging functions to be used as callbacks
11. What kind of values are available when inspecting inet ports?
- You can get inet specific data like statistics, local adress, port number and inet options used.
12. How can you find the type of a port (Files, TCP, UDP)?
- Call `erlang:port_info(Port, Key)`. Then look under the name section of Meta.
## Open-ended Questions
1. Why do you want a long time window available on global metrics?
- Some problems show up as a very long accumulation over weeks and wouldn't be detected over small time windows.
2. Which would be more appropriate between `function recon:proc_count/2` and `function{recon:proc_window/3} to find issues with: `Reductions`,  `Memory`,  `Message queue length`
- It seems like reductions and message_queue_length are short lived statistics so you'd want to use `proc_window`
- Memory would be longer lived so `proc_count`
3. How can you find information about who is the supervisor of a given process?
- Check the `monitored_by` field
4. When should you use `function{recon:inet_count/2}`? `function{recon:inet_window/3}`?
- You would probably want to use `inet_window` when you are tracking a metric within a time window that is short. You can use `inet_count` to check metrics which are duration agnostic
5. What could explain the difference in memory reported by the operating system and the memory functions in Erlang?
- The values returned by erlang memory functions are reported in bytes and they represent memory allocated(memory used by VM and not memory set aside by OS for VM). As such, it will eventually look smaller than what the the operating system reports.
6. Why is it that Erlang can sometimes look very busy even when it isn't?
-  VM does a lot of work unrelated to processes when it comes to scheduling and might do busy looping to keep threads awake when there is low work.
7. How can you find what proportion of processes on a node are ready to run, but can't be scheduled right away?
- `etop` and check `runq`. (Not sure)
## Hands-on
 1. What's the system memory?
 2. Is the node using a lot of CPU resources?
 3. Is any process mailbox overflowing?
 4. Which chatty process `council_member` takes the most memory?
 5. Which chatty process is eating the most CPU?
 6. Which chatty process is consuming the most bandwidth?
 7. Which chatty process sends the most messages over TCP? The least?
 8. Can you find out if a specific process tends to hold multiple connections or file descriptors open at the same time on a node?
 9. Can you find out which function is being called by the most processes at once on the node right now?