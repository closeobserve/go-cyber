### Calculate rank in parallel (once at 600 blocks)

1. Down during calculation -> need to restart with rank calculation with old context
2. Need to hold and show to users all new links

We need at least two contexts.  First, context with current rank graph.
It should be updated every 600 blocks from the second context.
Second, context only with new links that comes during 600 blocks calculation.

In block `n: (n % 600 == 0)` we should have calculation results somewhere 
(so may be we even don't need to synchronize calculation goroutine except cases of failure)
and come up with new rank hash.
Also we should update main rank storage with all new links.

Problems:

1. What if validator node falls and after restart it has not enough time to recalculate rank
 (cause it need to start calculation from scratch).
2. We cannot predict block where calculation will be finished.
Need to hold calculation results till block `n: (n % 600 == 0)`?
Also we could fall while holding calculation results.
May be we need third context...

Don't save rank. Calculate it before node start.
If node falls then it should recalculate rank.
Node doesn't participate in consensus till rank not calculated (obvious).