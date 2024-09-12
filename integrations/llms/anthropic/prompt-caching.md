# Prompt Caching

Prompt caching on Anthropic lets you cache individual messages in your request for repeat use. With caching, you can free up your tokens to include more context in your prompt, and also deliver responses significantly faster and cheaper.

Portkey makes Anthropic's prompt caching work on our OpenAI-compliant universal API.

Just pass Anthropic's `anthropic-beta` header in your request, and set the `cache_control` param in your respective message body:

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import Portkey from 'portkey-ai'
 
const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY", // defaults to process.env["PORTKEY_API_KEY"]
    virtualKey: "VIRTUAL_KEY" // Your Anthropic Virtual Key
<strong>    anthropicBeta: "prompt-caching-2024-07-31"
</strong>})

const chatCompletion = await portkey.chat.completions.create({
    messages: [
        { "role": 'system', "content": [
            { 
                "type":"text","text":"You are a helpful assistant"
            },
<strong>            {
</strong><strong>                "type":"text","text":"&#x3C;TEXT_TO_CACHE>", 
</strong><strong>                "cache_control": {"type": "ephemeral"}
</strong><strong>            }
</strong>        ]},
        { "role": 'user', "content": 'Summarize the above story for me in 20 words' }
    ],
    model: 'claude-3-5-sonnet-20240620',
    max_tokens: 250 // Required field for Anthropic
});

console.log(chatCompletion.choices[0].message.content);
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    virtual_key="ANTHROPIC_VIRTUAL_KEY",
<strong>    anthropic_beta="prompt-caching-2024-07-31"
</strong>)

chat_completion = portkey.chat.completions.create(
    messages= [
        { "role": 'system', "content": [
            {
                "type":"text","text":"You are a helpful assistant"
            },
<strong>            {
</strong><strong>                "type":"text","text":"&#x3C;TEXT_TO_CACHE>",
</strong><strong>                "cache_control": {"type": "ephemeral"}
</strong><strong>            }
</strong>        ]},
        { "role": 'user', "content": 'Summarize the above story in 20 words' }
    ],
    model= 'claude-3-5-sonnet-20240620',
    max_tokens=250
)

print(chat_completion.choices[0].message.content)

</code></pre>
{% endtab %}

{% tab title="OpenAI NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import OpenAI from "openai";
import { PORTKEY_GATEWAY_URL, createHeaders } from "portkey-ai";

const portkey = new OpenAI({
    apiKey: "ANTHROPIC_API_KEY",
    baseURL: PORTKEY_GATEWAY_URL,
    defaultHeaders: createHeaders({
        provider: "anthropic",
        apiKey: "PORTKEY_API_KEY",
<strong>        anthropicBeta: "prompt-caching-2024-07-31"
</strong>  }),
});

const chatCompletion = await portkey.chat.completions.create({
    messages: [
        { "role": 'system', "content": [
            { 
                "type":"text","text":"You are a helpful assistant"
            },
            {
                "type":"text","text":"&#x3C;TEXT_TO_CACHE>", 
                "cache_control": {"type": "ephemeral"}
            }
        ]},
        { "role": 'user', "content": 'Summarize the above story for me in 20 words' }
    ],
    model: 'claude-3-5-sonnet-20240620',
    max_tokens: 250
});

console.log(chatCompletion.choices[0].message.content);
</code></pre>
{% endtab %}

{% tab title="OpenAI Python" %}
<pre class="language-python"><code class="lang-python">from openai import OpenAI
from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders

client = OpenAI(
    api_key="ANTHROPIC_API_KEY",
    base_url=PORTKEY_GATEWAY_URL,
    default_headers=createHeaders(
        api_key="PORTKEY_API_KEY",
        provider="anthropic",
<strong>        anthropic_beta="prompt-caching-2024-07-31"
</strong>    )
)

chat_completion = portkey.chat.completions.create(
    messages= [
        { "role": 'system', "content": [
            {
                "type":"text","text":"You are a helpful assistant"
            },
<strong>            {
</strong><strong>                "type":"text","text":"&#x3C;TEXT_TO_CACHE>",
</strong><strong>                "cache_control": {"type": "ephemeral"}
</strong><strong>            }
</strong>        ]},
        { "role": 'user', "content": 'Summarize the above story in 20 words' }
    ],
    model= 'claude-3-5-sonnet-20240620',
    max_tokens=250
)

print(chat_completion.choices[0].message.content)
</code></pre>
{% endtab %}

{% tab title="REST API" %}
<pre class="language-bash"><code class="lang-bash">curl https://api.portkey.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ANTHROPIC_API_KEY" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-provider: anthropic" \ 
<strong>  -H "x-portkey-anthropic-beta: prompt-caching-2024-07-31" \
</strong>  -d '{
    "model": "claude-3-5-sonnet-20240620",
    "max_tokens": 1024,
    "messages": [
        { "role": 'system', "content": [
            { 
                "type":"text","text":"You are a helpful assistant"
            },
<strong>            {
</strong><strong>                "type":"text","text":"&#x3C;TEXT_TO_CACHE>", 
</strong><strong>                "cache_control": {"type": "ephemeral"}
</strong><strong>            }
</strong>        ]},
        { "role": 'user', "content": 'Summarize the above story for me in 20 words' }
    ]
  }'
</code></pre>
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Anthropic currently has certain restrictions on prompt caching, like:

* Cache TTL is set at **5 minutes** and can not be changed
* The message you are caching needs to cross minimum length to enable this feature
  * 1024 tokens for Claude 3.5 Sonnet and Claude 3 Opus
  * 2048 tokens for Claude 3 Haiku
{% endhint %}

For more, refer to Anthropic's prompt caching documentation [here](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching).

## Seeing Cache Results in Portkey

Portkey automatically calculate the correct pricing for your prompt caching requests & responses based on Anthropic's calculations here:

<figure><img src="../../../.gitbook/assets/CleanShot 2024-09-11 at 01.13.16@2x.png" alt=""><figcaption></figcaption></figure>

In the individual log for any request, you can also see the exact status of your request and verify if it was cached, or delivered from cache with two `usage` parameters:

* `cache_creation_input_tokens`: Number of tokens written to the cache when creating a new entry.
* `cache_read_input_tokens`: Number of tokens retrieved from the cache for this request.

<figure><img src="../../../.gitbook/assets/CleanShot 2024-09-11 at 01.11.07@2x.png" alt=""><figcaption></figcaption></figure>

