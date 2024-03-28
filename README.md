# What is Portkey?

Teams use Portkey to improve the cost, performance, and accuracy of their Gen AI apps.

It takes <2 mins to integrate and with that, it already starts monitoring all of your LLM requests and also makes your app resilient, secure, performant, and more accurate at the same time.

Here's a product walkthrough (3 mins):

{% embed url="https://youtu.be/PgwK5dmvwTk" %}

***

### Integrate in 3 Lines of Code

{% tabs %}
{% tab title="OpenAI Python" %}
<pre class="language-python"><code class="lang-python"># pip install portkey-ai

from openai import OpenAI
<strong>from portkey_ai import PORTKEY_GATEWAY_URL, createHeaders
</strong>
client = OpenAI(
<strong>    base_url=PORTKEY_GATEWAY_URL,
</strong><strong>    default_headers=createHeaders(provider="openai", api_key="PORTKEY_API_KEY")
</strong>)

chat_complete = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Say this is a test"}],
)

print(chat_complete.choices[0].message.content)
</code></pre>
{% endtab %}

{% tab title="OpenAI Node" %}
<pre class="language-javascript"><code class="lang-javascript">// npm i portkey-ai

import OpenAI from 'openai';
<strong>import { PORTKEY_GATEWAY_URL, createHeaders } from 'portkey-ai'
</strong>
const openai = new OpenAI({
<strong>  baseURL: PORTKEY_GATEWAY_URL,
</strong><strong>  defaultHeaders: createHeaders({provider: "openai", apiKey: "PORTKEY_API_KEY"})
</strong>});

async function main() {
  const chatCompletion = await openai.chat.completions.create({
    messages: [{ role: 'user', content: 'Say this is a test' }],
    model: 'gpt-3.5-turbo',
  });

  console.log(chatCompletion.choices);
}

main();
</code></pre>
{% endtab %}
{% endtabs %}

{% content-ref url="welcome/make-your-first-request.md" %}
[make-your-first-request.md](welcome/make-your-first-request.md)
{% endcontent-ref %}

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><h3>✨ Product Features</h3></td><td>Jump to the product section to learn more about the Portkey modules and the use cases they solve.</td><td></td><td><a href="broken-reference">Broken link</a></td></tr><tr><td><h3>🚀 Quickstart</h3></td><td>Setting up Portkey takes . Check out how.</td><td></td><td><a href="welcome/make-your-first-request.md">make-your-first-request.md</a></td></tr><tr><td><h3>📔 API Reference</h3></td><td>Head to the API reference and code samples for all Portkey functionality available through REST APIs and SDKs</td><td></td><td><a href="broken-reference">Broken link</a></td></tr><tr><td><h3>🤝 Integrations</h3></td><td>Find the best integration for you with 0+ models across LLM providers and multiple frameworks. </td><td></td><td><a href="welcome/integration-guides/">integration-guides</a></td></tr></tbody></table>

### Languages Supported

<table><thead><tr><th width="171">Language</th><th>Supported Library</th></tr></thead><tbody><tr><td>Javascript</td><td><a href="https://github.com/Portkey-AI/portkey-node-sdk">portkey-node-sdk</a><br><a href="https://github.com/openai/openai-node">openai-node</a></td></tr><tr><td>Python</td><td><a href="https://github.com/Portkey-AI/portkey-python-sdk">portkey-python-sdk</a><br><a href="https://github.com/openai/openai-python">openai-python</a></td></tr><tr><td>Go</td><td><a href="https://github.com/sashabaranov/go-openai">go-openai</a></td></tr><tr><td>Java</td><td><a href="https://github.com/TheoKanning/openai-java">openai-java</a></td></tr><tr><td>Rust</td><td><a href="https://github.com/64bit/async-openai">async-openai</a></td></tr><tr><td>Ruby</td><td><a href="https://github.com/alexrudall/ruby-openai">ruby-openai</a></td></tr></tbody></table>

### AI Providers Supported

View all supported [integration guides here](welcome/integration-guides/)

