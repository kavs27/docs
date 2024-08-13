# Patronus AI

Patronus excels in indstry and retrieval specific guardrails. It has a SOTA hallucination detection model Lynx, which is also [open source](https://www.patronus.ai/blog/lynx-state-of-the-art-open-source-hallucination-detection-model). Portkey integrates with Patronus' Lynx API to help you detect hallucinations in your requests.

Browse Patronus' docs for more info on each of the Guardrail: &#x20;

{% embed url="https://docs.patronus.ai/docs/introduction" %}

## Using Patronus with Portkey

### 1. Add Patronus API Key to Portkey

* Navigate to the "Integrations" page under "Settings"
* Click on the edit button for the Patronus integration and add your API key

### 2. Add Patronus' Guardrail Check

* Now, navigate to the "Guardrails" page
* Search for "Lynx" Guardrail Check and click on `Add`&#x20;
* Input your corresponding context, and save the check.
* Then set any actions you want on the check, and create the Guardrail!

<table><thead><tr><th width="167">Check Name</th><th>Description</th><th width="146">Parameters</th><th>Supported Hooks	</th></tr></thead><tbody><tr><td>TBD</td><td>TBD</td><td>Project ID: <code>string</code></td><td><code>beforeRequestHooks</code><br><br><code>afterRequestHooks</code></td></tr></tbody></table>

Your Aporia Guardrail is now ready to be added to any Portkey request you'd like!

### 3. Add Guardrail ID to a Config and Make Your Request

* When you save a Guardrail, you'll get an associated Guardrail ID - add this ID to the `before_request_hooks` or `after_request_hooks` methods in your Portkey Config
* Save this Config and pass it along with any Portkey request you're making!&#x20;

Your requests are now guarded by your Aporia policies and you can see the Verdict and any action you take directly on Portkey logs! More detailed logs for your requests will also be available on your Aporia dashboard.

***

## Get Support

If you face any issues with the Patronus integration, just ping the @patronusai team on the [community forum](https://discord.gg/portkey-llms-in-prod-1143393887742861333).
