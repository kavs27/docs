# Vercel

Portkey natively integrates with the Vercel AI SDK to make your apps production-ready and reliable. Just import Portkey's Vercel package and use it as a provider in your Vercel AI app to enable all of Portkey features:

* Full-stack observability and tracing for all requests
* Interoperability across 250+ LLMS
* Built-in 50+ SOTA guardrails
* Simple & semantic caching to save costs & time
* Route requests conditionally and make them robust with fallbacks, load-balancing, automatic retries, and more
* Continuous improvement based on user feedback

## Getting Started

### 1. Installation

```bash
npm install @portkey-ai/vercel-provider
```

### 2. Import & Configure Portkey Object

[Sign up for Portkey ](https://portkey.ai)and get your API key, and configure Portkey provider in your Vercel app:

```typescript
import { createPortkey } from '@portkey-ai/vercel-provider';

const portkeyConfig = {
      "provider": "openai", // Choose your provider (e.g., 'anthropic')
      "api_key": "OPENAI_API_KEY",
      "model": "gpt-4o" // Select from 250+ models
};

const portkey = createPortkey({
  apiKey: 'YOUR_PORTKEY_API_KEY',
  config: portkeyConfig,
});
```

{% hint style="info" %}
Portkey's configs are a powerful way to manage & govern your app's behaviour. Learn more about Configs [here](../../../product/ai-gateway/configs.md).
{% endhint %}

## Using Vercel Functions

Portkey provider works with all of Vercel functions `generateText`, `streamText`, `generateObject`, `streamObject`.

Here's how to use them with Portkey:

{% tabs %}
{% tab title="generateText" %}
<pre class="language-javascript"><code class="lang-javascript"><strong>import { createPortkey } from '@portkey-ai/vercel-provider';
</strong>import { generateText } from 'ai';

const portkeyConfig = {
      "provider": "openai", // Choose your provider (e.g., 'anthropic')
      "api_key": "OPENAI_API_KEY",
      "model": "gpt-4o" // Select from 250+ models
};

const portkey = createPortkey({
  apiKey: 'YOUR_PORTKEY_API_KEY',
  config: portkeyConfig,
});

const { text } = await generateText({
  model: portkey.chatModel(''),
  prompt: 'What is Portkey?',
});

console.log(text);
</code></pre>
{% endtab %}

{% tab title="streamText" %}
<pre class="language-javascript"><code class="lang-javascript"><strong>import { createPortkey } from '@portkey-ai/vercel-provider';
</strong>import { streamText } from 'ai';

const portkeyConfig = {
      "provider": "openai", // Choose your provider (e.g., 'anthropic')
      "api_key": "OPENAI_API_KEY",
      "model": "gpt-4o" // Select from 250+ models
};

const portkey = createPortkey({
  apiKey: 'YOUR_PORTKEY_API_KEY',
  config: portkeyConfig,
});

const result = await streamText({
  model: portkey('gpt-4-turbo'),
  prompt: 'Invent a new holiday and describe its traditions.',
});

for await (const chunk of result) {
  console.log(chunk);
}
</code></pre>
{% endtab %}

{% tab title="generateObject" %}
<pre class="language-javascript"><code class="lang-javascript"><strong>import { createPortkey } from '@portkey-ai/vercel-provider';
</strong>import { generateObject } from 'ai';
import { z } from 'zod';

const portkeyConfig = {
      "provider": "openai", // Choose your provider (e.g., 'anthropic')
      "api_key": "OPENAI_API_KEY",
      "model": "gpt-4o" // Select from 250+ models
};

const portkey = createPortkey({
  apiKey: 'YOUR_PORTKEY_API_KEY',
  config: portkeyConfig,
});

const { object } = await generateObject({
  model: portkey('gpt-4-turbo'),
  schema: z.object({
    recipe: z.object({
      name: z.string(),
      ingredients: z.array(z.string()),
      steps: z.array(z.string()),
    }),
  }),
  prompt: 'Generate a lasagna recipe.',
});

console.log(JSON.stringify(object, null, 2));
</code></pre>
{% endtab %}

{% tab title="streamObject" %}
```javascript
import { createPortkey } from '@portkey-ai/vercel-provider';
import { streamObject } from 'ai';
import { z } from 'zod';

const portkeyConfig = {
      "provider": "openai", // Choose your provider (e.g., 'anthropic')
      "api_key": "OPENAI_API_KEY",
      "model": "gpt-4o" // Select from 250+ models
};

const portkey = createPortkey({
  apiKey: 'YOUR_PORTKEY_API_KEY',
  config: portkeyConfig,
});

const result = await streamObject({
  model: portkey('gpt-4-turbo'),
  schema: z.object({
    story: z.object({
      title: z.string(),
      characters: z.array(z.string()),
      plot: z.array(z.string()),
    }),
  }),
  prompt: 'Create a short story about time travel.',
});

for await (const chunk of result) {
  console.log(JSON.stringify(chunk, null, 2));
}
```
{% endtab %}
{% endtabs %}

***

## Portkey Features

Portkey Helps you make your Vercel app more robust and reliable. The portkey config is modular way to make it work for you in whatever way you want.&#x20;

### [Interoperability](../../../product/ai-gateway/universal-api.md)

Portkey allows you to easily switch between 250+ AI models by simply changing the model name in your configuration. This flexibility enables you to adapt to the evolving AI landscape without significant code changes.

{% tabs %}
{% tab title="Switch from OpenAI to Anthropic" %}
Here's how you'd use OpenAI with Portkey's Vercel integration:

<pre class="language-javascript"><code class="lang-javascript">const portkeyConfig = {
<strong>      "provider": "openai",
</strong><strong>      "api_key": "OPENAI_API_KEY",
</strong><strong>      "model": "gpt-4-turbo"
</strong>};
</code></pre>

Now, to switch to Anthropic, just change your provider slug to `anthropic` and enter your Anthropic API key along with the model of choice:

<pre class="language-javascript"><code class="lang-javascript">const portkeyConfig = {
<strong>      "provider": "anthropic",
</strong><strong>      "api_key": "Anthropic_API_KEY",
</strong><strong>      "model": "claude-3-5-sonnet-20240620"
</strong>};
</code></pre>
{% endtab %}
{% endtabs %}

### [Observability](../../../product/observability/)

Portkey's OpenTelemetry-compliant observability suite gives you complete control over all your requests. And Portkey's analytics dashboards provide **40**+ key insights you're looking for including cost, tokens, latency, etc. Fast.

<figure><img src="../../../.gitbook/assets/Portkey Group (20).png" alt=""><figcaption><p>Portkey's Observability Dashboard</p></figcaption></figure>

### Reliability

Portkey enhances the robustness of your AI applications with built-in features such as [Caching](../../../product/ai-gateway/cache-simple-and-semantic.md), [Fallback](../../../product/ai-gateway/fallbacks.md) mechanisms, [Load balancing](../../../product/ai-gateway/load-balancing.md), [Conditional routing](../../../product/ai-gateway/conditional-routing.md), [Request timeouts](../../../product/ai-gateway/request-timeouts.md), etc.&#x20;

Here is how you can modify your config to include the following Portkey features- &#x20;

{% tabs %}
{% tab title="Fallback" %}
<pre class="language-javascript"><code class="lang-javascript">import { createPortkey } from '@portkey-ai/vercel-provider';
import { generateText } from 'ai';

<strong>const portkeyConfig =  {
</strong>	"strategy": {
		"mode": "fallback"
	},
	"targets": [
		{ "virtual_key":"anthropic-key" },
		{ "virtual_key":"aws-bedrock-key" }
	]
}

const portkey = createPortkey({
  apiKey: 'YOUR_PORTKEY_API_KEY',
  config: portkeyConfig,
});

const { text } = await generateText({
  model: portkey.chatModel(''),
  prompt: 'What is Portkey?',
});

console.log(text);
</code></pre>
{% endtab %}

{% tab title="Caching" %}
```javascript
import { createPortkey } from '@portkey-ai/vercel-provider';
import { generateText } from 'ai';

const portkeyConfig = { "cache": { "mode": "semantic" } }

const portkey = createPortkey({
  apiKey: 'YOUR_PORTKEY_API_KEY',
  config: portkeyConfig,
});

const { text } = await generateText({
  model: portkey.chatModel(''),
  prompt: 'What is Portkey?',
});

console.log(text);
```
{% endtab %}

{% tab title="Conditional routing" %}
<pre class="language-javascript"><code class="lang-javascript"><strong>const portkey_config =  {
</strong>	"strategy": {
		"mode": "conditional",
		"conditions": [
			...conditions
		],
		"default": "target_1"
	},
	"targets": [
		{
			"name": "target_1",
			"virtual_key":"xx"
		},
		{
			"name": "target_2",
			"virtual_key":"yy"
		}
	]
}
</code></pre>
{% endtab %}
{% endtabs %}

Learn more about Portkey's AI gateway features in detail [here](../../../product/ai-gateway/).

### [Guardrails](./#guardrails)

Portkey Guardrails allow you to enforce LLM behavior in real-time, verifying both inputs and outputs against specified checks.&#x20;

You can create Guardrail checks in UI and then pass them in your Portkey Configs with before request or after request hooks.

[Read more about Guardrails here](../../../product/guardrails/).

***

## [Portkey Config](../../../product/ai-gateway/configs.md)

Many of these features are driven by Portkey's Config architecture. The Portkey app simplifies creating, managing, and versioning your Configs.

For more information on using these features and setting up your Config, please refer to the [Portkey documentation](https://docs.portkey.ai).
