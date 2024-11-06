
# Environment Variable Management in Nushell

In Nushell, environment variables are managed using the `$env` variable, which functions as a record containing key-value pairs. This approach is more structured compared to traditional shells like Bash.

---

## Setting Environment Variables

To set an environment variable in Nushell, use the following syntax:

```nu
$env.VARIABLE_NAME = 'value'
```

**Example**:

```nu
$env.AWS_ACCESS_KEY_ID = 'ASIDGWF7T'
```

> **Note**: Ensure there are spaces around the `=` sign to avoid syntax errors.

---

## Accessing Environment Variables

Retrieve the value of an environment variable like so:

```nu
$env.VARIABLE_NAME
```

**Example**:

```nu
$env.AWS_ACCESS_KEY_ID
```

---

## Listing Environment Variables

To view all environment variables in a table format, you can use:

```nu
$env | table -e
```

This command pipes the `$env` record into the `table` command with the `-e` flag, which formats the environment variables into a structured table.

---

## Comparison with Other Shells

| Feature                        | Nushell Syntax                     | Traditional Shell (e.g., Bash) Syntax       |
|--------------------------------|------------------------------------|--------------------------------------------|
| **Setting Variables**          | `$env.VARIABLE_NAME = 'value'`     | `export VARIABLE_NAME='value'`             |
| **Accessing Variables**        | `$env.VARIABLE_NAME`               | `$VARIABLE_NAME`                           |
| **Listing Variables**          | `$env | table -e`                  | `printenv` or `env`                        |

---

## Advantages of Nushell's Approach

| Advantage                   | Description                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| **Consistency**             | Environment variables are managed through the `$env` record, promoting a structured syntax. |
| **Readability**             | `$env` provides clear access and separation from regular variables.        |
| **Enhanced Table View**     | Using `$env | table -e` offers a neatly formatted table for all variables. |

---

Nushell encapsulates environment variables within the `$env` record, providing a more structured and consistent approach to environment management. This differs from traditional shells, where variables are accessed and managed directly, often without clear separation from other shell variables.
