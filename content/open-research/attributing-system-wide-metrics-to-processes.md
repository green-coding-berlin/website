In our work we often encounter the fact that some metrics like **energy** or  **idle time** are only
available in system-wide granularity, but not per process or per container.

This especially poses the problem that you get unwanted data into the mix. An even more complex problem occurs
when you want to split the system-wide metric up and you have to select which splitting critieria to choose.

Let's show the problem on a concrete example: RAPL Package Energy.

This metric is exposed through a MSR of the CPU and can be read through for example `perf`.

This value by source is only available per core or per package. Since the processor does not inspect its own
code and does typically not know when it switched to a different container or process.

[Scaphandre](https://github.com/hubblo-org/scaphandre) uses splitting of this system-wide metric by the time on the `procfs`.

Here the values `/proc/stat` and `/proc/PID/stat` are read specifically and then divided.

These values are in *Jiffies*. This value is to be understood as a *chunk of time*.

In the Linux system this was originally a tick of the fixed rate scheduler. 

Nowadays the linux kernel typically does not have fixed ticks anymore, but is rather a [tickless kernel](https://blog.fpmurphy.com/2012/09/tickless-linux-kernels.html).

The values in `/proc/stat` are still not changed to a real time value, but still have to be converted to reflect 
real wall time.
For example if you want to get the idle time in Miliseconds you would something like the following in C:
```C
// C Code
user_hz = sysconf(_SC_CLK_TCK);
long int time_in_ms = (idle_time*1000000)/user_hz;
```
=> Beware: The time can be greater than the real wall time, because if you have multiple cores than every core will attribute
linearly to the total idle time.

Another thing which is nice to know about modern tickless kernels: Since the time is still in Jiffies one might ask: What good 
is this value when it could be that no processor was doing any work?

The OS keeps at least one core periodically alive (through interrupts) to have periodic timekeeping jobs (like updating `/proc/stat`) alive.
The other cores can go to full sleep mode.

## Is timesplitting useful?
The question is now: Is time a good splitting critieria to get the share of a process of the whole system activity?

Splitting by time answers the research questions: What amount of possible time that has been elapsed in the system (System time)
was the processor working on my processor and how much of the time was he working on something else or sleeping?

Typically you want to answer a different question though which is: How much of the possible work that the processor *could have done*
was attributed to my process and how much to something else?

In the light of Dynamic Voltage and Frequency Scaling and its implementations like TurboBoost or PowerNow! etc.
this can easily mean that your processor was running during a measurement interval for 20% in power mode, using 5x the normal amount of energy
and then 80% in sleep mode doing 0 instruction cycles. In total consuming 10J.
When we split by time however we would attribute 2J to our process and 8J to the system. Instead a more logical attribution would to fully
attribute the 10J to the process, cause the sleep did cost no Instruction Cycles.

Instuctions cycles have their own caveat though, cause the observed Instruction cycles might be necessary the ones that where relevant to our process as CPUs might execute different Instructions out of order: https://en.wikipedia.org/wiki/Hardware_performance_counter (IBS).


## Takeaway message:
You have to monitor for context switches and then either take a time probe or a Instruction counter probe.
Just relying on Instruction Counters OR on time without this signal can lead to skewed results.

TODO: How do we do that?