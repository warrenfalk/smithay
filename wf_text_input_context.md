# Committed surrounding text for compositor integrations

The text-input handle exposes the active focused field's committed surrounding
text, UTF-8 byte offsets, content hint/purpose, protocol object, surface, client
commit serial and most recent activation serial. Pending requests remain
invisible until commit. Partial commits retain unchanged state; enable resets
the field, and disable, leave, destruction or a surface change makes old context
unavailable. Debug formatting redacts surrounding text.

Reading this state neither claims the input-method-v2 slot nor acknowledges
client state. A compositor may use its existing input-method mediation flag to
receive text-input state without an external input method. Newly bound text
input objects receive enter when that flag is set. The low-level mediation
setter changes only the flag and reports whether it changed, allowing compositors
to preserve a real input method's enter/leave lifecycle.

Consumers decide field sensitivity, freshness and insertion policy. Context is
an application-provided excerpt, not a whole-document or widget-identity API.
The Niri fork supplies connection ownership, sensitive-field filtering, bounded
state acknowledgement and revision-guarded private paste. Keep the cache unit
test and Niri's committed/pending, re-enable, lazy-bind, ownership and UTF-8
protocol regressions when rebasing.
