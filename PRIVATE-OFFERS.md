# Non-mutating regular clipboard offer helpers

Base revision: `8a270232f1bfc416c781273c98a3f52a812a0bba`.

Expose the regular data devices belonging to a client, an opaque real-selection
change counter, and re-offering of that real selection to the focused client.
Neither data-control protocol participates in the per-client device list or the
re-offer. No API here changes source ownership or calls source cancellation.

The compositor is responsible for validating the destination, creating and
revoking private offers, asynchronous writes, cancellation and cleanup. Offers
are scoped to a client and seat, never a widget. A client may itself bridge data
to another system; these APIs do not make an X11 bridge private.

Validated through Niri's `clipboard_paste` protocol tests: a preexisting binary
source remains uncancelled, both MIME types and exact contents survive multiple
large private receives, both data-control protocols see the original source
before/during/after the operation (including new clients), and a concurrent real
copy advances the counter and wins over cleanup. Native GTK and Kitty insertion
also passed with a real cliphist watcher running.
