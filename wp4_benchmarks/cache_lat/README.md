# Cache Latency Benchmarks

This directory contains architecture-specific cache performance benchmarking tools designed to measure cache hierarchy latencies across different processor architectures.

## Architecture Support

### LicheePi 4A (`lat_cache_licheepi4a.c`)
Cache latency benchmark optimized for the LicheePi 4A development board:
- **Architecture**: RISC-V based system
- **L1 Cache**: 64KB instruction + data cache
- **L2 Cache**: 1MB unified cache
- **Cache Line**: 64-byte cache line size
- **CPU Affinity**: Pinned to core 3 for consistent measurements

### SiFive P550 (`lat_cache_sifivep550.c`)
Performance benchmark for SiFive P550 high-performance RISC-V core. The program uses the same mechanisms as the LicheePi4a, only updating cache hierarchy and cache sizes.

### x86 Architecture (`lat_cache_x86.c`)
Traditional x86_64 cache latency measurements.

## Benchmark Methodology

### Cache Hit Measurement
The benchmarks measure L1 cache hit latency by:
1. **Allocation**: Allocating cache-line aligned buffer
2. **Warm-up**: Pre-loading data into L1 cache with `memset()`
3. **Access Pattern**: Sequential access with cache-line stride
4. **Timing**: High-precision nanosecond timing using `CLOCK_MONOTONIC`

### Cache Miss Measurement
Cache miss latency is measured through:
1. **Cache Eviction**: Allocating large buffer (4× L2 size) to flush caches
2. **Memory Thrashing**: Writing to eviction buffer to clear cache hierarchy
3. **Cold Access**: Re-accessing original buffer after cache eviction
4. **Latency Calculation**: Measuring increased access time

## Technical Features

### CPU Affinity
- **Core Pinning**: Uses `sched_setaffinity()` to bind process to specific CPU core
- **Consistency**: Eliminates cache migration effects between cores
- **Reproducibility**: Ensures consistent cache state across runs

### Memory Alignment
- **Cache Line Alignment**: Uses `aligned_alloc()` for optimal cache behavior
- **Access Patterns**: Sequential access with cache-line boundaries
- **Architecture Awareness**: Respects 64-byte cache line requirements

### High-Precision Timing
- **Nanosecond Resolution**: `clock_gettime(CLOCK_MONOTONIC)` for precise measurements
- **Low Overhead**: Minimal timing overhead to avoid measurement interference
- **Statistical Accuracy**: Multiple accesses for average latency calculation

## Usage

### Compilation
```bash
# For LicheePi 4A
gcc -o lat_cache_licheepi4a lat_cache_licheepi4a.c -lpthread

# For SiFive P550
gcc -o lat_cache_sifivep550 lat_cache_sifivep550.c -lpthread

# For x86
gcc -o lat_cache_x86 lat_cache_x86.c -lpthread
```

### Execution
```bash
# Run architecture-specific benchmark
./lat_cache_licheepi4a

# Example output:
# L1 cache hit latency: 2.15 ns/access
# Cache miss latency: 45.30 ns/access
```

## Measurement Details

### LicheePi 4A Configuration
Based on the provided code, the LicheePi 4A benchmark uses:
- **L1 Cache Size**: 64KB (64 * 1024 bytes)
- **L2 Cache Size**: 1MB (1024 * 1024 bytes)
- **Cache Line Size**: 64 bytes
- **Target Core**: Core 3 (CORE_ID = 3)
- **Eviction Buffer**: 4MB (4 × L2 size)

### Access Pattern
The benchmark performs:
1. **Sequential Access**: Stride through buffer with cache-line granularity
2. **Cache-Line Boundary**: Each access targets a new cache line
3. **Read-Modify-Write**: Simple increment operation (`buf[i] += 1`)
4. **Latency Calculation**: Total time divided by number of cache lines accessed

### Cache Hierarchy Analysis
The two-phase measurement reveals:
- **L1 Hit Latency**: Fast access when data is in L1 cache
- **Memory Latency**: Slower access after cache eviction (includes L2, L3, and main memory)

## Architecture Considerations

### RISC-V Specific
- **Cache Coherency**: RISC-V cache coherency protocols
- **Memory Ordering**: RISC-V memory consistency model
- **Performance Counters**: May integrate with RISC-V performance monitoring

### Cross-Architecture Comparison
The multi-architecture approach enables:
- **Performance Comparison**: Direct latency comparison across architectures
- **Cache Efficiency**: Analysis of different cache designs
- **Memory Hierarchy**: Understanding of various memory subsystems

## Requirements

### System Requirements
- **GCC Compiler**: C99 support with POSIX extensions
- **pthread Library**: For CPU affinity and threading functions
- **Root/Scheduler Access**: May require elevated privileges for CPU affinity

### Hardware Requirements
- **Multi-core System**: CPU affinity requires multiple cores
- **Cache Hierarchy**: Target architecture with L1/L2 cache levels
- **Memory Bandwidth**: Sufficient memory for cache eviction tests

## Limitations and Considerations

### Measurement Accuracy
- **System Load**: Other processes may affect cache state
- **Frequency Scaling**: CPU frequency changes can affect timing
- **Memory Pressure**: System memory usage impacts cache behavior

### Architecture Variations
- **Cache Sizes**: Different processors have varying cache configurations
- **Cache Policies**: Write-back vs write-through affects latency
- **Prefetching**: Hardware prefetchers may influence measurements

## Troubleshooting

### Common Issues
1. **Permission Errors**: CPU affinity may require special permissions
2. **Compilation Errors**: Ensure pthread library is linked
3. **Inconsistent Results**: Check for system load and frequency scaling
4. **Core Availability**: Verify target core exists on the system
