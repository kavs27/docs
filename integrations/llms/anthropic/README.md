# Anthropic

Anthropic develops large language models in the Claude family, capable of performing a wide range of natural language processing tasks. These models can generate text, engage in conversations, and assist with various language-related applications. Integrating Anthropic's Claude APIs through Portkey provides developers with additional management features to enhance their use of these AI models.

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [Anthropic's Claude APIs](https://docs.anthropic.com/claude/reference/getting-started-with-the-api).

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../../product/ai-gateway/virtual-keys/) system.

This integration is suitable for developers working on applications that require advanced natural language processing capabilities, such as conversational AI, content generation, and text analysis systems.

Key Information:</br>
• Provider: [Anthropic official website](https://www.anthropic.com) </br>
• Models: Claude family of language models</br>
• Performance: Current metrics available on Portkey dashboards</br>
• Developer Resources: Refer to [Anthropic's docs](https://docs.anthropic.com/en/docs/welcome)

The Anthropic-Portkey integration combines Anthropic's language models with Portkey's management features to streamline AI implementation across various applications.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`anthropic`**</mark>
{% endhint %}

* Supported Claude Models:</br>
 • Claude 3.5 Opus </br>
 • Claude 3.5 Sonnet</br>
 • Claude 3.5 Haiku – (Coming later this year)</br>
 • Claude 3 Opus</br>
 • Claude 3 Sonnet</br>
 • Claude 3 Haiku</br>
 • Claude 2.1</br>
 • Claude 2.0</br>
 • Claude Instant 1.2</br>

* Supported features:</br>
 • Chats</br>
 • Chat-Vision </br>
 • Prompt caching </br> 

* Unsupported features:</br>
 • Embeddings</br>
 • Images</br>
 • Tool use </br>

## Portkey SDK Integration with Anthropic

Integrating Anthropic’s Claude models with Portkey allows for flexible, scalable API usage. Below are methods to make API calls and leverage Portkey's advanced features:

Simple API Call using Auth Headers
### **1. Install the Portkey SDK**

Add the Portkey SDK to your application to interact with Anthropic's API through Portkey's gateway.

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

To use Anthropic with Portkey, [get your Anthropic API key from here](https://console.anthropic.com/settings/keys), then add it to Portkey to create your Anthropic virtual key.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your Anthropic Virtual Key
})
```
{% endtab %}

{% tab title="Python SDK" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtual_key="VIRTUAL_KEY"   # Replace with your virtual key for Anthropic
)
```
{% endtab %}

{% tab title="OpenAI Python SDK" %}
<pre class="language-python"><code class="lang-python">from openai import OpenAI
<strong>from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
</strong>
client = OpenAI(
    api_key="ANTHROPIC_API_KEY",
<strong>    base_url=PORTKEY_GATEWAY_URL,
</strong><strong>    default_headers=createHeaders(
</strong><strong>        api_key="PORTKEY_API_KEY",
</strong><strong>        provider="anthropic"
</strong><strong>    )
</strong>)
</code></pre>
{% endtab %}

{% tab title="OpenAI Node SDK" %}
<pre class="language-typescript"><code class="lang-typescript">import OpenAI from "openai";
<strong>import { PORTKEY_GATEWAY_URL, createHeaders } from "portkey-ai";
</strong>
const client = new OpenAI({
  apiKey: "ANTHROPIC_API_KEY",
<strong>  baseURL: PORTKEY_GATEWAY_URL,
</strong><strong>  defaultHeaders: createHeaders({
</strong><strong>    provider: "anthropic",
</strong><strong>    apiKey: "PORTKEY_API_KEY",
</strong><strong>  }),
</strong>});
</code></pre>
{% endtab %}
{% endtabs %}

### **3. Invoke Chat Completions with Anthropic**

Use the Portkey instance to send requests to Anthropic. You can also override the virtual key directly in the API call if needed.

{% tabs %}
{% tab title="NodeJS SDK" %}
```javascript
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'claude-3-opus-20240229',
    max_tokens: 250 // Required field for Anthropic
});

console.log(chatCompletion.choices[0].message.content);
```
{% endtab %}

{% tab title="Python SDK" %}
```python
chat_completion = portkey.chat.completions.create(
    messages= [{ "role": 'user', "content": 'Say this is a test' }],
    model= 'claude-3-opus-20240229',
    max_tokens=250 # Required field for Anthropic
)
    
print(chat_completion.choices[0].message.content)
```
{% endtab %}

{% tab title="OpenAI Python SDK" %}
```python
chat_completion = client.chat.completions.create(
    messages = [{ "role": 'user', "content": 'Say this is a test' }],
    model = 'claude-3-opus-20240229',
    max_tokens = 250
)

print(chat_completion.choices[0].message.content)
```
{% endtab %}

{% tab title="OpenAI Node SDK" %}
```typescript
async function main() {
    const chatCompletion = await client.chat.completions.create({
        model: "claude-3-opus-20240229",
        max_tokens: 1024,
        messages: [{ role: "user", content: "Hello, Claude" }],
    });
    console.log(chatCompletion.choices[0].message.content);
}

main();
```
{% endtab %}
{% endtabs %}

## Multimodal capabilities with Portkey - Vision Chat Completion Usage

Portkey's multimodal Gateway fully supports Anthropic's vision models `claude-3-sonnet`,  `claude-3-haiku`,  `claude-3-opus`, and the latest, `claude-3.5-sonnet`

Portkey follows the OpenAI schema, which means you can send your image data to Anthropic in the **same format** as OpenAI.

{% hint style="info" %}
* Anthropic **ONLY** accepts **`base64`**-encoded images. Unlike OpenAI, it **does not** support **`image URLs`**.
* With Portkey, you can use the same format to send base64-encoded images to both Anthropic and OpenAI models.
{% endhint %}

Here's an example using Anthropic `claude-3.5-sonnet` model

{% tabs %}
{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">import base64
import httpx
from portkey_ai import Portkey

# Fetch and encode the image
image_url = "https://upload.wikimedia.org/wikipedia/commons/a/a7/Camponotus_flavomarginatus_ant.jpg"
image_data = base64.b64encode(httpx.get(image1_url).content).decode("utf-8")


## Directly Using Portkey's REST API
Alternatively, you can also directly call Anthropic models through Portkey's REST API - it works exactly the same as OpenAI API, with 2 differences: 
1. You send your requests to Portkey's complete Gateway URL </br> ```https://api.portkey.ai/v1/chat/completions```
2. You have to add Portkey specific headers.
   1. ```x-portkey-api-key``` for sending your Portkey API Key
   2. ```x-portkey-virtual-key``` for sending your provider's virtual key (Alternatively, if you are not using Virtual keys, you can send your Auth header for your provider, and pass the ```x-portkey-provider``` header along with it)
```cURL
curl https://api.portkey.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ANYSCALE_API_KEY" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-provider: anyscale" \ 
  -d '{
    "model": "mistralai/Mistral-7B-Instruct-v0.1",
    "messages": [{"role": "user","content": "Hello!"}]
  }'
```
## Using anhtropic on Portkey's prompt playground
<Add in ss video here>

## How to Use Anthropic System Prompt

With Portkey, we make Anthropic models interoperable with the OpenAI schema and SDK methods. So, instead of passing the `system` prompt separately, you can pass it as part of the `messages` body, similar to OpenAI:

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">const chatCompletion = await portkey.chat.completions.create({
    messages: [
<strong>        { role: 'system', content: 'Your system prompt' },
</strong>        { role: 'user', content: 'Say this is a test' }
    ],
<strong>    model: 'claude-3-opus-20240229',
</strong>    max_tokens: 250
});

