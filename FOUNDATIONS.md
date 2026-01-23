# Foundations of Financial Engineering, Algorithmic Trading & HFT

This repository is a curated, practitioner-oriented reference covering the core
technology, financial domain knowledge, crypto/DeFi foundations, and formal academic
programs required for serious work in financial engineering, programmatic trading,
market making, and high-frequency trading (HFT).

The emphasis is deliberately placed on systems, market microstructure, execution,
and risk, rather than purely academic abstractions.

---

## 1. Top 10 – Technology (Low-Latency Systems, Networking & Infrastructure)

This section covers the non-negotiable engineering foundations for building real-world
trading systems: CPU, memory, concurrency, networking, and messaging.  
In HFT and market making, these layers are inseparable.

1. [What Every Programmer Should Know About Memory – Ulrich Drepper](https://people.freebsd.org/~lstewart/articles/cpumemory.pdf): CPU caches, NUMA, memory ordering
2. [Systems Performance – Brendan Gregg](https://www.brendangregg.com/systems-performance-2nd-edition-book.html): observability, profiling, tail latency, kernel behavior
3. [Optimizing Software in C++ – Agner Fog](https://www.agner.org/optimize/optimizing_cpp.pdf): micro-architecture-aware optimization
4. [UNIX Network Programming – W. Richard Stevens](https://www.pearson.com/en-us/subject-catalog/p/unix-network-programming-volume-1/P200000003295): authoritative TCP/UDP knowledge for systems programming
5. [TCP/IP Illustrated, Volume 1 – W. Richard Stevens](https://www.pearson.com/en-us/subject-catalog/p/tcp-ip-illustrated-volume-1/P200000003305): protocol-level understanding of TCP, UDP, retransmissions, and ACKs
6. [Mechanical Sympathy – Martin Thompson](https://mechanical-sympathy.blogspot.com/): latency, queues, memory barriers, and systems-level thinking
7. [Design Patterns for Low-Latency Applications (HFT)](https://arxiv.org/abs/2309.04259): architecture patterns specific to high-performance trading systems
8. [Aeron (Low-Latency Messaging over UDP)](https://aeron.io/): practical example of UDP-based, loss-aware messaging
9. [FIX Trading Community (FIX / FAST / SBE)](https://www.fixtrading.org/): industry-standard protocols for order entry and market data
10. [Linux Performance & Networking Tools – Brendan Gregg](https://www.brendangregg.com/linuxperf.html): CPU, memory, disk, and network analysis

### Notes on Networking in Trading Systems

- UDP multicast dominates market data distribution; applications must handle loss.
- TCP remains common for order entry but requires careful tuning.
- Kernel behavior (IRQ affinity, socket buffers, busy polling) directly impacts latency and PnL.
- Networking issues often surface as apparent strategy problems.

> In trading systems, networking is part of the execution logic, not just infrastructure.

### Notes on Operating Systems for Trading Systems

Operating system behavior is a primary determinant of latency, jitter, and
determinism in trading systems. Many production issues attributed to strategies
or networking are, in practice, consequences of OS scheduling and memory
management decisions.

- Scheduler behavior directly impacts tail latency; throughput-oriented defaults are often unsuitable for low-latency workloads.
- Preemption, context switches, and involuntary scheduling introduce nondeterminism that is difficult to observe from user space.
- Syscalls and user/kernel boundary crossings are hidden latency sources.
- Page faults, transparent huge pages, and memory reclamation can cause unpredictable stalls.
- Timer resolution, clock sources, and TSC stability affect timestamping and replay accuracy.
- Interrupt handling (IRQs) and CPU affinity must be explicitly managed.

> In trading systems, Linux defaults optimize for fairness and throughput, not determinism.

### Notes on Cloud vs Colocation Infrastructure

Modern trading systems are typically deployed across a mix of cloud and
colocation environments. Understanding the trade-offs between these deployment
models is essential for correct system design.

- Cloud environments prioritize elasticity and operational simplicity, often at the cost of higher and more variable latency.
- Virtualization layers introduce jitter through noisy neighbors, shared resources, and opaque scheduling.
- Colocation provides predictable latency and physical proximity to exchanges, critical for market data ingestion and order execution.
- Hybrid architectures are common: research, simulation, and control planes in the cloud; execution and market data in colo.
- Crypto trading systems often tolerate higher latency but must account for failure modes, such as chain reorgs and RPC instability.
- Operational risk and latency risk must be traded off explicitly.

> In trading systems, infrastructure choices encode assumptions about acceptable latency, failure modes, and risk.

---

## 2. Rust – Core Knowledge for Trading Systems

Rust is increasingly used in performance-critical, safety-sensitive trading
infrastructure, particularly for market data, replay engines, execution gateways,
and crypto/DeFi systems.

### Core Rust (Must-Know)

1. [The Rust Programming Language (The Book)](https://doc.rust-lang.org/book/)
2. [Rustonomicon (Unsafe Rust, Memory Layout, Undefined Behavior)](https://doc.rust-lang.org/nomicon/)
3. [Rust Reference](https://doc.rust-lang.org/reference/)
4. [Rust Performance Book](https://nnethercote.github.io/perf-book/)
5. [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)
6. [Rust Atomics and Locks (Low-Level Concurrency in Practice)](https://marabos.nl/atomics/)

---

## 3. Top 10 – Financial Domain (Markets, Microstructure, Strategies)

1. [Options, Futures, and Other Derivatives – John Hull](https://www.pearson.com/en-us/subject-catalog/p/options-futures-and-other-derivatives/P200000003295)
2. [Algorithmic and High-Frequency Trading – Cartea, Jaimungal, Penalva](https://www.cambridge.org/core/books/algorithmic-and-highfrequency-trading/)
3. [Trading and Exchanges – Larry Harris](https://global.oup.com/academic/product/trading-and-exchanges-9780195144703)
4. [Market Microstructure Theory – Maureen O’Hara](https://www.wiley.com/en-us/Market+Microstructure+Theory-p-9780631225294)
5. [Advances in Financial Machine Learning – Marcos López de Prado](https://www.wiley.com/en-us/Advances+in+Financial+Machine+Learning-p-9781119482086)
6. [Trading Systems and Methods – Perry Kaufman](https://www.wiley.com/en-us/Trading+Systems+and+Methods%2C+5th+Edition-p-9781119604075)
7. [Quantitative Risk Management – McNeil, Frey, Embrechts](https://press.princeton.edu/books/hardcover/9780691141282/quantitative-risk-management)
8. [The Science of Algorithmic Trading and Portfolio Management – Robert Kissell](https://www.elsevier.com/books/the-science-of-algorithmic-trading-and-portfolio-management/kissell/9780124016896)
9. [Statistical Models and Methods for Financial Markets – Tze Leung Lai](https://www.springer.com/gp/book/9780387227289)
10. [Flash Boys – Michael Lewis](https://wwnorton.com/books/Flash-Boys/)  
    Context and history.

---

## 4. Top 10 – Crypto, DeFi & Digital Markets

1. [Mastering Bitcoin – Andreas Antonopoulos](https://github.com/bitcoinbook/bitcoinbook)
2. [Mastering Ethereum – Antonopoulos & Gavin Wood](https://github.com/ethereumbook/ethereumbook)
3. [Cryptoassets – Burniske & Tatar](https://www.penguinrandomhouse.com/books/553457/cryptoassets-by-chris-burniske-and-jack-tatar/)
4. [The Age of Cryptocurrency – Vigna & Casey](https://www.penguinrandomhouse.com/books/239206/the-age-of-cryptocurrency-by-paul-vigna-and-michael-j-casey/)
5. [Uniswap v3 Whitepaper](https://uniswap.org/whitepaper-v3.pdf)
6. [Automated Market Making and Loss-Versus-Rebalancing – Milionis et al.](https://arxiv.org/abs/2108.08999)
7. [Flashbots Research](https://research.flashbots.net/)
8. [EigenPhi Research](https://eigenphi.io/research)
9. [Token Economy / Tokenomics – Sean Au & Thomas Power](https://tokenomicsbook.com/)
10. [Paradigm Research](https://www.paradigm.xyz/research)

---

## 5. Top Academic Programs – Financial Engineering & Quant Finance

1. [EPFL – Master in Financial Engineering (MFE)](https://www.epfl.ch/education/master/programs/financial-engineering/)
2. [Imperial College London – MSc Risk Management & Financial Engineering](https://www.imperial.ac.uk/business-school/masters/risk-management-financial-engineering/)
3. [Imperial College London – MSc Mathematics & Finance](https://www.imperial.ac.uk/study/courses/postgraduate-taught/mathematics-finance/)
4. [National University of Singapore – MSc Financial Engineering](https://msfe.nus.edu.sg/)
5. [Nanyang Technological University – MSc Financial Engineering](https://www.ntu.edu.sg/education/graduate-programme/master-of-science-in-financial-engineering)
6. [Singapore Management University – MSc Quantitative Finance](https://masters.smu.edu.sg/programme/msc-in-quantitative-finance)
7. [WorldQuant University – MSc Financial Engineering](https://www.wqu.edu/)
8. [Columbia University – MS Financial Engineering](https://ieor.columbia.edu/financial-engineering)
9. [Carnegie Mellon – MS Computational Finance](https://www.cmu.edu/mscf/)
10. [UC Berkeley – Master of Financial Engineering](https://mfe.haas.berkeley.edu/)

### Rankings and Meta-Resources

- [QuantNet MFE Rankings](https://quantnet.com/mfe-programs-rankings/)
