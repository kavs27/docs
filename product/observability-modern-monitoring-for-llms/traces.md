# Tracing

{% hint style="success" %}
This feature is available for all plans:-

* [Developer](https://app.portkey.ai/): 10k Logs / Month with 3 day Log Retention
* [Production](https://app.portkey.ai/): 100k Logs / Month + $9 for additional 100k  with 30 Days Log Retention
* [Enterprise](https://portkey.ai/docs/product/enterprise-offering): Unlimited
{% endhint %}

Portkey helps you trace the entire lifecycle of your request in a single, common view.&#x20;

<figure><img src="../../.gitbook/assets/traces.gif" alt=""><figcaption></figcaption></figure>

Tracing on Portkey is especially helpful when you are running agentic workflows, or chat bots or have multi-step LLM calls happening for a single user request in your app.

Typically, Portkey would show all the logs in your multi-step calls separately on our dashboard. But with tracing, you can just enable the "Traces" view in the Logs page and we will automatically combine all the requests under a common trace ID and show you a much better, chronological view of your entire request's lifecycle.

### How to Enable Request Tracing

You can pass your Trace ID at time of making a request like this:

{% tabs %}
{% tab title="NodeJS" %}
```javascript
const requestOptions = {traceID: "YOUR TRACE ID"}
const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-3.5-turbo',
}, requestOptions);

console.log(chatCompletion.choices);
```
{% endtab %}

{% tab title="Python" %}
```python
completion = portkey.with_options(
    trace_id = "TRACE_ID"
).chat.completions.create(
    messages = [{ "role": 'user', "content": 'Say this is a test' }],
    model = 'gpt-3.5-turbo'
)
```
{% endtab %}

{% tab title="OpenAI NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">import { createHeaders } from 'portkey-ai'

const reqHeaders = {headers: <a data-footnote-ref href="#user-content-fn-1">createHeaders</a>({"traceID": "TRACE ID"})}

const chatCompletion = await openai.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-3.5-turbo',
}, reqHeaders);
</code></pre>
{% endtab %}

{% tab title="OpenAI Python" %}
```python
from portkey_ai import createHeaders

req_headers = createHeaders(trace_id="TRACE ID")

chat_complete = client.with_options(headers=req_headers).chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Say this is a test"}],
)
```
{% endtab %}

{% tab title="REST API" %}
<pre class="language-bash"><code class="lang-bash">curl https://api.portkey.ai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "x-portkey-api-key: $PORTKEY_API_KEY" \
  -H "x-portkey-provider: openai" \ 
<strong>  -H "x-portkey-trace-id: TRACE_ID" \
</strong>  -d '{
    "model": "gpt-4-turbo",
    "messages": [{
        "role": "system",
        "content": "You are a helpful assistant."
      },{
        "role": "user",
        "content": "Hello!"
      }]
  }'
</code></pre>
{% endtab %}
{% endtabs %}

Alternatively, you can also set trace ID at the client level:

{% tabs %}
{% tab title="NodeJS" %}
```javascript
import Portkey from 'portkey-ai';

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
    virtualKey: "VIRTUAL_KEY",
    traceID: "TRACE_ID"
})
```
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    virtual_key="VIRTUAL_KEY",
<strong>    trace_id="TRACE_ID"
</strong>)
</code></pre>
{% endtab %}
{% endtabs %}

Portkey also supports sending the trace ID as part of custom metadata:

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-typescript"><code class="lang-typescript">import Portkey from 'portkey-ai';

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
    virtualKey: "VIRTUAL_KEY",
})

const chatCompletion = await portkey.chat.completions.create({
    messages: [{ role: 'user', content: 'Who was ariadne?' }],
    model: 'gpt-4',
}, 
    metadata: {
        "_user": "USER_ID",
        "organisation": "ORG_ID",
<strong>        "traceId": "1729"
</strong>});

console.log(chatCompletion.choices);    
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    virtual_key="OPENAI_VIRTUAL_KEY"
)

response = portkey.with_options(
    metadata = {
        "_user": "USER_ID",
<strong>        "traceId": "1729"
</strong>}).chat.completions.create(
    messages = [{ "role": 'user', "content": 'What is 1729' }],
    model = 'gpt-4'
)

print(response.choices[0].message)
</code></pre>
{% endtab %}
{% endtabs %}

***

### Tracing Langchain Requests

Portkey has a dedicated handler that can instrument your Langchain requests and trace them correctly.

{% tabs %}
{% tab title="Trace Langchain Requests (Python)" %}
1. First, install Portkey SDK, and Langchain's packages

```bash
$ pip install langchain_OpenAI portkey-ai langchain_community
```

2. Import the packages

<pre class="language-python"><code class="lang-python">from langchain_openai import ChatOpenAI
from langchain.chains import LLMChain
<strong>from portkey_ai.langchain import LangchainCallbackHandler
</strong>from portkey_ai import createHeaders
</code></pre>

3. Instantiate Portkey's Langchain Callback Handler

<pre class="language-python"><code class="lang-python"><strong>portkey_handler = LangchainCallbackHandler(
</strong><strong>      api_key="YOUR_PORTKEY_API_KEY", 
</strong><strong>      metadata={
</strong><strong>            "user_name": "User_Name",
</strong><strong>            "traceId": "Langchain_sample_callback_handler"
</strong><strong>      }
</strong>)
</code></pre>

4. Pass it with `ChatOpenAI` or while invoking the LLM

