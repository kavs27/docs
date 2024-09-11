# SambaNova

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [SambaNova AI](https://sambanova.ai/).

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../product/ai-gateway/virtual-keys/) system.

{% hint style="info" %}
Provider Slug: **`sambanova`**
{% endhint %}

## Portkey SDK Integration with SambaNova Models

### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with SambaNova's API through Portkey's gateway.

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

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your SambaNova Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for SambaNova AI
)
```
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions**

Use the Portkey instance to send requests to the SambaNova API. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'Meta-Llama-3.1-405B-Instruct',
});

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'Meta-Llama-3.1-405B-Instruct'
)

print(completion)
```
{% endtab %}
{% endtabs %}

## Managing SambaNova Prompts

You can manage all prompts to SambaNova models in the [Prompt Library](../../product/prompt-library.md). All the current models of SambaNova are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

The complete list of supported models are available here:

{% embed url="https://community.sambanova.ai/t/supported-models/193" %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability/metadata.md)
2. [Add gateway configs to your SambaNova](../../product/ai-gateway/configs.md)[ requests](../../product/ai-gateway/configs.md)
3. [Tracing SambaNova requests](../../product/observability/traces.md)
4. [Setup a fallback from OpenAI to SambaNova APIs](../../product/ai-gateway/fallbacks.md)
