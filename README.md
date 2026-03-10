# ⚡ AgentPay (Python SDK)

*The Stripe for AI Agents.* AgentPay is an open-source framework that allows autonomous AI agents (LangChain, CrewAI, OpenClaw) to securely execute fiat transactions over the Indian UPI network using zero-click delegated mandates.

---

## 🛑 The Problem

AI agents can write code, browse the web, and book flights, but they are paralyzed at the checkout screen. Existing solutions force developers to use crypto (USDC) or require human OTP intervention for every single ₹100 transaction.

## 🟢 The Solution

AgentPay uses UPI AutoPay/Circle. The human user authorizes a strict mathematical spending limit (e.g., ₹2,000/month) once. After that, your AI agent can route payments autonomously without ever asking for a PIN again.

---

## 📦 Installation

```bash
pip install agentpay-india
```

---

## 🚀 Quickstart: The "Aha!" Moment

Give your AI the ability to buy things in 3 lines of code.

```python
from agentpay import AgentPayClient
from agentpay.tools import LangchainPayTool

# 1. Initialize the client with your Developer API Key
client = AgentPayClient(api_key="sk_live_12345")

# 2. Check if the user has authorized a spending limit for this agent
allowance = client.get_allowance(user_id="ankit_99")

if allowance.remaining > 500:
    # 3. The Agent executes the payment autonomously (Zero-Click)
    transaction = client.charge(
        user_id="ankit_99",
        amount=500,
        currency="INR",
        merchant_vpa="zomato@hdfcbank",
        reason="Post-workout high-protein meal"
    )
    print(f"Success! Txn ID: {transaction.id}")
else:
    # Ping the user on WhatsApp to increase their AI allowance
    client.request_allowance_increase(user_id="ankit_99", amount=2000)
```

---

## 🛠️ Framework Integrations

AgentPay drops right into the AI frameworks you are already using.

### LangChain Integration

```python
from langchain.agents import initialize_agent
from agentpay.integrations.langchain import UPIPaymentTool

tools = [UPIPaymentTool(api_key="sk_live_12345")]
agent = initialize_agent(tools, llm, agent="zero-shot-react-description")

agent.run("Order a ₹300 cold brew from Third Wave Coffee using my UPI mandate.")
```

---

## 🔐 Security & Guardrails

If an AI hallucinates, it shouldn't drain a bank account. AgentPay is built with hardcoded financial firewalls:

* **Velocity Limits:** Restrict agents to a maximum of X transactions per day.
* **Hard Caps:** Transactions over a specific threshold (e.g., ₹2000) automatically fallback to a human-in-the-loop WhatsApp approval prompt.
* **Merchant Whitelisting:** Restrict the agent so it can only pay approved VPAs (e.g., Swiggy, Uber, MakeMyTrip).