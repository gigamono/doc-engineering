## Engineering Documentation

### Table of Contents

1. [Error Handling](#error-handling)

### Error Handling <a name="error-handling" />

- All errors should be returned as `utilities::errors::Error` except in external-facing request handlers.

  #### Bad

  ```rs
  fn connect(conn_str: &str) -> io::Result<SQliteConnection> {
      SQliteConnection::establish(conn_str)
  }
  ```

  #### Good

  ```rs
  fn connect(conn_str: &str) -> utilities::result::Result<SQliteConnection> {
      SQliteConnection::establish(conn_str)?
  }
  ```

- Re-propagated `Error`s should provide high-level context information of what you were doing leading to the error.

  #### Bad

  ```rs
  fn connect(conn_str: &str) -> Result<SQliteConnection> {
      SQliteConnection::establish(conn_str).context("an error ocurred")
  }
  ```

  #### Good

  ```rs
  fn connect(conn_str: &str) -> Result<SQliteConnection> {
      SQliteConnection::establish(conn_str).context(r#"connecting to database "{}""#, conn_str)
  }
  ```

  Note the gerund, `connecting`. It should describe an ongoing action.

- On the other hand, new errors you introduce to the codebase should tell what went wrong


  #### Bad

  ```rs
  fn connect(conn_str: &str) -> Result<SQliteConnection> {
      Err(custom_error("connecting to database"))
  }
  ```

  #### Good

  ```rs
  fn connect(conn_str: &str) -> Result<SQliteConnection> {
      Err(custom_error(r#"could not connect to database "{}""#, conn_str))
  }
  ```
