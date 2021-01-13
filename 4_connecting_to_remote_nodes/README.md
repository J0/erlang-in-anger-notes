# 4 Connecting To Remote Nodes

## Review Questions (Theory)

1. What are the 4 ways to connect to a remote node?
- Job Control Mode, Remsh, SSH Daemon, and Named pipes
2. Can you connect to a node that wasn't given a name?
- Seems like it! You can pass in `$COOKIE` to remsh.
3. What's the command to go into the Job Control Mode (JCL)?
- Control-G
4.  Which method(s) of connecting to a remote shell should you avoid for a system that outputs a lot of data to standard output?
- You probably don't want to use Job Control mode because all the output coming from remote evaluation will be forwarded back to the local shell

5. What instances of remote connections shouldn't be disconnected using command G
- Probably named pipes. You will want to close with `^D` instead so that you cna disconnect from the shell while leaving the program running.

6. What command(s) should never be used to disconnect from a session?
- Don't use `q()` or `init:stop()` as it will terminate the remote host
7. Can all of the methods mentioned support having multiple users connected onto the same Erlang node without issue?
