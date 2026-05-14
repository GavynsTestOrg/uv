# Release Notes

## 2026-05-14

* Improved wording in user facing messages to make the text clearer without changing behavior.
* Accelerated repeat runs of `uv lock`, `uv sync`, `python find`, and `python pin` when an installed Python already matches the request.
* Added a dedicated Bazel integration guide so setup and sign in steps are easier to find and follow.
* Streamlined wheel builds so package creation completes more efficiently.
* Reduced duplicate entries in exported package reports so dependency listings stay cleaner and easier to review.
* Simplified Windows Intel GPU detection so hardware checks keep working with lower system overhead.