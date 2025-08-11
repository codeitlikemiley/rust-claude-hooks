# Claud Hooks for Rust


This is an attempt to streamline my workflow with Rust on claude


## `check` hooks usage


Make it executable:

```sh
# move check to ~/.local/bin/check
./chmod +x ./check
```

Basic usage:

```sh
# use the cwd as the path
./check
```

specifying the path

```sh
./check $(pwd)/project-a
# or
./check ./project-a
```

example output:

```sh
{
  "diagnostics": [
    {
      "severity": "warning",
      "code": "unused_imports",
      "message": "unused import: `super::*`",
      "location": {
        "file": "mobile/src/main.rs",
        "range": {
          "start": {
            "line": 51,
            "column": 9
          },
          "end": {
            "line": 51,
            "column": 17
          }
        }
      }
    }
  ],
  "summary": {
    "has_errors": false,
    "has_warnings": true,
    "error_count": 0,
    "warning_count": 1
  }
}
```



## `check` as PreToolUse

```sh
./check --pre
./check --pre $(pwd)/project-a
./check --pre ./project-a
```

output:

```sh
{
  "reason": "Code has 1 warning(s) but proceeding",
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "additionalContext": {
      "diagnostics": [
        {
          "severity": "warning",
          "code": "unused_imports",
          "message": "unused import: `super::*`",
          "location": {
            "file": "mobile/src/main.rs",
            "range": {
              "start": {
                "line": 51,
                "column": 9
              },
              "end": {
                "line": 51,
                "column": 17
              }
            }
          }
        }
      ],
      "summary": {
        "has_errors": false,
        "has_warnings": true,
        "error_count": 0,
        "warning_count": 1
      }
    }
  }
}
```

## `check` as PostToolUse

```sh
./check --post
./check --post $(pwd)/project-a
./check --post ./project-a
```

output:

```sh
{
  "reason": "Code compiled with 1 warning(s)",
  "hookSpecificOutput": {
    "hookEventName": "PostToolUse",
    "additionalContext": {
      "diagnostics": [
        {
          "severity": "warning",
          "code": "unused_imports",
          "message": "unused import: `super::*`",
          "location": {
            "file": "mobile/src/main.rs",
            "range": {
              "start": {
                "line": 51,
                "column": 9
              },
              "end": {
                "line": 51,
                "column": 17
              }
            }
          }
        }
      ],
      "summary": {
        "has_errors": false,
        "has_warnings": true,
        "error_count": 0,
        "warning_count": 1
      }
    }
  }
}
```

## Deny all wanings

- PreToolUse
```sh
./check --pre --deny-warnings
./check --pre --deny-warnings $(pwd)/project-a
./check --pre --deny-warnings ./project-a
```

- PostToolUse

```sh
./check --post --deny-warnings
./check --pre --deny-warnings $(pwd)/project-a
./check --pre --deny-warnings ./project-a
```

The only difference with the output above is 
1. decision is set to `block`
2. exit code is 2









