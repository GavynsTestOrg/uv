# Release Notes

## 2026-05-14

* Improved wording in user facing messages and related documentation notes to make the text clearer without changing behavior.
* Accelerated repeat runs of `uv lock`, `uv sync`, `python find`, and `python pin` when a matching local Python is already available.
* Added a dedicated Bazel integration guide so Bazel setup and sign in steps are easier to find and follow.
* Streamlined wheel builds so packaging completes more efficiently.
* Reduced duplicate entries in exported dependency inventories so package reports stay cleaner and easier to review.
* Simplified Windows Intel GPU detection so hardware checks keep working with less extra system overhead.