# Memory Latency Benchmarks

This directory contains tools for comprehensive memory and disk I/O latency analysis, providing detailed performance measurements and visualization capabilities.

## Components

### Core Benchmark (`lat_disk.c`)
High-precision disk I/O latency measurement tool that performs:
- **Direct I/O Operations**: Uses `O_DIRECT` flag to bypass system cache
- **Synchronous Writes**: `O_SYNC` ensures data reaches storage device
- **Aligned Memory Access**: Configurable memory alignment for optimal performance
- **Statistical Analysis**: Multiple iterations with min/max/average calculations
- **CPU Frequency Detection**: Automatic CPU frequency analysis for cycle-accurate measurements

### Automation Scripts
- **`bench_and_plot.sh`**: Automated benchmark execution with configuration validation, compilation, and result visualization
- **`lat_plot.py`**: Python plotting utility for generating performance graphs

### Configuration
- **`config.cfg`**: Main configuration file with required variables:
  - `ITERATIONS`: Number of test iterations
  - `MACHINE_NAME`: Identifier for output files and plots
  - `SIZE`: Block size for I/O operations
  - `WRITE_TO_LOCAL_DISK` or `WRITE_TO_DEVICE`: Target specification
  - `DIR_PLOTS`, `DIR_FILES`: Output directory paths
- **`config.cfg.example`**: Template configuration with example settings

### Output Directories
- **`files/`**: Raw benchmark data and temporary test files
- **`plots/`**: Generated performance visualization plots

## Benchmark Features

### Configurable Parameters
The benchmark supports multiple configuration options:

```bash
./lat_disk [OPTIONS]
  -s <size>         Block size for I/O operations (default: 512 bytes)
  -a <alignment>    Memory alignment (default: 512 bytes)
  -i <iterations>   Number of test iterations (default: 100)
  -d <device>       Target device/directory (default: ./files/test)
```

### Measurement Precision
- **Nanosecond Resolution**: Uses `CLOCK_MONOTONIC_RAW` for precise timing
- **CPU Cycle Conversion**: Automatic conversion to CPU cycles based on detected frequency
- **Direct Hardware Access**: Bypasses OS caching for accurate storage measurements

### Output Metrics
The benchmark reports:
- Average write latency (nanoseconds and CPU cycles)
- Minimum and maximum latency values
- Individual measurement data for statistical analysis
- CPU frequency detection and scaling information

## Usage Examples

### Basic Disk Latency Test
```bash
# Compile the benchmark
gcc -o lat_disk lat_disk.c

# Run with default parameters
./lat_disk
```

### Custom Configuration
```bash
# Test with 4KB blocks, 100 iterations, custom alignment
./lat_disk -s 4096 -a 4096 -i 100

# Test specific device/directory
./lat_disk -d /dev/sdb -s 1024 -i 50
```

### Automated Benchmark Suite
```bash
# Configure benchmark parameters
cp config.cfg.example config.cfg
# Edit config.cfg with required settings:
# - Set either WRITE_TO_LOCAL_DISK or WRITE_TO_DEVICE
# - Configure ITERATIONS, SIZE, MACHINE_NAME, etc.

# Run complete benchmark with automatic compilation and plotting
./bench_and_plot.sh
```

The automation script handles:
- **Configuration Validation**: Checks all required variables are set
- **Target Selection**: Validates either local file or device is specified (not both)
- **Directory Management**: Creates output directories as needed
- **Automatic Compilation**: Compiles `lat_disk.c` if binary doesn't exist
- **Safety Warnings**: Warns about potential data loss when writing to devices
- **Complete Pipeline**: Runs benchmark and generates plots automatically

## Technical Details

### CPU Frequency Analysis
The benchmark automatically detects CPU frequency by:
1. Reading `/sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq`
2. Taking multiple samples to detect frequency scaling
3. Using detected frequency for cycle-accurate measurements

### Memory Management
- **Aligned Allocation**: Uses `posix_memalign()` for optimal memory alignment
- **Direct I/O**: Ensures measurements reflect actual storage performance
- **Memory Initialization**: Pre-fills buffers to avoid lazy allocation effects

### Safety and Reliability
- **Error Handling**: Comprehensive error checking for all system calls
- **Resource Cleanup**: Proper cleanup of file descriptors and allocated memory
- **Validation**: Ensures write operations complete successfully

## Output Files

### Raw Data Files
- **`files/plot_ns`**: Individual latency measurements in nanoseconds
- **`files/plot_cycles`**: Individual latency measurements in CPU cycles
- **`files/test`**: Default test file for I/O operations

### Visualization
The plotting utilities generate graphs showing:
- Latency distribution histograms
- Time-series latency measurements
- Statistical analysis of performance variations

## Requirements

### System Requirements
- GCC compiler
- Write permissions for target device/directory
- Python 3.x (for plotting utilities)
- POSIX compliant system

### Hardware Considerations
- Direct I/O requires proper storage device support
- CPU frequency scaling may affect cycle measurements
- Memory alignment requirements vary by architecture

## Troubleshooting

### Common Issues
1. **Permission Errors**: Ensure write access to target device/directory
2. **Alignment Errors**: Block size must be multiple of alignment value
3. **Direct I/O Failures**: Some filesystems don't support `O_DIRECT`
4. **Frequency Detection**: May require root access for some systems

### Performance Considerations
- Use appropriate block sizes for target storage
- Consider filesystem overhead for file-based tests
- Multiple iterations improve statistical accuracy
