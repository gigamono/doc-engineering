## Engineering Documentation

### Table of Contents

1. [Error Handling](#error-handling)

### Error Handling <a name="error-handling" />

- All errors should be returned as [`SystemError`](https://github.dev/gigamono/utilities/blob/main/lib/messages/error.rs#L7-L13) except in external-facing request handlers.

  #### Bad

  ```rs
  fn connect(conn_str: &str) -> io::Result<SQliteConnection> {
      SQliteConnection::establish(conn_str)
  }
  ```

  #### Good

  ```rs
  fn connect(conn_str: &str) -> utilities::result::Result<SQliteConnection> {
      SQliteConnection::establish(conn_str).map_err(|err| SystemError::Conn {
          ctx: format!("connecting to db, `{}`", conn_str),
          src: err,
      })
  }
  ```

- `SystemError`s should provide high-level context information of what went wrong.

  #### Bad

  ```rs
  fn connect(conn_str: &str) -> Result<SQliteConnection> {
      SQliteConnection::establish(conn_str).map_err(|err| SystemError::Conn {
          ctx: "error ocurred",
          src: err
      })
  }
  ```

  #### Good

  ```rs
  fn connect(conn_str: &str) -> Result<SQliteConnection> {
      SQliteConnection::establish(conn_str).map_err(|err| SystemError::Conn {
          ctx: format!("connecting to db, `{}`", conn_str),
          src: err,
      })
  }
  ```
