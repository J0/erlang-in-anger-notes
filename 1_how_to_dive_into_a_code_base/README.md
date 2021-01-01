# 1 How To Dive Into A Code Base

## Review Questions (Theory)

1. How do you know if a code base is an application? A release?
Seems like there'll be a `relx` tuple in the `rebar.config`

2. What differentiates an application from a library application?
> Library applications will usually have modules named `appname_something` and one module called `appname`
> For regular OTP applications, there are two potential modules that act as the entry point: `appname` and `appname_app`
3. What can be said of processes under a `one_for_all` scheme for supervision?
> `one_for_all` is used for processes that entirely depend on each other

4. Why would someone use a `gen_fsm` behaviour over a `gen_server`?
> `gen_fsm` will deal with a sequence of events or inputs and react depending on them, as a Finite State Machine
> `gen_server` holds resources and tends to follow client/server patterns(or more generally request/response patterns)


## Review Questions(Practical)

1. Is this application meant to be uesd as a library? A standalone system?

 It appears to be an application.

2. What does it do?

It is an application built to demonstrate the capabilities of recon. It creates 26 different processes which have odd behaviors. One can then use Recon to determine what is going on

What is [recon](https://ferd.github.io/recon/) exactly?
 > Recon is a library to be dropped into any other Erlang project, to be used to assist DevOps people diagnose problems in production nodes.


3. Does it have any dependencies? What are they?

Yes, it has dependencies.

`rebar3 tree` gives

└─ council─1.0.0 (project app)
   ├─ gproc─0.5.0 (hex package)
   └─ recon─2.3.1 (hex package)

4. The app's `README` mentions being non-deterministic. Can you prove if this is true? How?

Run the program a few times and see if you get the same behavior. If you can find inconsistent behavior it's non deterministic. If it's constent for multiple runs then it is likely but not guaranteed to be deterministic.

5. Can you express the dependency chain of applications in there? Generate a diagram of them?

I tried using [Rebar deps graph](https://github.com/emedia-project/rebar_deps_graph) tool to generate a dependency graph but it seems dated. Wasn't able to get the result I wanted so I want with `rebar3 tree` which gives the result in q3

6. Can you add more processes to the main application than those described in the `README`

I can add another process 'yada' in `apps/council/src/council.app.src` and do `rebar3 relase` and start binary in foreground. 

