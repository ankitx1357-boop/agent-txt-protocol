# Agent.txt Protocol Specification (Draft v0.1)

## 1. Introduction
The `agent.txt` protocol is a standard for "Agent-to-Website" (A2B) commerce. It allows web servers to declare explicit pricing, access rules, and payment methods for autonomous AI Agents. 

Unlike `robots.txt`, which focuses on permission (Allow/Disallow), `agent.txt` focuses on **monetization** (Free/Paid).

## 2. File Location
The file must be hosted at the root of the domain:
`https://example.com/agent.txt`

## 3. Format
The file uses **YAML** syntax for human readability and easy parsing by LLMs.

---

### 4. The Structure (Template)

```yaml
# agent.txt - v0.1
version: "1.0"

# --- IDENTITY ---
# Defines who owns this data and verifies authenticity.
publisher:
  id: "did:web:example.com"   # Decentralized ID or Domain
  name: "Example Publisher Inc."
  contact: "admin@example.com"

# --- PAYMENT CONFIGURATION ---
# Defines how the Agent can pay for access.
payment:
  gateway_url: "[https://api.tollgate.io/v1/verify](https://api.tollgate.io/v1/verify)" # The verification endpoint
  accepted_networks: ["solana", "polygon"]
  accepted_tokens: ["USDC", "SOL", "MATIC"]
  merchant_wallet: "8xptShe34...zHQ9" # Publisher's Receiving Address

# --- ACCESS RULES ---
# Defines the cost for specific URL paths.
routes:
  # 1. Free Tier (Public Data)
  - path: "/"
    policy: "allow"
    cost: 0

  # 2. Public Content (Blogs, News) - Free read, but rate-limited
  - path: "/blog/*"
    policy: "allow"
    rate_limit: "100/hour"

  # 3. Premium Data (API endpoints, Archives) - Pay Per Request
  - path: "/api/v1/financial-data"
    policy: "paywall"
    cost: 0.005
    currency: "USDC"
    unit: "per_request"

  # 4. High Value Content - Pay Per Session
  - path: "/premium-report/*"
    policy: "paywall"
    cost: 1.00
    currency: "USDC"
    unit: "per_session"
