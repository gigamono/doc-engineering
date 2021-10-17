## Engineering Documentation
### Table of Contents
1. [Error Handling](#error-handling)


### Error Handling <a name="error-handling" />

- All errors should be returned as [`SystemError`](https://github.dev/gigamono/utilities/blob/main/lib/messages/error.rs#L7-L13) except in external-facing request handlers.
- `SystemError`s should provide high-level context information of what went wrong.

