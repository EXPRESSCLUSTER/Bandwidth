# Bandwidth required for remote clusters

This answers the following FAQ,

> I want to know the minimum bandwidth required between 2 remote ECX hosts in the same cluster.
---
Note, this presents a way to estimate the logical upper bound, and no guarantee.

---

Required bandwidth *W* (Mbps) is derived from the distance *D* (km) between the hosts

    W = 25000 / ( D + 370 )

In addition, the required time *T* (hours) for a recovery operation of *V* (GB) volume is derived from *V* and *W*

    T = 2.28 * V / W

For example, assuming **500 GB** volume and **50 km** distance, the cluster requires **59.5 Mbps** bandwidth and **19.2 hours** for recovery.

Using async replication would be required if RTT exceeds **20 msec**, making it difficult for sync replication to have enough performance as general purpose storage.

<!--
## Theory
-->

### Assumptions

- **Synchronous** replication
- **64 KB** TCP Window Size and no TCP Window Scaling
- Using dark fiber network, **0.01 msec/km** speed of light in the dark fiber
- Network switches between hosts causes delay of **3.7 msec** for one-way trip.
- The volume recovery is **full-copy**

### References

- [Disk Performance Basics](https://wintelguy.com/2013/20130406_disk_perf.html)

	> Obviously, optimal combination of IO performance, response time, and queue length will depend on the specific system requirements and load. For a “general purpose” storage system reasonable response time is considered to be below 20 ms. 

- [Measuring Disk Latency with Windows Performance Monitor (Perfmon)](https://docs.microsoft.com/en-us/archive/blogs/askcore/measuring-disk-latency-with-windows-performance-monitor-perfmon)

	> You have probably heard general statements about what are acceptable disk latency measurements: “Less than 10 milliseconds is good and more than 20 milliseconds is bad”. Although these rules of thumb are used to simplify analysis, they do not apply in all cases and may lead to incorrect conclusions.

- Windows [TCP receive window autotuning](https://docs.microsoft.com/en-us/windows-server/networking/technologies/network-subsystem/net-sub-performance-tuning-nics#bkmk_tcp_params)

	> Some applications define the size of the TCP receive window. If the application does not define the receive window size, the link speed determines the size as follows:
	> 
	> - Less than 1 megabit per second (Mbps): 8 kilobytes (KB)
	> - 1 Mbps to 100 Mbps: 17 KB
	> - 100 Mbps to 10 gigabits per second (Gbps): 64 KB
	> - 10 Gbps or faster: 128 KB
	> 
	> For example, on a computer that has a 1-Gbps network adapter installed, the window size should be 64 KB.
