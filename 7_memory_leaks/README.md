# 7 Memory Leaks

## Review Questions(Theory) 

1. Name some of the common sources of leaks in Erlang programs.
- Primarily data, either from `atoms`, `binary`, `code`, `ets` or `processes`
- In particular, it is worth noting that ETS tables are never garbage collected and maintain their memory usage as long as records will be left undeleted in a table.

2. What are the two main types of binaries in Erlang?
- `ProcBins` and `Refc` binaries
  
3. What could be to blame if no specific data type seems to be the source of a leak?
- binary leaks and memory fragmentation

4. If you find the node died with a process having a lot of memory, what could you do to find out which one it was?
- You can get a rough idea by doing `recon:proc_count(memory, 3)`

5. How could code itself cause a leak?
- Code on an Erlang node is loaded in memory in its own area, and sits there until it is garbage collected. Unfortunately, particular types of code like HiPE code can't be garbage collected from the VM when new versions are loaded. As such, memory can accumulate very slowly and cause a significant leak if many or large modules are native-compiled and loaded at run time.

6. How can you find out if garbage collections are taking too long to run?
- Set up the erlang system monitor by using `erlang:system_monitor()`. This will allow you to track long running garbage collection periods and large process heaps among other things.

## Open-ended Questions

1. How could you verify if a leak is caused by forgetting to kill processes, or by processes using too much memory on their own?
- I suspect you can also run `proc_count` and then get an overview of how many processes there are and how much memory they area each using. From there, you can get a rough idea of whether you have lots of processes or a few processes which consume lots of memory

2. A process opens a 150MB log file in binary mode to go extract a piece of information from it, and then stores that information in an ETS table. After figuring out you have a binary memory leak, what should be done to minimize binary memory usage on the node?
- Use binary copy  if keeping only a small fragment
- Move work the binary to a temporary one-off process that will die when it's done
- Add hibernation calls when appropriate

3. What could you use to find out if ETS tables are growing too fast?
- Call `ets:i()`

4. What steps should you go through to find out that a node is likely suffering from fragmentation? How could you disprove the idea that is could be due to a NIF or driver leaking memory?
- See section 7.3. In short, we can call `recon_alloc:memory(usage)` which will return a value between 0 and 1. If the value is close to 1 there's probably no issue. You are just using a lot of memory
- Additionally, you can also check if `recon_alloc:memory(allocated)` matches with what the OS reports

5. How could you find out if a process with a large mailbox (from reading `message_queue_len`) seems to be leaking data from there, or never handling new messages?
- I'm guessing you can use `process_info/2` to check the message queue and then infer from there but honestly I'm not too sure about the answer to this question.

6. A process with a large memory footprint seems to be rarely running garbage collections. What could explain this?
- Possibly the ETS table is leaking memory and/or there is a large amount of native code which can't be garbae collected.

7. When should you alter the allocation strategies on your nodes? Should you prefer to tweak this, or the way you wrote code?
- This seems like a trial and error process. You need to have a good understanding of the memory load and usage. This, coupled together with in-depth testing can help you decide whether to change allocation strategies. Based on the documentation, it seems that tweaking memory allocation is a complex ordeal and that changing the way code was written might be a better option. 


## Hands-on(WIP)
1. Using any system you know or have to maintain in Erlang (including toy systems), can you figure out if there are any binary memory leaks on there?


## References
- https://ferd.github.io/recon/recon_alloc.html
