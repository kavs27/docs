# Huggingface

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including all the text generation models supported by [Huggingface's Inference endpoints](https://huggingface.co/docs/api-inference/index).

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway-streamline-llm-integrations/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`huggingface`**</mark>
{% endhint %}

## Portkey SDK Integration with Huggingface

Portkey provides a consistent API to interact with models from various providers. To integrate Huggingface with Portkey:

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Huggingface's API through Portkey's gateway.

{% tabs %}
{% tab title="NodeJS" %}
```bash
npm install --save portkey-ai
```
{% endtab %}

{% tab title="Python" %}
```bash
pip install portkey-ai
```
{% endtab %}
{% endtabs %}

### **2. Initialize Portkey with the Virtual Key**

To use Huggingface with Portkey, [get your Huggingface Access token from here](https://huggingface.co/settings/tokens), then add it to Portkey to create your Huggingface virtual key.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY", // Your Huggingface Access Token
    huggingfaceBaseUrl: "HUGGINGFACE_DEDICATED_URL" // Optional: Use this if you have a dedicated server hosted on Huggingface
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY",   # Replace with your virtual key for Huggingface
    huggingface_base_url="HUGGINGFACE_DEDICATED_URL" # Optional: Use this if you have a dedicated server hosted on Huggingface
)
```
{% endtab %}

{% tab title="OpenAI Python SDK" %}
<pre class="language-python"><code class="lang-python">from openai import OpenAI
<strong>from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
</strong>
client = OpenAI(
    api_key="HUGGINGFACE_ACCESS_TOKEN",
<strong>    base_url=PORTKEY_GATEWAY_URL,
</strong><strong>    default_headers=createHeaders(
</strong><strong>        api_key="PORTKEY_API_KEY",
</strong><strong>        provider="huggingface",
</strong><strong>        huggingface_base_url="HUGGINGFACE_DEDICATED_URL"
</strong><strong>    )
</strong>)
</code></pre>
{% endtab %}

{% tab title="OpenAI Node SDK" %}
<pre class="language-typescript"><code class="lang-typescript">import OpenAI from "openai";
<strong>import { PORTKEY_GATEWAY_URL, createHeaders } from "portkey-ai";
</strong>
const client = new OpenAI({
  apiKey: "HUGGINGFACE_ACCESS_TOKEN",
<strong>  baseURL: PORTKEY_GATEWAY_URL,
</strong><strong>  defaultHeaders: createHeaders({
</strong><strong>    provider: "huggingface",
</strong><strong>    apiKey: "PORTKEY_API_KEY",
</strong><strong>    huggingfaceBaseUrl: "HUGGINGFACE_DEDICATED_URL"
</strong><strong>  }),
</strong>});
</code></pre>
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions with Huggingface**

Use the Portkey instance to send requests to Huggingface. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'meta-llama/Meta-Llama-3.1-8B-Instruct', // make sure your model is hot
});

console.log(chatCompletion.choices[0].message.content);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
chat_completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'meta-llama/meta-llama-3.1-8b-instruct', // make sure your model is hot
)
    
print(chat_completion.choices[0].message.content)
```
{% endtab %}

{% tab title="OpenAI Python SDK" %}
```python
chat_completion = client.chat.completions.create(
    messages = [{ "role": 'user', "content": 'Say this is a test' }],
    model = 'meta-llama/meta-llama-3.1-8b-instruct', // make sure your model is hot
)

print(chat_completion.choices[0].message.content)
```
{% endtab %}

{% tab title="OpenAI Node SDK" %}
```typescript
async function main() {
    const chatCompletion = await client.chat.completions.create({
        model: "meta-llama/meta-llama-3.1-8b-instruct", // make sure your model is hot
        messages: [{ role: "user", content: "How many points to Gryffindor?" }],
    });
    console.log(chatCompletion.choices[0].message.content);
}

main();
```
{% endtab %}
{% endtabs %}

## Managing Huggingface Prompts

You can manage all prompts to Huggingface in the [Prompt Library](../../product/prompt-library.md). All the current models of Huggingface are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability-modern-monitoring-for-llms/metadata.md)
2. [Add gateway configs to your Huggingface requests](../../product/ai-gateway-streamline-llm-integrations/configs.md)
3. [Tracing Huggingface requests](../../product/observability-modern-monitoring-for-llms/traces.md)
4. [Setup a fallback from OpenAI to Huggingface's APIs](../../product/ai-gateway-streamline-llm-integrations/fallbacks.md)
