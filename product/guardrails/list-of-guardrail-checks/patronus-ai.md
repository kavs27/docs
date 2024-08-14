# Patronus AI

Patronus excels in industry-specific guardrails for RAG workflows. It has a SOTA hallucination detection model Lynx, which is also [open source](https://www.patronus.ai/blog/lynx-state-of-the-art-open-source-hallucination-detection-model). Portkey integrates with multiple Patronus evaluators to help you enforce LLM behavior.

[Here's the list of all Patronus evaluators supported on Portkey.](patronus-ai.md#id-2.-add-patronus-guardrail-check) Browse Patronus' docs for more info:

{% embed url="https://docs.patronus.ai/docs/introduction" %}

## Using Patronus with Portkey

### 1. Add Patronus API Key to Portkey

{% hint style="info" %}
Grab your Patronus API [key from here](https://app.patronus.ai/).
{% endhint %}

On the "Integrations" page, click on the edit button for the Patronus and add your API key.

<figure><img src="../../../.gitbook/assets/CleanShot 2024-08-14 at 15.48.39@2x.png" alt=""><figcaption></figcaption></figure>

### 2. Add Patronus' Guardrail Checks & Actions

Navigate to the "Guardrails" page and you will see the Guardrail Checks offered by Patronus there. Add the ones you want, set actions, and create the Guardrail!

<div align="left">

<figure><img src="../../../.gitbook/assets/CleanShot 2024-08-14 at 15.50.50@2x.png" alt="" width="375"><figcaption></figcaption></figure>

</div>

#### List of Patronus Guardrail Checks

<table><thead><tr><th width="142">Check Name</th><th width="260">Description</th><th width="124">Parameters</th><th>Supported Hooks	</th></tr></thead><tbody><tr><td>Detect PHI</td><td>Checks for protected health information (PHI), defined broadly as any information about an individual's health status or provision of healthcare.</td><td><mark style="color:blue;"><strong><code>ON</code></strong></mark> or <mark style="color:red;"><strong><code>OFF</code></strong></mark></td><td><code>afterRequestHooks</code></td></tr><tr><td>Detect PII</td><td>Checks for personally identifiable information (PII) - this is information that, in conjunction with other data, can identify an individual.</td><td><mark style="color:blue;"><strong><code>ON</code></strong></mark> or <mark style="color:red;"><strong><code>OFF</code></strong></mark></td><td><code>afterRequestHooks</code></td></tr><tr><td>Is Concise</td><td>Check that the output is clear and concise.</td><td><mark style="color:blue;"><strong><code>ON</code></strong></mark> or <mark style="color:red;"><strong><code>OFF</code></strong></mark></td><td><code>afterRequestHooks</code></td></tr><tr><td>Is Helpful</td><td>Check that the output is helpful in its tone of voice.</td><td><mark style="color:blue;"><strong><code>ON</code></strong></mark> or <mark style="color:red;"><strong><code>OFF</code></strong></mark></td><td><code>afterRequestHooks</code></td></tr><tr><td>Is Polite</td><td>Check that the output is polite in conversation.</td><td><mark style="color:blue;"><strong><code>ON</code></strong></mark> or <mark style="color:red;"><strong><code>OFF</code></strong></mark></td><td><code>afterRequestHooks</code></td></tr><tr><td>No Apologies</td><td>Check that the output does not contain apologies.</td><td><mark style="color:blue;"><strong><code>ON</code></strong></mark> or <mark style="color:red;"><strong><code>OFF</code></strong></mark></td><td><code>afterRequestHooks</code></td></tr><tr><td>No Gender Bias</td><td>Check whether the output contains gender stereotypes. Useful to mitigate PR risk from sexist or gendered model outputs.</td><td><mark style="color:blue;"><strong><code>ON</code></strong></mark> or <mark style="color:red;"><strong><code>OFF</code></strong></mark></td><td><code>afterRequestHooks</code></td></tr><tr><td>No Racias Bias</td><td>Check whether the output contains any racial stereotypes or not.</td><td><mark style="color:blue;"><strong><code>ON</code></strong></mark> or <mark style="color:red;"><strong><code>OFF</code></strong></mark></td><td><code>afterRequestHooks</code></td></tr><tr><td>Retrieval Answer Relevance</td><td>Checks whether the answer is on-topic to the input question. Does not measure correctness.</td><td><mark style="color:blue;"><strong><code>ON</code></strong></mark> or <mark style="color:red;"><strong><code>OFF</code></strong></mark></td><td><code>afterRequestHooks</code></td></tr><tr><td>Detect Toxicity</td><td>Checks output for abusive and hateful messages.</td><td><mark style="color:blue;"><strong><code>ON</code></strong></mark> or <mark style="color:red;"><strong><code>OFF</code></strong></mark></td><td><code>afterRequestHooks</code></td></tr><tr><td>Custom Evaluator</td><td>Checks against custom criteria, based on Patronus evaluator profile name.</td><td><strong><code>string</code></strong> <br>(evaluator's profile name)</td><td><code>afterRequestHooks</code></td></tr></tbody></table>

Your Patronus Guardrail is now ready to be added to any Portkey request you'd like!

### 3. Add Guardrail ID to a Config and Make Your Request

{% hint style="info" %}
Patronus integration on Portkey currently only works on model outputs and not inputs.
{% endhint %}

* When you save a Guardrail, you'll get an associated Guardrail ID - add this ID to the `after_request_hooks` methods in your Portkey Config.
* Save this Config and pass it along with any Portkey request you're making!&#x20;

Your requests are now guarded by your Patronus evaluators and you can see the Verdict and any action you take directly on Portkey logs! More detailed logs for your requests will also be available on your Patronus dashboard.

***

## Get Support

If you face any issues with the Patronus integration, just ping the @patronusai team on the [community forum](https://discord.gg/portkey-llms-in-prod-1143393887742861333).
