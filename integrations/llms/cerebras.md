# Cerebras

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including the models hosted on [Cerebras Inference API](https://cerebras.ai/inference).

## Portkey SDK Integration with Cerebras

Portkey provides a consistent API to interact with models from various providers. To integrate Cerebras with Portkey:

### **1. Install the Portkey SDK**

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

### **2. Initialize Portkey with Cerebras**

* `Authorization`: Pass your Cerebras API key with the `Authorization` param.
* `custom_host`: Set the target Cerebras API URL to `https://api.cerebras.ai/v1`.
* `provider`: Since Cerebras follows the OpenAI schema, set the reference provider as `openai`.

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
<strong>    provider: "openai",
</strong><strong>    customHost: "https://api.cerebras.ai/v1",
</strong><strong>    Authorization: "Bearer CEREBRAS_API_KEY"
</strong>})
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key ="PORTKEY_API_KEY",  # Replace with your Portkey API key
<strong>    provider = "openai",
</strong><strong>    custom_host = "https://api.cerebras.ai/v1",
</strong><strong>    Authorization = "Bearer CEREBRAS_API_KEY"
</strong>)
</code></pre>
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions**

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
<strong>    model: 'llama3.1-8b',
</strong>});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
<strong>    model= 'llama3.1-8b'
</strong>)

print(completion)
</code></pre>
{% endtab %}
{% endtabs %}

***

## Supported Models

Cerebras currently supports `Llama-3.1-8B` and `Llama-3.1-70B`. You can find more info here:

{% embed url="https://inference-docs.cerebras.ai/introduction" %}

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability/metadata.md)
2. [Add gateway configs to your Cerebras](../../product/ai-gateway/configs.md)[ requests](../../product/ai-gateway/configs.md)
3. [Tracing Cerebras requests](../../product/observability/traces.md)
4. [Setup a fallback from OpenAI to Cerebras](../../product/ai-gateway/fallbacks.md)
