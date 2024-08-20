# Langchain Agents

## Getting Started

### 1. Install the required packages:

```bash
pip install -qU llama-agents llama-index portkey-ai
```

### &#x20;2. Configure your Langchain LLM objects:

```python
from langchain_openai import ChatOpenAI, createHeaders
from portkey_ai import createHeaders, PORTKEY_GATEWAY_URL

llm1 = ChatOpenAI(
    api_key="OpenAI_API_Key",
     base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
    provider="openai", 
    api_key="PORTKEY_API_KEY"
    )
)
```

That's all you need to do to use Portkey with Llama Index agents. Execute your agents and visit [Portkey.ai](https://portkey.ai) to observe your Agent's activity.

## Integration Guide

Here's a simple Google Colab notebook that demonstrates Llama Index with Portkey integration

[![Google Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1ab\_XnSf-HR1KndEGBgXDW6RvONKoHdzL?usp=sharing)

{% embed url="https://colab.research.google.com/drive/1ab_XnSf-HR1KndEGBgXDW6RvONKoHdzL?usp=sharing" %}

## Make your agents Production-ready with Portkey

Portkey makes your Llama Index agents reliable, robust, and production-grade with its observability suite and AI Gateway. Seamlessly integrate 200+ LLMs with your Llama Index agents using Portkey. Implement fallbacks, gain granular insights into agent performance and costs, and continuously optimize your AI operations—all with just 2 lines of code.

Let's dive deep! Let's go through each of the use cases!

### 1. [Interoperability](../../product/ai-gateway-streamline-llm-integrations/universal-api.md)

Easily switch between 200+ LLMs. Call various LLMs such as Anthropic, Gemini, Mistral, Azure OpenAI, Google Vertex AI,  AWS Bedrock, and many more by simply changing the  `provider` and `API key` in the `ChatOpenAI` object.

{% tabs %}
{% tab title="OpenAI to Azure OpenAI" %}
If you are using OpenAI with CrewAI, your code would look like this:

```python
llm = ChatOpenAI(
    api_key="OpenAI_API_Key",
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="openai", #choose your provider
        api_key="PORTKEY_API_KEY"
    )
)
```

To switch to Azure as your provider, add your Azure details to Portley vault ([here's how](../integration-guides/azure-openai.md)) and use Azure OpenAI using virtual keys

```python
llm = ChatOpenAI(
    api_key="api-key",
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="azure-openai", #choose your provider
        api_key="PORTKEY_API_KEY",  
        virtual_key="AZURE_VIRTUAL_KEY"   # Replace with your virtual key for Azure
    )
)
```
{% endtab %}

{% tab title="Anthropic to AWS Bedrock" %}
If you are using Anthropic with CrewAI, your code would look like this:

```
llm = ChatOpenAI(
    api_key="OpenAI_API_Key",
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="openai", #choose your provider
        api_key="PORTKEY_API_KEY"
    )
)
```

To switch to AWS Bedrock as your provider, add your AWS Bedrock details to Portley vault ([here's how](../integration-guides/aws-bedrock.md)) and use AWS Bedrock using virtual keys,

```
llm = ChatOpenAI(
    api_key="api-key",
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="azure-openai", #choose your provider
        api_key="PORTKEY_API_KEY",  
        virtual_key="AZURE_VIRTUAL_KEY"   # Replace with your virtual key for Azure
    )
)
```
{% endtab %}
{% endtabs %}

### 2. [Reliability](../../product/ai-gateway-streamline-llm-integrations/)

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

### 3. [Metrics](../../product/observability-modern-monitoring-for-llms/)

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

### 4. [Logs](../../product/observability-modern-monitoring-for-llms/logs.md)

Agent runs are complex. Logs are essential for diagnosing issues, understanding agent behavior, and improving performance. They provide a detailed record of agent activities and tool use, which is crucial for debugging and optimizing processes.

Portkey offers comprehensive logging features that capture detailed information about every action and decision made by your AI agents. Access a dedicated section to view records of agent executions, including parameters, outcomes, function calls, and errors. Filter logs based on multiple parameters such as trace ID, model, tokens used, and metadata.

<figure><img src="../../.gitbook/assets/222.gif" alt=""><figcaption></figcaption></figure>

### 5. Traces

AI agents are becoming more complex, with the use of advanced abstractions like chains, agents with tools, and sophisticated prompts. Portkey's **Traces** feature provides invaluable insights agents run.

**What is a trace**?  A trace in Portkey represents a single agentic flow, capturing the complete execution path of an agent's run.

Key benefits of Portkey's traces for agents:

* Visualize the execution path of complex agent interactions
* Identify bottlenecks in your agent pipelines
* Understand the intricate flow of your agent's decision-making process

**Adding Metadata to traces:**&#x20;

Metadata in Portkey consists of custom key-value pairs attached to your `LangchainCallbackHandler`. It allows you to filter and analyze specific agent runs from your traces.

**Combining multiple agent runs**:

Consistent metadata can be used to encapsulate the complete execution of your workflow, uniting all agents, LLMs, actions, etc., within one framework. This allows you to visualize and analyze complex, multi-agent processes as a cohesive unit.

To use traces in Langchain agent you need to import `LangchainCallbackHandler`  from Portkey's Langchain library like this

```python
from portkey_ai.langchain import LangchainCallbackHandler

portkey_handler = LangchainCallbackHandler(
    api_key="YOUR_PORTKEY_API_KEY",
    metadata={
        "session_id": "session 1", # Use the same metadata across your application to combine tracking
        "agent_id": "research_agent_1", #specific to the agent
    }
)

config = {
    'callbacks': [portkey_handler]
}

llm = ChatOpenAI(
    api_key="YOUR_OPENAI_API_KEY_HERE",
    callbacks=[portkey_handler],
    # ... other parameters
)
```

### 6. Guardrails

LLMs are brittle - not just in API uptimes or their inexplicable `400`/`500` errors, but also in their core behavior. You can get a response with a `200` status code that completely errors out for your app's pipeline due to mismatched output. With Portkey's Guardrails, we now help you enforce LLM behavior in real-time with our _Guardrails on the Gateway_ pattern.

Using Portkey's Guardrail platform, you can now verify your LLM inputs AND outputs to be adhering to your specifed checks; and since Guardrails are built on top of our [Gateway](https://github.com/portkey-ai/gateway), you can orchestrate your request exactly the way you want - with actions ranging from _denying the request_, _logging the guardrail result_, _creating an evals dataset_, _falling back to another LLM or prompt_, _retrying the request_, and more.

\


### 7. [Continuous Improvement](../../product/observability-modern-monitoring-for-llms/feedback.md)

Improve your Agent runs by capturing qualitative & quantitative user feedback on your requests.\
Portkey's Feedback APIs provide a simple way to get weighted feedback from customers on any request you served, at any stage in your app. You can capture this feedback on a request or conversation level and analyze it by adding meta data to the relevant request.

### 8. [Caching](../../product/ai-gateway-streamline-llm-integrations/cache-simple-and-semantic.md)

Agent runs are time-consuming and expensive due to their complex pipelines. Caching can significantly reduce these costs by storing frequently used data and responses.\
Portkey offers a built-in caching system that stores past responses, reducing the need for agent calls saving both time and money.

```json
{
 "cache": {
    "mode": "semantic" // Choose between "simple" or "semantic"
 }
}
```

### 9. [Security & Compliance](../../product/enterprise-offering/security-portkey.md)

Set budget limits on provider API keys and implement fine-grained user roles and permissions for both the app and the Portkey APIs.

***

## [Portkey Config](../../product/ai-gateway-streamline-llm-integrations/configs.md)

Many of these features are driven by Portkey's Config architecture. The Portkey app simplifies creating, managing, and versioning your Configs.

For more information on using these features and setting up your Config, please refer to the [Portkey documentation](https://docs.portkey.ai).
