# Conditional Routing

{% hint style="info" %}
This feature is available on all Portkey [plans](https://portkey.ai/pricing).
{% endhint %}

Using Portkey Gateway, you can route your requests to different provider targets based on custom conditions you define. These can be conditions like:

* If this user is on the `paid plan`, route their request to a `custom fine-tuned model`
* If this user is an `EU resident`, call an `EU hosted model`
* If the request is coming from `testing environment` with a `llm-pass-through` flag, route it to the `cheapest model`&#x20;
* ..and more!

Using this strategy, you can set up various conditional checks on the `metadata` keys you're passing with your requests, and based on whether the checks <mark style="color:red;">`FAIL`</mark> or <mark style="color:green;">`PASS`</mark>, you can route the request to the appropriate target — all happening very fast on the _gateway_, on _edge_.

***

## Enabling Conditional Routing

Conditional routing is one of the _strategies_ in Portkey's [Gateway Configs](configs.md). (others being `fallback` and `loadbalance`). To use it in your app,&#x20;

1. You need to create a `conditional` config in Portkey UI.
2. Save the Config and get an associated Config ID.
3. And just pass the Config ID along with your requests using the `config` param.

***

## 1. Creating the `conditional` Config

Here's how a sample `conditional` config looks (along with its simpler, tree view).&#x20;

{% tabs %}
{% tab title="Sample Config" %}
```json
{
	"strategy": {
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
```
{% endtab %}

{% tab title="Tree View" %}
```
│─ config
│  │─ strategy mode:conditional
│  │  │─ conditions 
│  │  │  │─ array of conditions
│  │  │─ default 
│  │  │  │─ target name
│  │─ targets
│  │  │─ target 1
│  │  │  │─ name
│  │  │  │─ provider details
│  │  │─ target 2
│  │  │  │─ name
│  │  │  │─ provider details
```
{% endtab %}
{% endtabs %}

* `strategy.mode`: Set to `conditional`
* `strategy.conditions`: Query conditions with rules applied on metadata values along with which target to call when the condition passes
* `strategy.default`: The default target name to call when none of the conditions pass
* `targets`: Array of target objects with unique `names` and provider details. These target names are referenced in the `conditions` objects above.

{% hint style="info" %}
`conditions` and `default` are **required params** for the `conditional` strategy.
{% endhint %}

### Structure of `conditions` Object

`conditions` are where you will actually write the routing rules. Here's a sample `condition` object:

```json
{
  "query": { "metadata.user_plan": { "$eq": "paid" } },
  "then": "finetuned-gpt4"
}
```

* `query`: Write the exact rule for checking metadata values
* `then`: Define which target to call if the query <mark style="color:green;">`PASSES`</mark>&#x20;

### List of Condition Query Operators

| Operator | Description     |
| -------- | --------------- |
| `$eq`    | Equals          |
| `$ne`    | Not equals      |
| `$in`    | In array        |
| `$nin`   | Not in array    |
| `$regex` | Match the regex |

### Logical Query Operators

* `$and`: All conditions must be true
* `$or`: At least one condition must be true

#### Example Condition objects with Logical Operators

{% tabs %}
{% tab title="AND" %}
```json
{
  "$and": [
    { "metadata.user_type": { "$eq": "pro" } },
    { "metadata.model": { "$eq": "gpt-4" } }
  ]
}
```
{% endtab %}

{% tab title="OR" %}
```json
{
  "$or": [
    { "metadata.user_type": { "$eq": "pro" } },
    { "metadata.user_quota": { "$eq": "premium" } }
  ]
}
```
{% endtab %}

{% tab title="NESTING AND, OR" %}
```json
{
  "$or": [
    {
      "$and": [
        { "metadata.user_type": { "$eq": "pro" } },
        { "metadata.user_tier": { "$eq": "tier-1" } }
      ]
    },
    { "metadata.user_quota": { "$eq": "premium" } }
  ]
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
1. You can write nested queries (with `$and`, `$or` operators)
2. When a condition is incorrect or it fails, Portkey moves on to the next condition until it finds a successful condition.
3. If no conditions pass, then the `default` target name is called
4. Since Portkey iterates through the queries sequentially, the order of your conditions is important
{% endhint %}

***

## 2. Getting Config ID

Based on the `conditions` and the Config structure described above, you can create your [Config in Portkey UI](https://app.portkey.ai/configs), and save it to get Config ID. The UI also helps you autofill and autoformat your Config.

#### Adding the above sample condition to our final Config:

{% tabs %}
{% tab title="Simple Conditional Config" %}
```json
{
	"strategy": {
		"mode": "conditional",
		"conditions": [
			{
			  "query": { "metadata.user_plan": { "$eq": "paid" } },
			  "then": "finetuned-gpt4"
			},
			{
			  "query": { "metadata.user_plan": { "$eq": "free" } },
			  "then": "base-gpt4"
			}			
		],
		"default": "target_1"
	},
	"targets": [
		{
			"name": "finetuned-gpt4",
			"virtual_key":"xx"
		},
		{
			"name": "base-gpt4",
			"virtual_key":"yy"
		}
	]
}
```
{% endtab %}

{% tab title="Nested Conditional Config" %}
```json
{
  "strategy": {
    "mode": "conditional",
    "conditions": [
      {
        "query": {
          "$and": [
            { "metadata.user_type": { "$eq": "pro" } },
            { "metadata.user_tier": { "$eq": "tier-1" } }
          ]
        },
        "then": "gpt4_v2_target"
      },
      {
        "query": {
          "$or": [
            { "metadata.client": { "$eq": "UI" } },
            { "metadata.app_name": { "$regex": "my_app" } }
          ]
        },
        "then": "app_target"
      }
    ],
    "default": "default_target"
  },
  "targets": [
    { "name": "gpt4_v2_target", "virtual_key": "openai-xx"},
    { "name": "app_target", "virtual_key": "openai-yy" },
    { "name": "default_target", "virtual_key": "openai-zz" }
  ]
}
```
{% endtab %}
{% endtabs %}

***

## 3. Using the Config ID in Requests

Now, while instantiating your Portkey client or while sending headers, you just need to pass the Config ID and all your requests will start getting routed according to your conditions.

Conditional routing happens on Portkey's on-the-edge stateless AI Gateway. We scan for the given query field in your request body, apply the query condition, and route to the specified target based on it.&#x20;

Currently, we support **Metadata based routing** — i.e. routing your requests based on the metadata values you're sending along with your request.&#x20;

### Applying Conditional Routing Based on Metadata

{% tabs %}
{% tab title="Sample Full Request" %}
<pre class="language-python"><code class="lang-python">from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
<strong>    config="my-conditional-router-config"
</strong>)

response = portkey.with_options(
    metadata = {
        "user_plan": "free",
        "environment": "production",
        "session_id": "1729"
}).chat.completions.create(
    messages = [{ "role": 'user', "content": 'What is 1729' }]
)
</code></pre>

[Here, we're using the following Config that we defined above](conditional-routing.md#adding-the-above-sample-condition-to-our-final-config).
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Soon, Portkey will also support routing based on other critical parameters like `input character count`, `input token count`, `prompt type`, `tool support`, and more.\
\
Similarly, we will also add support for smart routing to wider targets, like `fastest`, `cheapest`, `highest uptime`, `lowest error rate`, etc.

\
[Please join us on Discord](https://portkey.wiki/chat) to share your thoughts on this feature and get early access to more  routing capabilities.
{% endhint %}
