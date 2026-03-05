# Foundations of Financial Engineering, Algorithmic Trading & HFT

This repository is a curated, practitioner-oriented reference covering the core
technology, financial domain knowledge, crypto/DeFi foundations, and formal academic
programs required for serious work in financial engineering, programmatic trading,
market making, and high-frequency trading (HFT).

The emphasis is deliberately placed on systems, market microstructure, execution,
and risk, rather than purely academic abstractions.

---

## 1. Top Technology (Low-Latency Systems, Networking & Infrastructure)

This section covers the non-negotiable engineering foundations for building real-world
trading systems: CPU, memory, concurrency, networking, and messaging.  
In HFT and market making, these layers are inseparable.

1. [What Every Programmer Should Know About Memory – Ulrich Drepper](https://people.freebsd.org/~lstewart/articles/cpumemory.pdf): CPU caches, NUMA, memory ordering
2. [Systems Performance – Brendan Gregg](https://www.brendangregg.com/systems-performance-2nd-edition-book.html): observability, profiling, tail latency, kernel behavior
3. [Optimizing Software in C++ – Agner Fog](https://www.agner.org/optimize/optimizing_cpp.pdf): micro-architecture-aware optimization
4. [UNIX Network Programming – W. Richard Stevens](https://www.amazon.com/UNIX-Network-Programming-Richard-Stevens/dp/0139498761): authoritative TCP/UDP knowledge for systems programming
5. [TCP/IP Illustrated – W. Richard Stevens](https://www.amazon.es/TCP-IP-Illustrated-Protocols-APC/dp/0201633469): protocol-level understanding of TCP, UDP, retransmissions, and ACKs
6. [Mechanical Sympathy – Martin Thompson](https://mechanical-sympathy.blogspot.com/): latency, queues, memory barriers, and systems-level thinking
7. [Design Patterns for Low-Latency Applications (HFT)](https://arxiv.org/abs/2309.04259): architecture patterns specific to high-performance trading systems
8. [Aeron (Low-Latency Messaging over UDP)](https://aeron.io/): practical example of UDP-based, loss-aware messaging
9. [FIX Trading Community (FIX / FAST / SBE)](https://www.fixtrading.org/): industry-standard protocols for order entry and market data
10. [Linux Performance & Networking Tools – Brendan Gregg](https://www.brendangregg.com/linuxperf.html): CPU, memory, disk, and network analysis
11. [Beej's Guide to Network Programming](https://beej.us/guide/bgnet/html/split/index.html)
12. [High Performance Browser Networking - Ilya Grigorik](https://hpbn.co/)

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
6. [Rust Atomics and Locks - Mara Bos](https://marabos.nl/atomics/)
7. [Rust for Rustaceans - Jon Gjengset](https://www.amazon.com/Rust-Rustaceans-Programming-Experienced-Developers-ebook/dp/B0957SWKBS)

---

## 3. Top Financial Domain (Markets, Microstructure, Strategies)

1. [Options, Futures, and Other Derivatives – John Hull](https://www.pearson.com/nl/en_NL/higher-education/subject-catalogue/finance/Options-Futures-and-Other-Derivatives-Hull.html)
2. [Algorithmic and High-Frequency Trading – Cartea, Jaimungal, Penalva](https://www.amazon.com/Algorithmic-High-Frequency-Trading-Mathematics-Finance/dp/1107091144)
3. [Trading and Exchanges – Larry Harris](https://www.amazon.com/Trading-Exchanges-Market-Microstructure-Practitioners/dp/0195144708)
4. [Market Microstructure Theory – Maureen O’Hara](https://www.amazon.com/Market-Microstructure-Theory-Maureen-OHara/dp/0631207619)
5. [Advances in Financial Machine Learning – Marcos López de Prado](https://www.amazon.com/Advances-Financial-Machine-Learning-Lopez/dp/1119482089)
6. [Trading Systems and Methods – Perry Kaufman](https://www.amazon.com/Trading-Systems-Methods-Wiley/dp/1119605350)
7. [Quantitative Risk Management – McNeil, Frey, Embrechts](https://www.amazon.com/Quantitative-Risk-Management-Techniques-Princeton/dp/0691166277)
8. [The Science of Algorithmic Trading and Portfolio Management – Robert Kissell](https://www.amazon.com/Science-Algorithmic-Trading-Portfolio-Management/dp/0124016898)
9. [Statistical Models and Methods for Financial Markets – Tze Leung Lai](https://www.amazon.com/Statistical-Methods-Financial-Springer-Statistics/dp/1441926682)
10. [Flash Boys – Michael Lewis](https://wwnorton.com/books/Flash-Boys/)  

---

## 4. Top Crypto, DeFi & Digital Markets

1. [Bitcoin: A Peer-to-Peer Electronic Cash System - Satoshi Nakamoto](https://bitcoin.org/bitcoin.pdf)
2. [Mastering Bitcoin – Andreas Antonopoulos](https://github.com/bitcoinbook/bitcoinbook)
3. [Learn me a Bitcoin - Greg Walker](https://learnmeabitcoin.com/)
4. [Mastering Ethereum – Antonopoulos & Gavin Wood](https://github.com/ethereumbook/ethereumbook)
5. [Cryptoassets – Burniske & Tatar](https://www.amazon.com/Cryptoassets-Innovative-Investors-Bitcoin-Beyond/dp/1260026671)
6. [The Age of Cryptocurrency – Vigna & Casey](https://www.amazon.com/Age-Cryptocurrency-Blockchain-Challenging-Economic/dp/1250081556)
7. [Uniswap v3 Whitepaper](https://uniswap.org/whitepaper-v3.pdf)
8. [Token Economy / Tokenomics – Sean Au & Thomas Power](https://www.amazon.com/Tokenomics-Crypto-Shift-Blockchains-Tokens-ebook/dp/B07CSP51B9)
9. [Flashbots Research](https://www.flashbots.net/research-database)
10. [EigenPhi Research](https://eigenphi.substack.com/s/defi-research)
11. [Paradigm Research](https://www.paradigm.xyz/)
12. [How Crypto Works Book](https://github.com/lawmaster10/howcryptoworksbook)

---

## 5. Top Academic Programs – Financial Engineering & Quant Finance

1. [Baruch Collegue - Master of Financial Engineering](https://mfe.baruch.cuny.edu/)
2. [Massachusetts Institute of Technology - Master of Finance](https://mitsloan.mit.edu/mfin/)
3. [EPFL – Master in Financial Engineering (MFE)](https://www.epfl.ch/education/master/programs/financial-engineering/)
4. [Imperial College London – MSc Risk Management & Financial Engineering](https://www.imperial.ac.uk/business-school/masters/risk-management/)
5. [Imperial College London – MSc Mathematics & Finance](https://www.imperial.ac.uk/study/courses/postgraduate-taught/mathematics-finance/)
6. [Imperial College London – MSc Financial Technology](https://www.imperial.ac.uk/business-school/masters/financial-technology/)
7. [Singapore Management University – MSc Quantitative Finance](https://masters.smu.edu.sg/programme/msc-in-quantitative-finance)
8. [Columbia University – MS Financial Engineering](https://ieor.columbia.edu/financial-engineering-msfe)
9. [Carnegie Mellon – MS Computational Finance](https://www.cmu.edu/mscf/)
10. [UC Berkeley – Master of Financial Engineering](https://mfe.haas.berkeley.edu/)
11. [Boston - MS in Mathematical Finance & Financial Technology](https://www.bu.edu/academics/questrom/programs/mathematical-finance/ms/)
12. [Hesperides - Master en Finanzas Cuantitativas y Métodos Computacionales](https://hesperides.edu.es/estudios/master-en-finanzas-cuantitativas-y-metodos-computacionales/)

### Rankings and Meta-Resources

- [QuantNet MFE Rankings](https://quantnet.com/mfe-programs-rankings/)
