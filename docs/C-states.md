**C-states** provide the ability to request that the CPU enters a low-power state y turning off some pieces of logic or even cores. 

# Packages in a CPU

First, it is important to understand what are the pieces of a CPU. We can split the whole CPU (as we name it usually) into 4 differents levels : 

1. *core* level
2. *uncore* level
3. *package* level
4. *dram* level

The levels can be illustrated in here :

![cpu levels](./medias/img/packages.svg)

## *Core* level

First, the *core* refers to only the cores, as they might be multiples. It excludes the GPU integrated unit (if concerned) but also all **shared caches*. Nowadays, we have CPUs with at leats 4 cores so in general, L1 cache is exclusive to a core but L2 and L3 are shared. 

Thus, the *core* level is composed of a core, and its exclusive memory cache.

## *Uncore* level

The *Uncore* level refers to the integrated GPU. In many (expecially labtop one) CPUs, an integrated Graphic Processing Unit is included for rendering graphics. 

So this level can be empty if the CPU is only made of CPU cores.

## *Package* level

The *package* level refers to all *core* levels, the integrated GPU and all caches. It only excludes the DRAM.

## *DRAM* level

This level is refering to only the *Dynamic Random Access Memory*. We will probably not use this very much. 

# Ressources

- [official Intel documentation of CPU power in 12th generation model](https://edc.intel.com/content/www/us/en/design/ipla/software-development-platforms/client/platforms/alder-lake-desktop/12th-generation-intel-core-processors-datasheet-volume-1-of-2/011/power-management/)
- [CPU Power Management book](https://link.springer.com/chapter/10.1007/978-1-4302-6638-9_2#Sec25)
- [Minimum tutorial of cpu power management](https://metebalci.com/blog/a-minimum-complete-tutorial-of-cpu-power-management-c-states-and-p-states/)