# WP4 Benchmarks

This directory contains performance benchmarking tools and utilities for Work Package 4 (WP4), focusing on memory and cache latency analysis across different architectures.

## Directory Structure

```
wp4_benchmarks/
├── cache_lat/          # Cache latency benchmarking tools
│   ├── lat_cache_licheepi4a.c    # LicheePi 4A specific cache benchmark
│   ├── lat_cache_sifivep550.c    # SiFive P550 specific cache benchmark
│   └── lat_cache_x86.c           # x86 architecture cache benchmark
└── mem_lat/            # Memory and disk latency benchmarking suite
    ├── bench_and_plot.sh         # Automated benchmarking and plotting script
    ├── config.cfg                # Configuration file for benchmarks
    ├── config.cfg.example        # Example configuration template
    ├── lat_disk.c                # Disk I/O latency benchmark
    ├── lat_plot.py               # Python plotting utility
    ├── files/                    # Output files and test data
    └── plots/                    # Generated benchmark plots
```

## Benchmark Categories

### Cache Latency (`cache_lat/`)
Architecture-specific cache performance benchmarks measuring:
- L1 cache hit/miss latencies
- Cache line access patterns
- Memory hierarchy performance characteristics

Supports multiple architectures:
- **LicheePi 4A**: RISC-V based development board
- **SiFive P550**: High-performance RISC-V core
- **x86**: Traditional x86_64 architecture

### Memory Latency (`mem_lat/`)
Comprehensive memory and storage performance analysis including:
- Disk I/O latency measurements
- Memory access pattern analysis
- Automated benchmarking with visualization
- Configurable test parameters

## Quick Start

### Cache Benchmarks
```bash
cd cache_lat/
gcc -o lat_cache_licheepi4a lat_cache_licheepi4a.c
./lat_cache_licheepi4a
```

### Memory Benchmarks
```bash
cd mem_lat/
# Configure benchmark parameters
cp config.cfg.example config.cfg
# Edit config.cfg as needed

# Run automated benchmark suite
./bench_and_plot.sh
```

## Architecture Focus

These benchmarks are designed with particular attention to RISC-V architecture performance characteristics, supporting the evaluation and optimization of:
- RISC-V cache hierarchies
- Memory subsystem performance
- Storage I/O patterns
- Cross-architecture performance comparison

## Output and Analysis

Benchmarks generate both numerical results and visualization plots for:
- Latency measurements in nanoseconds and CPU cycles
- Performance comparison across different configurations
- Statistical analysis of latency distributions
- Graphical representations of benchmark results

## Documentation

For detailed information about specific benchmark suites:
- See `cache_lat/README.md` for cache latency benchmark details
- See `mem_lat/README.md` for memory latency benchmark documentation

## Requirements

- GCC compiler with C99 support
- Python 3.x (for plotting utilities, if needed)
