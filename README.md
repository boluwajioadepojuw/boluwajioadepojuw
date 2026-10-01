# Boluwaji Oluwaseyi Adepoju

Cybersecurity student building a home SOC end to end. The six repositories
below are one system: the same telemetry flows from ingestion to detection
to triage to a written report.

## The stack

| Repo | What it is | CI |
| --- | --- | --- |
| [SOCAtelier](https://github.com/boluwajioadepojuw/SOCAtelier) | Home SOC lab: Elastic + Suricata + Sysmon, Lynx console (FastAPI + React), 97 detection rules, 14 investigation reports | [![CI](https://github.com/boluwajioadepojuw/SOCAtelier/actions/workflows/ci.yml/badge.svg)](https://github.com/boluwajioadepojuw/SOCAtelier/actions/workflows/ci.yml) |
| [SigScope](https://github.com/boluwajioadepojuw/SigScope) | Sigma-to-ATT&CK coverage gate that fails CI when coverage drops; Navigator layer export | [![CI](https://github.com/boluwajioadepojuw/SigScope/actions/workflows/ci.yml/badge.svg)](https://github.com/boluwajioadepojuw/SigScope/actions/workflows/ci.yml) |
| [SplunkHarbor](https://github.com/boluwajioadepojuw/SplunkHarbor) | Splunk lab: ingestion, index routing, five SPL detections as saved searches with webhook alerting | [![CI](https://github.com/boluwajioadepojuw/SplunkHarbor/actions/workflows/ci.yml/badge.svg)](https://github.com/boluwajioadepojuw/SplunkHarbor/actions/workflows/ci.yml) |
| [IocVerdict](https://github.com/boluwajioadepojuw/IocVerdict) | IOC enrichment console: six free sources, 0-100 scoring, MITRE mapping | [![CI](https://github.com/boluwajioadepojuw/IocVerdict/actions/workflows/ci.yml/badge.svg)](https://github.com/boluwajioadepojuw/IocVerdict/actions/workflows/ci.yml) |
| [DomainSieve](https://github.com/boluwajioadepojuw/DomainSieve) | NRD feed to Suricata rules: brand impersonation domains become gateway blocks | [![CI](https://github.com/boluwajioadepojuw/DomainSieve/actions/workflows/ci.yml/badge.svg)](https://github.com/boluwajioadepojuw/DomainSieve/actions/workflows/ci.yml) |
| [ArpSieve](https://github.com/boluwajioadepojuw/ArpSieve) | ARP spoofing detector, live and pcap, with sample captures as test fixtures | [![CI](https://github.com/boluwajioadepojuw/ArpSieve/actions/workflows/ci.yml/badge.svg)](https://github.com/boluwajioadepojuw/ArpSieve/actions/workflows/ci.yml) |

## The L1 loop

```mermaid
flowchart LR
    A[SigScope<br/>Sigma coverage gate] -->|portable detections| B[SOCAtelier<br/>SOC lab + Lynx console]
    C[SplunkHarbor<br/>Splunk ingestion] -->|Windows telemetry| B
    B --> D[14 investigation reports]
    E[IocVerdict<br/>IOC enrichment] -->|verdict per indicator| D
    F[DomainSieve<br/>NRD to Suricata rules] -->|gateway blocks| B
    G[ArpSieve<br/>ARP poisoning detection] -->|network layer| B
```

Detect (SigScope rules, SOCAtelier profiles) -> ingest (SplunkHarbor,
Suricata, Sysmon) -> triage (Lynx console) -> enrich (IocVerdict,
DomainSieve, ArpSieve) -> respond (actions, osTicket) -> write up
(investigation reports).

## Working principles

- Every lab run is a controlled simulation; the Linux cases are live on
  the lab machine, the Windows cases are archived replays - each report
  says which.
- Deterministic where it matters: the console and the phishing analyzer
  work fully offline, no AI service required.
- Detection as code: CI gates rule coverage, lint and build the console,
  and validates the Splunk configs before anything merges.