console.log(chatCompletion.choices);
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">completion = portkey.chat.completions.create(
    messages= [
<strong>        { "role": 'system', "content": 'Your system prompt' },
</strong>        { "role": 'user', "content": 'Say this is a test' }
    ],
<strong>    model= 'claude-3-opus-20240229',
</strong>    max_tokens=250 # Required field for Anthropic
)
    
print(completion.choices)
</code></pre>
{% endtab %}
{% endtabs %}

## How to switch between models
{% tabs %}
{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">
from portkey_ai import Portkey
# Initialize the Portkey client
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtualKey: "VIRTUAL_KEY"   # Add your Anthropic virtual key
});
completion = portkey.chat.completions.create(
    messages= [
        { "role": 'system', "content": 'Your system prompt' },
        { "role": 'user', "content": 'Say this is a test' }
    ],
    model= 'claude-3-opus-20240229', # Replace with your preferred model. For supported models, refer to "https://docs.portkey.ai/docs/integrations/llms/anthropic"
    max_tokens=250 # Required field for Anthropic
)
    
print(completion.choices)
</code></pre>
{% endtab %}
 
## Vision Chat Completion Usage

print(response)
</code></pre>
{% endtab %}

{% tab title="NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai';

const portkey = new Portkey({
  apiKey: "PORTKEY_API_KEY", 
  virtualKey: "VIRTUAL_KEY"
});


async function getChatCompletionFunctions() {
    const response = await portkey.chat.completions.create({
      model: "claude-3-5-sonnet-20240620",
      messages: [
        {
          role: "system",
          content: "You are a helpful assistant who describes images."
        },
        {
          role: "user",
          content: [
            { type: "text", text: "What's in this image?" },
<strong>            {
</strong><strong>              type: "image_url",
</strong><strong>              image_url: {
</strong><strong>                url: "data:image/jpeg;base64,BASE64_IMAGE_DATA"
</strong><strong>              }
</strong><strong>            }
</strong>          ]
        }
      ],
      max_tokens: 300
    });
    console.log(response);
  }
// Call the function
getChatCompletionFunctions();
</code></pre>
{% endtab %}

{% tab title="OpenAI NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import OpenAI from 'openai';
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const openai = new OpenAI({
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    apiKey: "PORTKEY_API_KEY",
    virtualKey: "ANTHROPIC_VIRTUAL_KEY"
  })
});

