# Tool-rt-trace-bpf

## Purpose
Crucible tool for BCC-based real-time kernel tracing during benchmark execution. Captures kernel trace events, backtraces, and summary statistics across designated CPU cores using eBPF/BCC without CDM metric indexing.

## Languages
- Bash: start and stop lifecycle scripts (`rt-trace-bpf-start`, `rt-trace-bpf-stop`)
- Python: post-processor stub (`rt-trace-bpf-post-process.py`)

## Key Files
| File | Purpose |
|------|---------|
| `rt-trace-bpf-start` | Mounts debugfs, loads kheaders, and launches `rt-trace-bcc.py` in background |
| `rt-trace-bpf-stop` | Sends SIGINT to collector, waits for exit, and compresses trace output with xz |
| `rt-trace-bpf-post-process.py` | Post-processor script (raw trace tool; non-CDM indexed) |
| `rickshaw.json` | Rickshaw integration: endpoint allow/block lists, file deployment, post-process script |
| `workshop.json` | Engine image build requirements (builds BCC and rt-trace-bpf from source) |
| `tool-metadata.json` | Machine-readable description and CDM-indexed status (consumed by `crucible tools list`) |
| `multiplex.json` | Parameter validation rules and `defaults` preset for multiplex (mirrors benchmark `multiplex.json`) |

## Configuration
- `--cpu-list <list>` — List of CPU cores to trace (e.g. `0-3`, `0,2,4`, default: `0`)

## Architecture
- `rt-trace-bpf-start` — Validates `--cpu-list`, mounts `debugfs` at `/sys/kernel/debug` if unmounted, loads `kheaders` kernel module, and runs `/root/rt-trace-bpf/rt-trace-bcc.py --cpu-list <cpu_list> --summary --backtrace` in background saving PID to `rt-trace-bpf-pids.txt`
- `rt-trace-bpf-stop` — Reads `rt-trace-bpf-pids.txt`, issues `kill -s SIGINT`, waits for processes to exit cleanly, and compresses `rt-trace-bpf-stderrout.txt` into `.xz`
- Raw trace output and kernel header info are preserved as run artifacts

## Testing
- Validate syntax: `bash -n rt-trace-bpf-start && bash -n rt-trace-bpf-stop`
- Schema validation: validate `tool-metadata.json` against `schema/tool-metadata.json` and `multiplex.json` against `req-schema.json`
- Multiplex evaluation: `python3 multiplex.py --requirements multiplex.json --input in.json --output out.json --flat`

## Conventions
- Primary branch is `main`
- Profiler tool allowed on compute and profiler roles, blocked on client and server roles
- Standard Bash and Python modelines with 4-space indentation
