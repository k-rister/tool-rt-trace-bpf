# tool-rt-trace-bpf

BCC-based real-time kernel tracing tool for the [crucible](https://github.com/perftool-incubator/crucible) performance testing framework. Captures kernel trace events using eBPF/BCC during benchmark execution.

Built from upstream [rt-trace-bpf](https://github.com/xzpeter/rt-trace-bpf) and [BCC](https://github.com/iovisor/bcc).

## Integration

Rt-trace-bpf runs as a profiler tool on endpoint nodes. It is allowed on profiler, master, worker, and compute collector roles but blocked on client and server roles. The collector invokes `rt-trace-bcc.py` with configurable CPU list, summary, and backtrace options.
