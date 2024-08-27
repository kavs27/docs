---
description: You can also use Portkey if you are doing custom agent orchestration!
---

# Bring Your own Agents

## Getting Started

### 1. Install the required packages:

```bash
!pip install portkey-ai openai
```

### **2.** Configure your  OpenAI object:

```python
client = OpenAI(
    api_key="OPENAI_API_KEY",
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="openai",
        api_key="PORTKEY_API_KEY",
        virtual_key="openai-latest-a4a53d"
    )
)
```

## Integrate Portkey with your custom Agents

This notebook demonstrates integrating a ReAct agent with Portkey

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://dub.sh/ReAct-agent)

## Make your agents Production-ready with Portkey

Portkey makes your agents reliable, robust, and production-grade with its observability suite and AI Gateway. Seamlessly integrate 200+ LLMs with your custom agents using Portkey. Implement fallbacks, gain granular insights into agent performance and costs, and continuously optimize your AI operations—all with just 2 lines of code.

Let's dive deep! Let's go through each of the use cases!

### 1. [Interoperability](../../product/ai-gateway/universal-api.md)

Easily switch between 200+ LLMs. Call various LLMs such as Anthropic, Gemini, Mistral, Azure OpenAI, Google Vertex AI,  AWS Bedrock, and many more by simply changing the  `provider` and `API key` in the `ChatOpenAI` object.

{% tabs %}
{% tab title="OpenAI to Azure OpenAI" %}
If you are using OpenAI with CrewAI, your code would look like this:

```python
client = OpenAI(
    api_key="OPENAI_API_KEY",
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="openai",
        api_key="PORTKEY_API_KEY",
    )
)
```

To switch to Azure as your provider, add your Azure details to Portley vault ([here's how](../llms/azure-openai.md)) and use Azure OpenAI using virtual keys

```python
client = OpenAI(
    api_key="API_KEY", #We will use Virtual Key in this
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="azure-openai", 
        api_key="PORTKEY_API_KEY",
        virtual_key="AZURE_VIRTUAL_KEY" #Azure Virtual key
    )
)
```
{% endtab %}

{% tab title="Anthropic to AWS Bedrock" %}
If you are using Anthropic with CrewAI, your code would look like this:

```python
client = OpenAI(
    api_key="ANTROPIC_API_KEY",
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="anthropic",
        api_key="PORTKEY_API_KEY",
    )
)
```

To switch to AWS Bedrock as your provider, add your AWS Bedrock details to Portley vault ([here's how](../llms/aws-bedrock.md)) and use AWS Bedrock using virtual keys,

```python
client = OpenAI(
    api_key="api_key", #We will use Virtual Key in this
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="bedrock",
        api_key="PORTKEY_API_KEY",
        virtual_key="AWS_VIRTUAL_KEY" #Bedrock Virtual Key
    )
)
```
{% endtab %}
{% endtabs %}

### 2. [Reliability](../../product/ai-gateway/)

Agents are _brittle_. Long agentic pipelines with multiple steps can fail at any stage, disrupting the entire process. Portkey solves this by offering built-in **fallbacks** between different LLMs or providers, **load-balancing** across multiple instances or API keys, and implementing automatic **retries** and request **timeouts**. This makes your agents more reliable and resilient.

Here's how you can implement these features using Portkey's config

```json
{
  "retry": {
    "attempts": 5
  },
  "strategy": {
    "mode": "loadbalance" // Choose between "loadbalance" or "fallback"
  },
  "targets": [
    {
      "provider": "openai",
      "api_key": "OpenAI_API_Key"
    },
    {
      "provider": "anthropic",
      "api_key": "Anthropic_API_Key"
    }
  ]
}
```

### 3. [Metrics](../../product/observability/)

Agent runs can be costly. Tracking agent metrics is crucial for understanding the performance and reliability of your AI agents. Metrics help identify issues, optimize runs, and ensure that your agents meet their intended goals.

Portkey automatically logs comprehensive metrics for your AI agents, including **cost**, **tokens used**, **latency**, etc. Whether you need a broad overview or granular insights into your agent runs, Portkey's customizable filters provide the metrics you need.\
For agent-specific observability, add `Trace-id` to the request headers for each agent.&#x20;

```python
llm2 = ChatOpenAI(
    api_key="Anthropic_API_Key",
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="PORTKEY_API_KEY",
        provider="anthropic",
        trace_id="research_agent1" #Add individual trace-id for your agent analytics
    )
)
```

### 4. [Logs](../../product/observability/logs.md)

Agent runs are complex. Logs are essential for diagnosing issues, understanding agent behavior, and improving performance. They provide a detailed record of agent activities and tool use, which is crucial for debugging and optimizing processes.

Portkey offers comprehensive logging features that capture detailed information about every action and decision made by your AI agents. Access a dedicated section to view records of agent executions, including parameters, outcomes, function calls, and errors. Filter logs based on multiple parameters such as trace ID, model, tokens used, and metadata.

<figure><img src="../../.gitbook/assets/222.gif" alt=""><figcaption></figcaption></figure>

### 5. [Continuous Improvement](../../product/observability/feedback.md)

Improve your Agent runs by capturing qualitative & quantitative user feedback on your requests.\
Portkey's Feedback APIs provide a simple way to get weighted feedback from customers on any request you served, at any stage in your app. You can capture this feedback on a request or conversation level and analyze it by adding meta data to the relevant request.

### 6. [Caching](../../product/ai-gateway/cache-simple-and-semantic.md)

Agent runs are time-consuming and expensive due to their complex pipelines. Caching can significantly reduce these costs by storing frequently used data and responses.\
Portkey offers a built-in caching system that stores past responses, reducing the need for agent calls saving both time and money.

```json
{
 "cache": {
    "mode": "semantic" // Choose between "simple" or "semantic"
 }
}
```

### 7.[ Security & Compliance](../../product/enterprise-offering/security-portkey.md)

Set budget limits on provider API keys and implement fine-grained user roles and permissions for both the app and the Portkey APIs.

***

## [Portkey Config](../../product/ai-gateway/configs.md)

Many of these features are driven by Portkey's Config architecture. The Portkey app simplifies creating, managing, and versioning your Configs.

For more information on using these features and setting up your Config, please refer to the [Portkey documentation](https://docs.portkey.ai).