async function getChatCompletionFunctions(){
  const response = await openai.chat.completions.create({
    model: "claude-3-5-sonnet-20240620",
    messages: [
      {
        role: "user",
        content: [
          { type: "text", text: "What’s in this image?" },
<strong>          {
</strong><strong>            type: "image_url",
</strong><strong>            image_url: {
</strong><strong>              url: "data:image/jpeg;base64,BASE64_IMAGE_DATA"
</strong><strong>            }
</strong><strong>          }
</strong>        ],
      },
    ],
  });
  
  console.log(response)

}
await getChatCompletionFunctions();
</code></pre>
{% endtab %}

{% tab title="OpenAI Python" %}
<pre class="language-python"><code class="lang-python">from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

openai = OpenAI(
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="PORTKEY_API_KEY",
        virtual_key="ANTHROPIC_VIRTUAL_KEY"
    )
)


response = openai.chat.completions.create(
   model="claude-3-5-sonnet-20240620",
    messages=[
        {
            "role": "system",
            "content": "You are a helpful assistant, who describes imagse"
        },
        {
            "role": "user",
            "content": [
<strong>                {
</strong><strong>                    "type": "image_url",
</strong><strong>                    "image_url": {
</strong><strong>                        "url": f"data:image/jpeg;base64,{base_64_encoded_image}"
</strong><strong>                    }
</strong><strong>                }
</strong>            ]
        }
    ],
    max_tokens=1400,
)

print(response)
</code></pre>
{% endtab %}

{% tab title="REST" %}
<pre class="language-bash"><code class="lang-bash">curl "https://api.portkey.ai/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-virtual-key: ANTHROPIC_VIRTUAL_KEY" \
  -d '{
    "model": "claude-3-5-sonnet-20240620",
    "messages": [
      {
        "role": "system",
        "content": "You are a helpful assistant who describes images."
      },
      {
        "role": "user",
        "content": [
          {
            "type": "text",
            "text": "What is in this image?"
          },
<strong>          {
</strong><strong>            "type": "image_url",
</strong><strong>            "image_url": {
</strong><strong>              "url": "data:image/jpeg;base64,BASE64_IMAGE_DATA"
</strong><strong>            }
</strong><strong>          }
</strong>        ]
      }
    ],
    "max_tokens": 300
  }'
</code></pre>
{% endtab %}
{% endtabs %}

#### [API Reference](./#vision-chat-completion-usage)

On completion, the request will get logged in Portkey where any image inputs or outputs can be viewed. Portkey will automatically render the base64 images to help you debug any issues quickly.

**For more info, check out this guide:**

{% content-ref url="../../../product/ai-gateway/multimodal-capabilities/vision.md" %}
[vision.md](../../../product/ai-gateway/multimodal-capabilities/vision.md)
{% endcontent-ref %}

## Special feature - Prompt Caching
Portkey also works with Anthropic's new prompt caching feature and helps you save time & money for all your Anthropic requests. Refer to this guide to learn how to enable it:

{% content-ref url="prompt-caching.md" %}
[prompt-caching.md](prompt-caching.md)
{% endcontent-ref %}

## Managing Anthropic Prompts

You can manage all prompts to Anthropic in the [Prompt Library](../../../product/prompt-library.md). All the current models of Anthropic are supported and you can easily start testing different prompts.

Once you're ready with your prompt, you can use the `portkey.prompts.completions.create` interface to use the prompt in your application.

## Next Steps

The complete list of features supported in the SDK are available on the link below.

{% content-ref url="../../../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../../../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

You'll find more information in the relevant sections:

1. [Add metadata to your requests](../../../product/observability/metadata.md)
2. [Add gateway configs to your Anthropic requests](../../../product/ai-gateway/configs.md)
3. [Tracing Anthropic requests](../../../product/observability/traces.md)
4. [Setup a fallback from OpenAI to Anthropic's Claude APIs](../../../product/ai-gateway/fallbacks.md)
