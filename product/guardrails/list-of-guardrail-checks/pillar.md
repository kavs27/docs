# Pillar

[Pilla Security](https://www.pillar.security/) is an all-in-one platform that empowers organizations to monitor, assess risks, and secure their AI activities.

You can now use Pillar's advanced detection & evaluation models on Portkey with our open source integration, and make your app secure and reliable.

To get started with Pillar, chat with their team here:

{% embed url="https://www.pillar.security/get-a-demo" %}

## Using Pillar with Portkey

### 1. Add Pillar API Key to Portkey

* Navigate to the "Integrations" page under "Settings"
* Click on the edit button for the Pillar integration and add your API key

<figure><img src="../../../.gitbook/assets/CleanShot 2024-08-16 at 19.01.30@2x.png" alt=""><figcaption></figcaption></figure>

### 2. Add Pillar's Guardrail Check

* Now, navigate to the "Guardrails" page
* Search for Pillar's Guardrail Checks `Scan Prompt` or `Scan Response` and click on `Add`&#x20;
* Pick the info you'd like scanned and save the check.
* Set any actions you want on your check, and create the Guardrail!

<table><thead><tr><th width="167">Check Name</th><th>Description</th><th width="146">Parameters</th><th>Supported Hooks	</th></tr></thead><tbody><tr><td>Scan Prompt</td><td>Analyses your inputs for <code>prompt injection</code>, <code>PII</code>, <code>Secrets</code>, <code>Toxic Language</code>, and <code>Invisible Character</code></td><td>Dropdown</td><td><code>beforeRequestHooks</code></td></tr><tr><td>Scan Response</td><td>Analyses your outputs for <code>PII</code>, <code>Secrets</code>, and <code>Toxic Language</code></td><td>Dropdown</td><td><code>afterRequestHooks</code></td></tr></tbody></table>



<figure><img src="../../../.gitbook/assets/CleanShot 2024-08-16 at 19.02.01@2x.png" alt="" width="563"><figcaption></figcaption></figure>

Your Pillar Guardrail is now ready to be added to any Portkey request you'd like!

### 3. Add Guardrail ID to a Config and Make Your Request

* When you save a Guardrail, you'll get an associated Guardrail ID - add this ID to the `before_request_hooks` or `after_request_hooks` params in your Portkey Config
* Save this Config and pass it along with any Portkey request you're making!&#x20;

Your requests are now guarded by your Pillar checks and you can see the Verdict and any action you take directly on Portkey logs! More detailed logs for your requests will also be available on your Pillar dashboard.

***

## Get Support

If you face any issues with the Pillar integration, just ping the @Pillar team on the [community forum](https://discord.gg/portkey-llms-in-prod-1143393887742861333).
