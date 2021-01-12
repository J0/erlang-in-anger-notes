
## Review Questions (Theory)

1. How can you choose where a crash dump will be generated?
- You can set the `ERL_CRASH_DUMP` environment variable
2. What are the common avenues to explore if the crash dump shows that the node ran out of memory?
- You might have one of:
    1. Memory Fragmentation
    2. Memory leaks in C code or drivers
    3. Lots of memory that got to be garbage-collected before generating the crash dump
3. What should you look for if your process count is suspiciously low?
- One possible issue is that a specific application has reached its maximal restart frequency within supervisors and that prompted the node to shutdown. Check the error logs that led to the failure.
4. If you find the node died with a process having a lot of memory, what could you do to find out which one it was?
- You can head to the "Memory" section of the dump and check if a type(ETS or Binary, for example) seems to be fairly large. 

## Open-ended Questions(Practical)
Using the analysis of the crash dump in 6.1
1. What are specific outliers that could point to an issue?
Under message queue length the top process has a message queue length of 5010932. Additionally, it has a Heap + Stack memory size of 284745853 words. Finally, at time of crash there were 36421 Processes waiting and 1 Garbing.
2. Does it look like repeated errors are the issue? If not what could it be
Yes, from 6.2 it seems that the node was locked up waiting on IO for `io:format/2` calls