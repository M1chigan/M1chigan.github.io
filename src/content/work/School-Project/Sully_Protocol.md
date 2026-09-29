---
title: Sully Protocol - AI Marketplace
publishDate: 2026-08-31
img: /assets/portrait.jpg
img_alt: Sully Protocol Architecture
description: |
  A decentralized AI marketplace and nested multi-agent infrastructure developed during an 8-week internship at GIST, South Korea .
tags:
  - Blockchain
  - Artificial Intelligence
  - Python
  - Solidity
---

## Project Overview

Developed at the Gwangju Institute of Science and Technology (GIST) within the INFONET laboratory, the Sully Protocol aims to create a trust-minimized inter-agent economy. As custom AIs become more democratized through frameworks like OpenClaw and Hermes, a standardized collaboration bottleneck emerged. This project provides a decentralized infrastructure enabling autonomous machine-to-machine task delegation and financial settlement.

### Key Components

The architecture is divided into several on-chain and off-chain elements:

- **On-chain Marketplace:** A smart contract deployed on the Sepolia testnet handling USDC settlements, featuring a "burn fee" and rating system to ensure mutually assured destruction (MAD) against malicious behaviors.
- **Bookkeeper Service:** A Python-based off-chain indexer powered by The Graph, responsible for sub-second discovery, multi-criteria matching, and agent push notifications.
- **Sully Agent & Orchestrator:** The Sully Agent acts as a Web3 gateway and enforces rules, while the Orchestrator manages task decomposition among specialized sub-agents using a Progressive Disclosure prompt routing method.
- **Setup Application:** A cross-platform GUI wizard built in Python to ensure protocol accessibility and easy workspace injection.

### Outcomes and Learnings

This research internship resulted in a validated hybrid Proof of Concept (PoC) demonstrating end-to-end machine-to-machine workflows. The entire protocol is open-source and available on GitHub. Through this project, significant skills were acquired in smart contract security, multi-agent AI frameworks, and distributed storage systems like IPFS.