<table><thead><tr><th width="169">AI Provider</th><th>Status<select multiple><option value="0e5d342ec974480189a514d494d2e511" label="fully supported" color="blue"></option><option value="9804c88061ab49f691e76c360acd3392" label="public" color="blue"></option><option value="ea651ba6b6904eb0bdf8db90bafd9404" label="partially supported" color="blue"></option></select></th></tr></thead><tbody><tr><td><a href="welcome/integration-guides/openai.md">OpenAI</a></td><td><span data-option="0e5d342ec974480189a514d494d2e511">fully supported, </span><span data-option="9804c88061ab49f691e76c360acd3392">public</span></td></tr><tr><td><a href="welcome/integration-guides/anthropic.md">Anthropic</a></td><td><span data-option="0e5d342ec974480189a514d494d2e511">fully supported, </span><span data-option="9804c88061ab49f691e76c360acd3392">public</span></td></tr><tr><td><a href="welcome/integration-guides/azure-openai.md">Azure OpenAI</a></td><td><span data-option="0e5d342ec974480189a514d494d2e511">fully supported, </span><span data-option="9804c88061ab49f691e76c360acd3392">public</span></td></tr><tr><td><a href="welcome/integration-guides/cohere.md">Cohere</a></td><td><span data-option="0e5d342ec974480189a514d494d2e511">fully supported, </span><span data-option="9804c88061ab49f691e76c360acd3392">public</span></td></tr><tr><td><a href="welcome/integration-guides/anyscale-llama2-mistral-zephyr.md">Anyscale</a></td><td><span data-option="0e5d342ec974480189a514d494d2e511">fully supported, </span><span data-option="9804c88061ab49f691e76c360acd3392">public</span></td></tr><tr><td><a href="welcome/integration-guides/google-palm.md">Google Palm</a></td><td><span data-option="0e5d342ec974480189a514d494d2e511">fully supported, </span><span data-option="9804c88061ab49f691e76c360acd3392">public</span></td></tr><tr><td><a href="welcome/integration-guides/gemini.md">Google Gemini</a></td><td><span data-option="0e5d342ec974480189a514d494d2e511">fully supported, </span><span data-option="9804c88061ab49f691e76c360acd3392">public</span></td></tr><tr><td><a href="welcome/integration-guides/together-ai.md">Together AI</a></td><td><span data-option="0e5d342ec974480189a514d494d2e511">fully supported, </span><span data-option="9804c88061ab49f691e76c360acd3392">public</span></td></tr><tr><td><a href="welcome/integration-guides/perplexity-ai.md">Perplexity</a></td><td><span data-option="0e5d342ec974480189a514d494d2e511">fully supported, </span><span data-option="9804c88061ab49f691e76c360acd3392">public</span></td></tr><tr><td><a href="welcome/integration-guides/mistral-ai.md">Mistral</a></td><td><span data-option="0e5d342ec974480189a514d494d2e511">fully supported, </span><span data-option="9804c88061ab49f691e76c360acd3392">public</span></td></tr><tr><td><a href="welcome/integration-guides/stability-ai.md">Stability</a></td><td><span data-option="0e5d342ec974480189a514d494d2e511">fully supported, </span><span data-option="9804c88061ab49f691e76c360acd3392">public</span></td></tr><tr><td><a href="welcome/integration-guides/nomic.md">Nomic</a></td><td><span data-option="0e5d342ec974480189a514d494d2e511">fully supported, </span><span data-option="9804c88061ab49f691e76c360acd3392">public</span></td></tr><tr><td>AI21</td><td><span data-option="0e5d342ec974480189a514d494d2e511">fully supported, </span><span data-option="9804c88061ab49f691e76c360acd3392">public</span></td></tr><tr><td>AWS Bedrock</td><td><span data-option="0e5d342ec974480189a514d494d2e511">fully supported, </span><span data-option="9804c88061ab49f691e76c360acd3392">public</span></td></tr><tr><td>AzureML</td><td><span data-option="ea651ba6b6904eb0bdf8db90bafd9404">partially supported</span></td></tr><tr><td>BYOLLM</td><td><span data-option="0e5d342ec974480189a514d494d2e511">fully supported, </span><span data-option="9804c88061ab49f691e76c360acd3392">public</span></td></tr></tbody></table>

### Frameworks Supported

<table><thead><tr><th width="172">Framework</th><th>Status<select multiple><option value="c20415239ee94fdd8c0240eb0cb2b98f" label="python" color="blue"></option><option value="e7e31fdc78ee49cf8a5349b051d71dd5" label="typescript" color="blue"></option><option value="b5571db3c54b4adc9d43af9802742cd6" label="native" color="blue"></option></select></th></tr></thead><tbody><tr><td><a href="welcome/integration-guides/langchain-python.md">Langchain</a></td><td><span data-option="b5571db3c54b4adc9d43af9802742cd6">native, </span><span data-option="c20415239ee94fdd8c0240eb0cb2b98f">python</span></td></tr><tr><td><a href="welcome/integration-guides/llama-index-python.md">Llamaindex</a></td><td><span data-option="b5571db3c54b4adc9d43af9802742cd6">native, </span><span data-option="c20415239ee94fdd8c0240eb0cb2b98f">python</span></td></tr></tbody></table>
