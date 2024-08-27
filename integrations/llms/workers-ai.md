# Workers AI

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [Workers AI.](https://developers.cloudflare.com/workers-ai/)

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**workers-ai**</mark>
{% endhint %}

## Portkey SDK Integration with Workers AI Models

Portkey provides a consistent API to interact with models from various providers. To integrate Workers AI with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Workers AI's API through Portkey's gateway.

{% tabs %}
{% tab title="NodeJS" %}
```javascript
npm install --save portkey-ai
```
{% endtab %}

{% tab title="Python" %}
```python
pip install portkey-ai
```
{% endtab %}
{% endtabs %}

### **2. Initialize Portkey with the Virtual Key**

To use Workers AI with Portkey, [get your API key from here](https://console.groq.com/keys), then add it to Portkey to create the virtual key.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your Workers AI Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for Groq
)
```
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions with** Workers AI

Use the Portkey instance to send requests to Workers AI. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'llama-3.1-8b-instruct-awq',
});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'mistral-medium'
)

print(completion)
```
{% endtab %}
{% endtabs %}

## Managing Workers AI Prompts

You can manage all prompts to Workers AI in the [Prompt Library](../../product/prompt-library.md). All the current models of Workers AI are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability/metadata.md)
2. [Add gateway configs to your Workers requests](../../product/ai-gateway/configs.md)
3. [Tracing Workers AI requests](../../product/observability/traces.md)
4. [Setup a fallback from OpenAI to Workers AI APIs](../../product/ai-gateway/fallbacks.md)

| [api.ts](https://github.com/Portkey-AI/gateway/blob/main/src/providers/workers-ai/api.ts)                   | [feat: add support for embeddings api](https://github.com/Portkey-AI/gateway/commit/26ff9105f4322b2b691c1f89c031128f50c1b8b7) | 4 months ago |
| ----------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------ |
| [chatComplete.ts](https://github.com/Portkey-AI/gateway/blob/main/src/providers/workers-ai/chatComplete.ts) | [fix: prettier](https://github.com/Portkey-AI/gateway/commit/6aaa717954bd0b19cd820a6a2026a49011bb3230)                        | 4 months ago |
| [complete.ts](https://github.com/Portkey-AI/gateway/blob/main/src/providers/workers-ai/complete.ts)         | [fix: prettier](https://github.com/Portkey-AI/gateway/commit/6aaa717954bd0b19cd820a6a2026a49011bb3230)                        | 4 months ago |
| [embed.ts](https://github.com/Portkey-AI/gateway/blob/main/src/providers/workers-ai/embed.ts)               | [fix: prettier](https://github.com/Portkey-AI/gateway/commit/6aaa717954bd0b19cd820a6a2026a49011bb3230)                        | 4 months ago |
| [index.ts](https://github.com/Portkey-AI/gateway/blob/main/src/providers/workers-ai/index.ts)               |                                                                                                                               |              |
