This page aims to explain and present all tools which are used by Linux to interract with the CPU. It will not deal with tools used by the user to control the performance states. 

# `CPUFreq` for fixing dynamic freqencies

`CPUFreq` was implemented into the **Linux Kernel** and is the tool used by Linux to control the frequencies of the cores. It is really interesting because it allows to fix dynamix values for the frequencies, not only pre fixed values. 

There are **6** `CPUFreq` governors in Linux :

1. Performance
2. Powersave
3. Userspace
4. Ondemand
5. Conservative
6. Shedutil

The actual governor from a core can be displayed with the following command :

```sh
# works at least on Fedora 23,24 and Ubuntu 24.04
cat /sys/devices/system/cpu/cpuX/cpufreq/scaling_governor
```

where `X` is the core of the CPU, or `*` if you want to see them all. 

> [!NOTE]
> It is not sure if all governors are available every time or for each Linux version. There is a file called `/sys/devices/system/cpu/cpu*/cpufreq/scaling_available_governors`, containing only `performance` and `powersaver` in **Ubuntu 24.04**, so maybe all governors are not implemented.

## Existing governors

### Performance  

Straightforward, it will set the frequency to the maximum within the borders of `scaling_min_freq` and `scaling_max_freq`.

### Powersave

This governor will set the frequency to the lowest within the same border as above. 

### Userspace

The CPUfreq governor *userspace* allows the user, or any userspace program running with UID `root`, to set the CPU to a specific frequency by making a sysfs file `scaling_setspeed` available in the CPU-device directory.

### Ondemand

This governor will set a frequency based on the current system load. The load estimations are triggered each period of time, and this period of time can be set. When the estimation is triggered, `cpufreq` checks the usage and set the frequency accordingly. 

This governor creates also some files in `Sysfs`(wich act like parameters or args) :

- `sampling_rate` : mesured in µs ($` 10^{-6}s `$). Interval of how often the kernel will look to the usage and *may* update the frequency
- `sampling_rate_min` : limit of above
- `up_thresold` : the thresold required to make the frequency up 
- `ignore_nice_load` (0 or 1): include the "nice" process to the CPU load or not [^1]
- `sampling_down_factor` : controls if the diminutions in frequency are made at the same rate as the up. If the value is 1 then it means the sampling rate is the same, otherwise it acts as a multiplier. 
- `powersave_bias` : percentage (times 10) devrease of the value that will be aimed after the determination of the frequency by `ondemand`

[^1]: The kernel's documentation is unclear about this point. It is said that *"the processes that are run with a 'nice' value will not count"*, but *niceness* is just the importance of the process and ranges from -20 to 20. In the documentation, it would probably means that a high value (maximum?) will be ignored

### Conservative

This governor acts much more like the `ondemand` by setting the frequency depending of the usage, however it will gracefully increase and decrease instead of jumping from max to min. 

It creates also 3 files in `Sysfs`, **in addition to the same as in `ondemand`**: 

- `freq_step` : percentage of the maximum frequence, which will be used as the step for any increase or decrease (default to `5%`)
- `down_thresold` : same as `up_thresold` in `ondemand` but works on the opposite direction (thresold for decrease)
- `sampling_down_factor` : same as in `ondemand` 

### schedutil

This governor is also based on the CPU usage but aims to be more precise and have better integration. 

## Creating governors

A really interesting feature of the governors is that you can create your own governors, and defining their behavior. It can be really useful for profiling high performance systems, in terms of energy or computing. 

A new governor must register itself with the `CPUfreq` core using `cpufreq_register_governor`. The struct `cpufreq_governor`, which has to be passed to that function, must contain the following values:

- `govenor->name` : A unique name for this governor
- `governor->owner` : .THIS_MODULE for the governor module (if appropriate)

plus a set of hooks to the functions implementing the governor's logic.

The CPUfreq governor may call the CPU processor driver using one of
these two functions:

```c
int cpufreq_driver_target(struct cpufreq_policy *policy,
                                 unsigned int target_freq,
                                 unsigned int relation);

int __cpufreq_driver_target(struct cpufreq_policy *policy,
                                   unsigned int target_freq,
                                   unsigned int relation);
```
>[!TIP] The difference between those 2 functions is subtile, but here is what the Kernel's documentation says :
> > What's the difference between these two functions? When your governor is
in a direct code path of a call to governor callbacks, like
governor->start(), the policy->rwsem is still held in the cpufreq core,
and there's no need to lock it again (in fact, this would cause a
deadlock). So use __cpufreq_driver_target only in these cases. In all
other cases (for example, when there's a "daemonized" function that
wakes up every second), use cpufreq_driver_target to take policy->rwsem
before the command is passed to the cpufreq driver.

# Ressources 

- [Documentation from Linux Kernel about `CPUFreq`](https://www.kernel.org/doc/Documentation/cpu-freq/governors.txt#:~:text=The%20CPUfreq%20governor%20%22userspace%22%20allows,in%20the%20CPU%2Ddevice%20directory.&text=The%20CPUfreq%20governor%20%22ondemand%22%20sets,on%20the%20current%20system%20load.)