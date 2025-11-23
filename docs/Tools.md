Curated list of tools to interract with C-States, along with tutorials and results.

# `CPUPower`

`cpupower` is a very popular, native linux tool, and easy to interract with. It displays informations related to the power managment side of the CPU, includind C-States and P-States. `cpupower` interracts with the `/sys/` file system to get infos and to apply requests from the user. 

# `turbostat`

**`turbostat`** is a powerful command-line utility for Linux that provides deep, detailed insight into the low-level **CPU topology, frequency, power consumption, and idle statistics**. Unlike simpler tools, `turbostat` directly reads **Model Specific Registers (MSRs)** and **PCI configuration space** to gather highly accurate data. Its primary advantage is its ability to report hardware residency—the actual time the CPU cores and packages spend in specific C-states (idle states like C0, C1, C6, etc.)—and to show real-time power consumption metrics via the **RAPL (Running Average Power Limit)** interface (supported on both modern Intel and AMD Zen CPUs). This makes it indispensable for Linux users, developers, and power management engineers who need precise data to diagnose performance issues, verify BIOS settings, or optimize system power efficiency.

Thus, `turbostat` is way more precise than `cpupower`. 

# Ressources

