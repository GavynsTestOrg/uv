# Release Notes

## 2026-05-14

* Improved wording in user facing messages to make the text clearer without changing behavior.
* Accelerated repeat runs of `uv lock`, `uv sync`, `python find`, and `python pin` by avoiding Python download manifest loading when an installed Python already matches the request.
* Added dedicated Bazel authentication helper guidance in a Bazel integration guide so setup is easier to find and follow.
* Streamlined asynchronous wheel ZIP writing so wheel builds complete more efficiently.
* Reduced duplicate entries in generated software bill of materials output so dependency listings stay cleaner and easier to review.
* Simplified Windows Intel XPU detection so hardware checks keep working with lower system overhead.