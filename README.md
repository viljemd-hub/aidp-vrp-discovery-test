# AIDP + VRP: Open Service Discovery and Verifiable Offers

A joint proposal between **AI Docking Port (AIDP)** (Viljem Dvojmoč) and **VRP — Vacation Rental Protocol** (Rouiada Abbas, HemmaBo). See [`PROPOSAL.md`](PROPOSAL.md).

## What we're proposing

Two adjacent, independently-built answers to the same gap — an AI agent can discover an independent accommodation provider it has never seen before (AIDP), and can verify a signed offer it receives from one (VRP) — presented as one problem with two connected pieces, not two projects asking to be adopted. Neither needs the other to work on its own; they're being proposed together because an agent needs both discovery and verification to act safely.

## Background: why agent discoverability isn't the same as search visibility

A short exchange (Viljem ↔ Duck.ai/DuckDuckGo AI, 2026-11-02) surfaced the distinction this proposal is built on:

> A manifest existing doesn't mean an agent will check for it unprompted. A search engine can find a page by its text or links; an agent additionally needs a way to know it should look for a machine-readable entry point at all - and in what order, relative to ordinary prose.
>
> A good test is the "cold start" test: ask an agent about a provider with no URLs given, and see whether it finds the machine-readable layer on its own. If it only succeeds after being shown the manifest or a well-known URL directly, the protocol is readable, but not yet genuinely discoverable.

This is the gap §1 of `PROPOSAL.md` names as "entry-point ordering."

## Status

Draft proposal, open for technical feedback. Not yet claiming adoption - see `PROPOSAL.md` §5 for the specific questions we're asking.
