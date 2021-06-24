# Bandwidth required for remote clusters

This answers the following FAQ,

> I want to know the minimum bandwidth required between 2 remote ECX hosts in the same cluster.

Required bandwidth *W* (Mbps) is derived from the distance *D* (km) between the hosts

    W = 25000 / ( D + 370 )

In addition, the required time *T* (hours) for a recovery oprstsyion of *V* (GB) volume is derived from *V* and *W*

    T = 2.28 * V / W

For example, assuming **500 GB** volume and **50 km** distance, the cluster requires **59.5 Mbps** bandwidth and **19.2 hours** for recovery.

Using async replication would be required if RTT exceeds **20 msec**, making it difficult for sync replication to have enough performance as general purpose storage.

<!--
## Theory
-->

#### Assumptions
- **Synchronous** replication
- **64 KB** TCP Window Size and no TCP Window Scaling
- Using dark fiber network, **0.01 msec/km** speed of light in the dark fiber
- Network switches between hosts causes delay of **3.7 msec** for one-way trip.
- The volume recovery is **full-copy**

---

Note, this presents a way to estimate the logical upper bound, and no guarantee.