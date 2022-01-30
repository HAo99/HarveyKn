**globrunqget()** `runtime/proc`

Try get a batch of G's from the global runnable queue. sched.lock must be held.

**runqget()** `runtime/proc`

Get g from local runnable queue.
If inheritTime is true, gp should inherit the remaining time in the current time slice.
Otherwise, it should start a new time slice.
Executed only by the owner P.

**findrunnable()** `runtime/proc`

Finds a runnable goroutine to execute.
Tries to steal from other P's, get g from local or global queue, poll network.

**wakefing()**

**runqget()** `runtime/proc`

Get g from local runnalbe queue.
If inheritTime is true, gp should inherit the remaining time in the current time slice.
Otherwise, it should start a new time slice.
Executed only by the owner P.

**netpoll()** `runtime/netpoll_kqueue`

netpoll checks for ready network connections.
Runs list of goroutines that become runnable.
delay < 0: blocks indefinitely
delay == 0: does not block, just polls
delay > 0: block for up to that many nanoseconds

**runqsteal()** `runtime/proc`

Steal half of elemets from local runnable queue of p2 and put onto local runnable queue of p.
Returns one of the stolen elements (or nil if failed)

**runqgrab()** `runtime/proc`

Grabs a batch of goroutines from _p_'s runnable queue into batch.
Batch is a ring buffer starting at batchHead.
Returns number of grabbed goroutines
Can be executed by any P.