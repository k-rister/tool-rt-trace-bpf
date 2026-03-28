# RT-Trace-BPF Tool

## Purpose
BCC-based real-time kernel tracing tool. Captures kernel trace events using eBPF/BCC during benchmark execution.

## Languages
- Bash: start/stop scripts (`rt-trace-bpf-start`, `rt-trace-bpf-stop`)
- Perl: post-processor (`rt-trace-bpf-post-process`, stub — uses `toolbox::metrics`, `toolbox::json`, `toolbox::cpu` from `$TOOLBOX_HOME/perl`)

## Key Files
| File | Purpose |
|------|---------|
| `rt-trace-bpf-start` | Launches `rt-trace-bcc.py` with CPU list, summary, and backtrace flags |
| `rt-trace-bpf-stop` | Stops collector, waits for exit, compresses output with xz |
| `rt-trace-bpf-post-process` | Post-processing stub for collected trace data |
| `rickshaw.json` | Rickshaw integration: endpoint allow/block lists, file deployment, post-process script |
| `workshop.json` | Engine image build: compiles BCC and rt-trace-bpf from source |

## Conventions
- Primary branch is `main`
- Runs as a profiler tool on master/worker/profiler/compute roles, blocked on client/server
- Depends on BCC and kernel headers at runtime
