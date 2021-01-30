# 9 Tracing

## Review Questions (Theory)
1. When would you want to move time stamping of traces to the VM's trace mechanisms directly? What would be a possible downside of doing this?	
2. Imagine that traffic sent out of a node does so over SSL, over a multi-tenant system. However, due to wanting to validate data sent (following a customer complain), you need to be able to inspect what was seen clear text. Can you think up a plan to be able to snoop in the data sent to their end through the \module{ssl} socket, without snooping on the data sent to any other customer?

## Hands-On

Using the code at [Ferd's demo](https://github.com/ferd/recon\_demo) (these may require a decent understanding of the code there):
1.  Can chatty processes `council_member` message themselves? **hint: can this work with registered names? Do you need to check the chattiest process and see if it messages itself?**
2. Can you estimate the overall frequency at which messages are sent globally?
3. Can you crash a node using any of the tracing tools? (**hint: dbg makes it easier due to its greater flexibility**
