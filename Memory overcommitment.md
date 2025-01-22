[[OS]] [[VIRTUALIZATION]]

>[!info] Definition
>**Memory evercommitment** is a concept in computing that covers the *assigment of more memory to virtual computing devices* (or processes) *than the physical machine* they are hosted, or running on, actually has.

This is possible because *virtual machines* (or process) do not necessarily use as much memory at any one point as they are assigned, creating a buffer. 

If four virtual machines each have 1GB of memory on a physical machine with 4GB of memory, but those virtual machines are only using 500MB, it is possible to create additional virtual machines that take advantage of the 500MB each existing machine is leaving free.

Memory swapping is then used to handle spikes in memory usage.

The disadvantages of this approach is that **memory swap files** are slower to read from than 'actual' memory, which can lead to *performance drops*.
Another disadvantages is that, when running out of real memory, the *system is relying on the applications to **not use the additional memory** despite it being allocated to them*. Should a program do so anyway, it or another has to be killed in order to free up memory to prevent the system from freezing. The **OOM Killer** is what performs this task. 

