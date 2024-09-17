# Anthropic

Portkey provides a robust and secure gateway to facilitate the integration of various Large Language Models (LLMs) into your applications, including [Anthropic's Claude APIs](https://docs.anthropic.com/claude/reference/getting-started-with-the-api).

With Portkey, you can take advantage of features like fast AI gateway access, observability, prompt management, and more, all while ensuring the secure management of your LLM API keys through a [virtual key](../../../product/ai-gateway/virtual-keys/) system.

{% hint style="info" %}
Provider Slug**: **<mark style="color:blue;">**`anthropic`**</mark>
{% endhint %}

## Portkey SDK Integration with Anthropic

Portkey provides a consistent API to interact with models from various providers. To integrate Anthropic with Portkey:

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

## Vision Chat Completion Usage

Portkey's multimodal Gateway fully supports Anthropic's vision models `claude-3-sonnet`, `claude-3-haiku`,  `claude-3-opus`, and the newest `claude-3.5-sonnet`. Images are made available to Anthropic's model by passing the **base64** encoded image directly in the request.

Here's an example using Anthropic `claude-3.5-sonnet` model

{% tabs %}
{% tab title="Python" %}
```python
import base64
import httpx
from portkey_ai import Portkey

# Fetch and encode the image
image1_url = "https://upload.wikimedia.org/wikipedia/commons/a/a7/Camponotus_flavomarginatus_ant.jpg"
image1_data = base64.b64encode(httpx.get(image1_url).content).decode("utf-8")

# Initialize the Portkey client
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",  # Replace with your Portkey API key
    virtualKey: "VIRTUAL_KEY"   # Add your provider's virtual key
});

# Create the request
response = portkey.chat.completions.create(
    model="claude-3-5-sonnet-20240620",
    messages=[
        {
            "role": "system",
            "content": "You are a helpful assistant, who describes imagse"
        },
        {
            "role": "user",
            "content": [
                {
                    "type": "image_url",
                    "image_url": {
                        "url": f"data:image/jpeg;base64,{image1_data}"
                    }
                }
            ]
        }
    ],
    max_tokens=1400,
)
print(response)
```
{% endtab %}

{% tab title="NodeJS" %}
```javascript
import Portkey from 'portkey-ai';

// Initialize the Portkey client
const portkey = new Portkey({
  apiKey: "PORTKEY_API_KEY", // Replace with your Portkey API key
  virtualKey: "VIRTUAL_KEY" // Add your anthropic's virtual key
});

// Generate a chat completion
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
            {
              type: "image_url",
              image_url: {
                url: "data:image/jpeg;base64,BASE64_IMAGE_DATA"
              }
            }
          ]
        }
      ],
      max_tokens: 300
    });
    console.log(response);
  }
// Call the function
getChatCompletionFunctions();
```
{% endtab %}

{% tab title="OpenAI NodeJS" %}
```javascript
import OpenAI from 'openai'; // We're using the v4 SDK
import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'

const openai = new OpenAI({
  apiKey: 'ANTHROPIC_API_KEY', // defaults to process.env["OPENAI_API_KEY"],
  baseURL: PORTKEY_GATEWAY_URL,
  defaultHeaders: createHeaders({
    provider: "openai",
    apiKey: "PORTKEY_API_KEY" // defaults to process.env["PORTKEY_API_KEY"]
  })
});

// Generate a chat completion with streaming
async function getChatCompletionFunctions(){
  const response = await openai.chat.completions.create({
    model: "gpt-4-vision-preview",
    messages: [
      {
        role: "user",
        content: [
          { type: "text", text: "What’s in this image?" },
          {
            type: "image_url",
            image_url:
              "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg",
          },
        ],
      },
    ],
  });
  
  console.log(response)

}
await getChatCompletionFunctions();
```
{% endtab %}

{% tab title="OpenAI Python" %}
```python
from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

openai = OpenAI(
    api_key='Anthropic_API_KEY',
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        provider="anthropic",
        api_key="PORTKEY_API_KEY"
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
                {
                    "type": "image_url",
                    "image_url": {
                        "url": f"data:image/jpeg;base64,{base_64_encoded_image}"
                    }
                }
            ]
        }
    ],
    max_tokens=1400,
)

print(response)
```
{% endtab %}

{% tab title="REST" %}
```bash
curl "https://api.portkey.ai/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-provider: anthropic" \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
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
            "text": "What's in this image?"
          },
          {
            "type": "image_url",
            "image_url": {
              "url": "data:image/jpeg;base64,BASE64_IMAGE_DATA"
            }
          }
        ]
      }
    ],
    "max_tokens": 300
  }'
```
{% endtab %}
{% endtabs %}

#### [API Reference](./#vision-chat-completion-usage)

On completion, the request will get logged in the logs UI where any image inputs or outputs can be viewed. Portkey will automatically load the base64 images making for a great debugging experience with vision models.

<figure><img src="../../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

For more info, check out this guide:

{% content-ref url="../../../product/ai-gateway/multimodal-capabilities/vision.md" %}
[vision.md](../../../product/ai-gateway/multimodal-capabilities/vision.md)
{% endcontent-ref %}

## Prompt Caching

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
