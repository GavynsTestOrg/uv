# Release Notes

## 2026-05-14

* Improved wording in user facing assertion text and related comments to make messages read more clearly without changing behavior.
* Accelerated warm runs of `uv lock`, `uv sync`, `python find`, and `python pin` by skipping Python download metadata loading when an installed Python already satisfies the request.
* Added a dedicated Bazel integration guide so Bazel setup and authentication steps are easier to find and follow.
* Streamlined wheel builds to write ZIP contents more efficiently, which improves packaging performance when building wheels.
* Reduced duplicate package entries in CycloneDX software bill of materials output so exported dependency reports stay cleaner and easier to review.
* Simplified Intel XPU detection on Windows through native system APIs, which keeps hardware detection working while reducing extra dependency overhead.