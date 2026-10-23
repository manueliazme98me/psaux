# psaux

A Bash implementation of `ps aux` that avoids creating new processes by reading information directly from `/proc`.

## Overview

`psaux.bash` is designed for constrained Unix environments where the usual process-inspection tools are unavailable because the system cannot spawn additional processes. In those situations, the script can still provide a practical view of running processes without invoking external commands such as `ps`, `awk`, `cat`, or other subprocess-based utilities.

This makes it useful for troubleshooting, emergency diagnostics, and situations where a shell is still available but process creation is restricted or exhausted.

## Features

- Bash-only implementation
- No child processes created during the main listing loop
- Reads process state directly from `/proc`
- Displays process metadata similar to `ps aux`, including:
  - user
  - PID
  - CPU usage
  - memory usage
  - virtual size
  - resident set size
  - TTY
  - state
  - start time
  - elapsed time
  - command

## Requirements

- Bash 4 or newer
- Linux-based system with `/proc`
- A working terminal environment

## Usage

```bash
./psaux.bash
```

You can also run it directly with Bash:

```bash
bash ./psaux.bash
```

## Example output

```text
USER     PID   %CPU  %MEM  VSZ   RSS   TTY    STAT  START  TIME  COMMAND
root     1     0.0   0.1   1234  456   ?      Ss     10:12  0:00  /sbin/init
root     42    0.2   0.3   3456  789   pts/0  S      10:20  0:01  bash
...
```

## How it works

The script avoids forking by using Bash builtins and direct reads from `/proc/<pid>/stat`, `/proc/<pid>/status`, and related kernel procfs entries. It reconstructs key fields needed for a `ps aux`-style view without invoking external programs.

This approach is intentionally best-effort and is best suited to diagnostic and recovery scenarios rather than full replacement of the standard `ps` utility.

## Limitations

- It is not a complete reimplementation of all `ps` behavior.
- Output formatting and field semantics may differ from standard Linux tools.
- Some process attributes are approximated rather than computed using the full set of kernel interfaces available to native utilities.
- It is intended for constrained or emergency shell environments, not as a general-purpose replacement for standard system monitoring tools.

## Security and compatibility

Use this script in environments where process creation is restricted or where a lightweight diagnostic tool is needed. It does not require a privileged daemon or external binary, but it does rely on the host system exposing `/proc` entries and standard Linux process metadata.

## License

This project is distributed under the terms of the repository license. See the project license file for details.

## Notes

This utility is most valuable when the standard toolchain is unavailable in a minimal or broken shell environment. It is a practical workaround, not a substitute for fully featured process management tools.
