https://ibc.cosmos.network/main/ibc/overview.html#receipts-and-timeouts

* The timeoutHeight indicates a consensus height on the destination chain after which the packet is no longer be processed, and instead counts as having timed-out. 

* The timeoutTimestamp indicates a timestamp on the destination chain after which the packet is no longer be processed, and instead counts as having timed-out.

    // NOTE timeout is the block height after which this transaction will not
    // be processed by the chain