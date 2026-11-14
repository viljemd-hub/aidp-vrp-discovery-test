# From AI Tool Integration to Open Service Discovery and Verifiable Offers

*How can an AI agent discover independent providers, retrieve data from their own sources, and verify an offer — without requiring every provider to be individually integrated into the platform?*

**Authors:** Viljem Dvojmoč (CM / AI Docking Port) · Rouiada Abbas (Vacation Rental Protocol; operator of HemmaBo, its reference implementation)

---

## 1. A Problem That Can Be Tested

Today, a developer can connect an AI agent to a specific tool or API. The harder question is how the agent can discover *new* providers it was never wired up to, understand their capabilities, and determine whether an offer it receives is verifiable and current.

We are not claiming existing platforms cannot do this at all. We want to demonstrate a specific, reproducible gap that remains even when conventional integrations are already in place:

- **Discovery** — an agent cannot find an independent provider's own system unless someone has manually integrated it.
- **Verification** — once an agent does find an offer, it has no general way to confirm it actually came from the provider, is still current, and wasn't altered in transit by an intermediary.
- **Authorization boundary** — finding a verifiable offer is not the same as being allowed to act on it; an agent needs to know what it can safely do next (read, check availability, book).
- **Entry-point ordering** — publishing both a machine-readable manifest and ordinary prose documentation does not by itself establish which an agent will consult first. Whether explicit manifest-first guidance improves discovery, interaction count, token cost or error rate is a testable hypothesis. Prose carries descriptive information for humans and agents alike; only the signed offer carries verifiable price and availability, so prose cannot substitute for successful offer verification when presenting a price or availability as VRP-verified.

## 2. A Real-World Example

A user asks an AI agent:

> *"Find accommodation for 13–14 February 2027, two guests, booked directly with the host."*

Proposed workflow:

1. The agent identifies independent accommodation providers that might match.
2. It discovers a source describing each provider's capabilities (**AI Docking Port**).
3. It retrieves a signed offer and verifies its signature (**VRP**).
4. It checks current availability and pricing directly with the provider.
5. It presents the result and a direct booking link to the user.
6. The booking is completed through a system that actually supports that action.

This is a demonstration scenario. Steps 2–3 are independently live today on real production nodes (see below); the full chain end-to-end, across both projects at once, is the experiment we are proposing — not something already shipped.

## 3. What Already Exists

| | **AI Docking Port** (Viljem Dvojmoč) | **VRP** (Rouiada Abbas) |
|---|---|---|
| Starting point | A provider runs its own local booking system and wants to stay independent of centralized platforms. | An AI agent needs structured access to a provider's offer and a way to verify it. |
| Main question | How does an agent discover an independent provider and find the right data source? | How does an agent verify a signed offer, and distinguish that from checking live availability or completing a booking? |
| Live today | Reception Protocol node on real production (`apartmamatevz.si`): `.well-known/cm-ai-docking-port.json` + `manifest.json` discovery, stateless depth/fanout-bounded forwarding between nodes, geographic relevance gating, documented request/response envelopes and error codes, rate limiting. A JSON-RPC/A2A translator (`a2a_rpc.php`) with a signed AgentCard (Ed25519, detached JWS) and signed per-result responses. A second, non-production demo node for multi-node testing. | Open spec (CC0) with a reference implementation (HemmaBo); `villaakerlyckan.se` is a node running it. `apartmamatevz.si` is now an independent second signing node — its signed stay offers verify against its JWKS with both the spec's own tooling (vrp-verify 0.3.1) and a third-party open-source verifier ([Swarnabha753/vrp-independent-verifier](https://github.com/Swarnabha753/vrp-independent-verifier)), run by Rouiada Abbas on 2026-11-14. |
| Not yet connected | The Docking Port's discovery/routing layer does not itself verify offer authenticity — it points *at* a source, it does not vouch for the data. | VRP verifies that an offer is signed by the host's own domain, is current, and matches the stay asked for; it does not by itself solve how an agent finds a provider it has never seen before. |

Neither side needs the other to function on its own. The two pieces answer two different, adjacent parts of the same larger question, and can be adopted independently or together.

VRP also has a proposed experimental extension, **peers**, in which a node lists the domains of other VRP nodes it knows and the client walks them itself; AI Docking Port forwards requests between nodes on the agent's behalf. Different mechanisms, compatible.

## 4. What We Propose Testing

A small, bounded, reproducible experiment — not a request to adopt a platform:

- One AI agent (any framework/tool-use setup willing to try it).
- Two or three independent provider nodes (both of our live nodes qualify; a third volunteer node would strengthen the result).
- One discovery mechanism (AI Docking Port manifest + Reception forwarding).
- One offer-verification mechanism (VRP signed offer + JWKS).
- One end-to-end, reproducible trace: agent → discovers provider → retrieves signed offer → verifies it → checks live availability → gets a direct booking link.
- Whether signaling "consult the machine-readable entry point first; prose carries descriptive information, the signed offer carries the verifiable facts" measurably reduces interaction count, token cost and error rate compared to no such signal.

Everything needed to reproduce this is already public: both nodes' discovery documents, both specs, and working code.

## 5. Questions for You

1. Is this a meaningful gap in current agent architectures, or is it already adequately addressed by existing mechanisms you've built?
2. Where would a mechanism like this best fit into your existing stack — at the agent-framework layer, the MCP/tool layer, or the platform-integration layer?
3. Would a small, reproducible proof of concept (as described above) be worth evaluating together?

Our goal right now is to validate the problem and the proposed approach with people who actually build this infrastructure, before making any broader claim. If it's relevant to your work, we're happy to share full technical documentation and run a live demonstration against our real nodes.

## Links

- VRP spec: https://vacationrentalprotocol.com/spec/v0.1
- Verify apartmamatevz.si as a VRP node: https://vacationrentalprotocol.com/verify?host=apartmamatevz.si
- AI Docking Port manifest: https://apartmamatevz.si/.well-known/cm-ai-docking-port.json
- AI Docking Port background: https://apartmamatevz.si/journal/posts/2026-10-07-cm-ai-docking-port-from-experiment-to-protocol-for-connected-systems.html

— Viljem Dvojmoč (CM AI Docking Port) & Rouiada Abbas (Vacation Rental Protocol; operator of HemmaBo, its reference implementation)
