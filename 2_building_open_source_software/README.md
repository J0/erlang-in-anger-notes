# 2 Building Open Source Applications


## Review Questions (Theory)

1. Are Erlang Supervision trees started depth-first?breadth-first? Synchronously or Asynchronously

The are started top-down from left to right. See [this manual](https://adoptingerlang.org/docs/development/supervision_trees/) for more details.

Based on a quick search on [the docs](https://erlang.org/doc/design_principles/sup_princ.html) (Do a ctrl-F for "synchronous"). It seems that processes are started synchronously and the supervisor process does not return until all child processes have been started. Shutdown is asynchronous though.


2, What are the three application strategies? What do they do?
> `Permanent`: if the app terminates, the entire system is takend down excluding manual termination of the app with `application:stop/1`
> `transient`: if the app terminates for a normal reason, that's okay. Any other reason for terminatino shuts down the entire system.
> `temporary`: the application is allowed to stop for any reason. It will be reported, but nothing bad will happen.

3. What are the main differences between the directory structure of an app and a release?

>For releases, the structure can a bit different. Releases are collections of applications, and their structures may reflect that.

>Instead of having a top-level app alone in `src`, applications can be nested one level deeper in a `apps` or `lib` directory.

4. When should you use a release?

>If what you're writing is a stand-alone piece of code that could be used by someone building a product, it's likely an OTP application.
>If what you're building is a product that stands on its own and should be deployed by users as-is (or with a little configuration), what you should be building is an OTP release

I guess this ties back to 2. as well

5. [TODO] Give two examples of the type of state that can go in a process' init function, and two examples of the type of state that shouldn't go in a process' init function

Supervised processes provide guarantees during the initialized state

So you
- Shouldn't need a connection to be established so you shouldn't require a database connection and a network connection?

It seems like you can store
- Restart Strategy
- Maximum time before you timeout

This isn't much of an answer so I'm going to add a TODO and come back to it later.


## Review Questions(Practical)


1. Extract the main application hosted in the release to make it independent, and includable in other projects.

@TODO

2. Host the application somewhere (Github, Bitbucket, local server), and build a release with that application as a dependency.

@TODO

3. The main application's workers `council_member` starts a server and connects to it in its `init/1` function. Can you make this connection happen outside of the init function's? Is there a benefit to doing so in this specific case?

@TODO

