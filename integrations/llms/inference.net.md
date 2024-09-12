# Inference.net

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including the models hosted on [Inference.net](https://www.inference.net/).

{% hint style="info" %}
Provider slug: <mark style="color:blue;">**`inference-net`**</mark>
{% endhint %}

## Portkey SDK Integration with Inference.net

Portkey provides a consistent API to interact with models from various providers. To integrate Inference.net with Portkey:

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

### **2. Initialize Portkey with Inference.net Authorization**

* Set `provider` name as `inference-net`
* Pass your API key with `Authorization` header

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    provider: "inference-net",
    Authorization: "Bearer INFERENCE-NET API KEY"
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    provider="inference-net",
    Authorization="Bearer INFERENCE-NET API KEY"
)
```
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions**

{% tabs %}
{% tab title="NodeJS SDK" %}
<pre class="language-javascript"><code class="lang-javascript">const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
<strong>    model: 'llama3',
</strong>});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Python SDK" %}
<pre class="language-python"><code class="lang-python">completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
<strong>    model= 'llama3'
</strong>)

print(completion)
</code></pre>
{% endtab %}
{% endtabs %}

***

## Supported Models

Find more info about models supported by Inference.net here:

{% embed url="https://www.inference.net/" %}

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../product/observability/metadata.md)
2. [Add gateway configs to your Inference.net](../../product/ai-gateway/configs.md)[ requests](../../product/ai-gateway/configs.md)
3. [Tracing Inference.net requests](../../product/observability/traces.md)
4. [Setup a fallback from OpenAI to Inference.net](../../product/ai-gateway/fallbacks.md)
