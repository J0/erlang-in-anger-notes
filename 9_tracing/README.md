# 9 Tracing
1. Why is debugger use generally limited on Erlang?

- With a debugger you can insert break points and step through a program. However, this model of debugging is incompatible with many Erlang programs-- execution is concurrent and if you insert a break point on one process the rest of the processes still continue to run. This limits the extent to which you can debug the program because as soon as a process needs to interact with the one you're debugging its calls start to time out and crash. This could possibly cause the entire node to fail with it

2. What are the options you can use to trace OTP processes?
- In short you can use `sys`, `dbg`, tracing BIFs, `redbug`, `recon_trace`.

- `sys` comes standard with OTP and allows the user to set custom tracing functions, log all kinds of events, and so on. It’s generally complete and fine to use for development. It suffers a bit in production because it doesn’t redirect IO to remote shells, and
doesn’t have rate-limiting capabilities for trace messages. It is still recommended to read the documentation for the module.
- `dbg` also comes standard with Erlang/OTP. Its interface is a bit clunky in terms of
usability, but it’s entirely good enough to do what you need. The problem with it is
that you have to know what you’re doing, because dbg can log absolutely everything
on the node and kill one in under two seconds.
- tracing BIFs are available as part of the erlang module. They’re mostly the raw blocks used by all the applications mentioned in this list, but their lower level of abstraction makes them rather difficult to use.
- `redbug` is a production-safe tracing library, part of the `eper` suite. It has an internal rate-limiter, and a nice usable interface. To use it, you must however be willing to add in all of `eper`’s dependencies. The toolkit is fairly comprehensive and can be a very interesting install.
- `recon_trace` is recon’s take on tracing. The objective was to allow the same levels
of safety as with redbug, but without the dependencies. The interface is different, and the rate-limiting options aren’t entirely identical. It can also only trace function calls, and not messages.

3. What determines whether a given set of functions or processes get traced?
- This is dependent on whether their pids match and/or their trace patterns match

4. How can you stop tracing with `recon_trace`? With other tools?
- You can cancel tracing with `recon_trace` by running `recon_trace:clear`

5. How can you trace non-exported function calls?
- You can pass in `{ scope, local}` argument to `recon_trace`


## Open-ended questions
1. When would you want to move time stamping of traces to the VM's trace mechanisms directly? What would be a possible downside of doing this?	

2. Imagine that traffic sent out of a node does so over SSL, over a multi-tenant system. However, due to wanting to validate data sent (following a customer complain), you need to be able to inspect what was seen clear text. Can you think up a plan to be able to snoop in the data sent to their end through the \module{ssl} socket, without snooping on the data sent to any other customer?

## Hands-On

Using the code at [Ferd's demo](https://github.com/ferd/recon\_demo) (these may require a decent understanding of the code there):
1.  Can chatty processes `council_member` message themselves? **hint: can this work with registered names? Do you need to check the chattiest process and see if it messages itself?**
2. Can you estimate the overall frequency at which messages are sent globally?
3. Can you crash a node using any of the tracing tools? (**hint: dbg makes it easier due to its greater flexibility**
