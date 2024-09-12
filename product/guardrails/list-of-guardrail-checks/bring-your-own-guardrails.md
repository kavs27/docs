# Bring Your Own Guardrails

Portkey supports bringing your own Guardrail using a custom webhook:

<div align="left">

<figure><img src="../../../.gitbook/assets/CleanShot 2024-08-14 at 18.21.48@2x.png" alt="" width="375"><figcaption></figcaption></figure>

</div>

You can add `Webhook` as a Guardrail Check and setup any Guardrail Actions along with it. This is useful when you have an existing custom guardrail pipeline in your app and you are sending your LLM inputs or outputs to it for sync or async evaluation. Now, you can bring that onto Portkey's Gateway, make it production-grade, and enforce LLM behavior in real-time, easily.

## How Custom Webhook Works

In the Guardrail check, Portkey expects your `Webhook URL` and any `headers` (including `Authorization`) you need to send.

### Headers

Headers here should be a JSON object, like below:

```json
{
    "Authorization":"MY_API_KEY",
    "Content-Type":"Application/JSON"
}
```

Portkey makes a <mark style="color:green;">`POST`</mark> request to your webhook URL and expects two objects in the response: <mark style="color:orange;">`verdict`</mark> and <mark style="color:orange;">`data`</mark>.

## Webhook Response

Portkey expects two objects: `verdict` and `data`

| Object    | Type      | Required? |
| --------- | --------- | --------- |
| `verdict` | `boolean` | Yes       |
| `data`    | `any`     | No        |

#### Here's a sample webhook response:

```json
{
  "verdict": true,
  "data": {
    "reason": "User passed all security checks",
    "score": 0.95,
    "additionalInfo": {
      "userStatus": "verified",
      "lastCheckTimestamp": "2024-08-21T14:30:00Z"
    }
  }
}
```

#### Check out the Webhook implementation here:

{% @github-files/github-code-block url="https://github.com/Portkey-AI/gateway/blob/main/plugins/default/webhook.ts" %}

Based on the verdict value, the Guardrail Check will <mark style="color:green;">`PASS`</mark> or <mark style="color:red;">`FAIL`</mark>, and will have subsequent impact on the Guardrail Actions you've set.

{% hint style="success" %}
The webhook request automatically time out after **3 seconds** - this can not be changed. So, if a webhook request times out, the Guardrail verdict will return <mark style="color:green;">`PASS`</mark> for that request.
{% endhint %}

Head over to the [**Portkey Discord community**](https://portkey.ai/community) if you are building out custom webhooks and need any help!