<pre><code>llm = ChatOpenAI(
    api_key="OPENAI_API_KEY",
<strong>    callbacks=[portkey_handler],
</strong>)

chain = LLMChain(
    llm=llm,
    prompt=prompt,
<strong>    callbacks=[portkey_handler] # You can also pass it here
</strong>)

handler_config = {'callbacks' : [portkey_handler]}

<strong>chain.invoke({"input": "what is langchain?"}, config=handler_config) # This will also work
</strong></code></pre>
{% endtab %}
{% endtabs %}

***

### Tracing Llamaindex Requests

Portkey has a dedicated handler to instrument your Llamaindex requests on Portkey.

{% tabs %}
{% tab title="Trace Llamaindex Requests" %}
1. First, install Portkey SDK, and Langchain's packages

```bash
$ pip install openai portkey-ai llama-index 
```

2. Import the packages

<pre class="language-python"><code class="lang-python">from llama_index.llms.openai import OpenAI
<strong>from portkey_ai.llamaindex import LlamaIndexCallbackHandler
</strong>from llama_index.core import Settings
from llama_index.core.callbacks import CallbackManager
</code></pre>

3. Instantiate Portkey's Langchain Callback Handler

```python
portkey_handler = LlamaIndexCallbackHandler(
      api_key="PORTKEY_API_KEY",
      metadata={
            "user_name": "User_Name",
            "traceId": "Llamaindex_sample_callback_handler"
      }
)
```

4. Pass it with `OpenAI` or with `callback_manager`&#x20;

<pre class="language-python"><code class="lang-python">llm = OpenAI(
    model="gpt-4o",
    api_key="OPENAI_API_KEY",
<strong>    callbacks=[portkey_handler],
</strong>)

# You can also set up global settings with the callback handler

Settings.callback_manager = CallbackManager([portkey_handler])
Settings.llm = llm
</code></pre>
{% endtab %}
{% endtabs %}

***

## Inserting Logs

If you are using the [Insert Log API](../../portkey-endpoints/logs/insert-a-log.md) to add logs to Portkey, your `traceId`, `spanId` etc. will become part of the metadata object in your log, and Portkey will instrument your requests to take into account these values.

The logger endpoint supports inserting a single log as well as log array, and helps you build traces of any depth or complexity.

{% content-ref url="../../portkey-endpoints/logs/insert-a-log.md" %}
[insert-a-log.md](../../portkey-endpoints/logs/insert-a-log.md)
{% endcontent-ref %}

***

## Tracing for Gateway Features

Portkey tracing also works very well to capture the Gateway behavior on retries, fallbacks, etc. Portkey will automatically group all the requests that were part of a single fallback or retry config and show the failed and succeeded requests chronologically as "spans" inside a "trace".

This is especially useful when you want to understand the total latency and behavior of your app when retry or fallbacks were triggered.

***

##

sfasf

***

## How Tracing on Portkey Works

Portkey does OpenTelemetry-compliant tracing for your requests. You just need to pass the trace ID with your requests, and with that, all of your LLM calls that have the same trace ID will show up together as different "spans" in the Traces view.

Now, using Portkey, you can also instrument your tracing EXACTLY as you want, by passing additional metadata keys to denote **child spans** and **parent spans** in a **trace.**

{% hint style="info" %}
A "Span" is just another word for an LLM call here, but one that is part of parent trace.
{% endhint %}

### Trace Tree

Portkey traces uses a tree data structure, similar to OTel

```
traceId
├─ parentSpanId
│  ├─ spanId
│  ├─ spanName
```

Each node in the tree is a span with a unique span\_id and optional span\_name. Child spans link to a single parent via parent\_span\_id. Spans without parent become root nodes

| Key          | Expected Value | Required? |
| ------------ | -------------- | --------- |
| traceId      | Unique string  | YES       |
| spanId       | Unique string  | NO        |
| spanName     | string         | NO        |
| parentSpanId | Unique string  | NO        |

You can pass the above additional fields as part of metadata with any request:

```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    virtual_key="OPENAI_VIRTUAL_KEY"
)

response = portkey.with_options(
    metadata = {
        "traceId": "1729",
        "parentSpanId: "call_start",
        "spanId": "llm_call",
        "spanName": "LLM Call"
}).chat.completions.create(
    messages = [{ "role": 'user', "content": 'What is 1729' }],
    model = 'gpt-4'
)

print(response.choices[0].message)
```

Based on these values, Portkey will instrument your requests, and show the exact trace with its span on the "Traces" view.

{% hint style="info" %}
Using Metadata to help you instrument Traces is very powerful. Beyond traces & spans, you can add any details you require to the metadata and Portkey can give you filtered traces for that metadata key:value, as well as give aggregates stats for it.&#x20;
{% endhint %}

## When to Use Tracing

This is helpful for the following reasons:

1. You can see aggregate LLM costs at the trace ID level, which can make it effortless to see how much each agent run cost, or each user chat session cost, etc.
2. You can easily browse through all the requests in a single trace on Portkey and identify if it failed at any stage
3. You can see the entire request lifecycle along with how much time the entire trace took

***

## Capturing User Feedback

Trace IDs can also be used to link user feedback to specific generations. This can be used in a system where users provide feedback, like a thumbs up or thumbs down, or something more complex via our feedback APIs. This feedback can be linked to traces which can span over a single generation or multiple ones. Read more here:

{% content-ref url="feedback.md" %}
[feedback.md](feedback.md)
{% endcontent-ref %}

[^1]: Imported from the Portkey SDK
