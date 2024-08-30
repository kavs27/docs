---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Audio

## Example Usage

### Create Speech (Text-to-Speech)

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-typescript"><code class="lang-typescript">import fs from "fs";
import path from "path";
import Portkey from "portkey-ai";

const portkey = new Portkey({
  apiKey: "PORTKEY_API_KEY",
  virtualKey: "OPENAI_VIRTUAL_KEY",
});

const speechFile = path.resolve("./speech.mp3");

async function main() {
  const mp3 = await portkey.audio.speech.create({
    model: "tts-1",
    voice: "alloy",
<strong>    input: "Today is a wonderful day to build something people love!",
</strong>  });
  const buffer = Buffer.from(await mp3.arrayBuffer());
  await fs.promises.writeFile(speechFile, buffer);
}
main();

</code></pre>
{% endtab %}

{% tab title="Python" %}
```python
from pathlib import Path
from portkey_ai import Portkey

portkey = Portkey(
    api_key="PORTKEY_API_KEY",
    virtual_key="OPENAI_VIRTUAL_KEY"
)

speech_file_path = Path(__file__).parent / "speech.mp3"

response = portkey.audio.speech.create(
  model="tts-1",
  voice="alloy",
  input="1729"
)

f = open(speech_file_path, "wb")
f.write(response.content)
f.close()

```
{% endtab %}
{% endtabs %}

### Create Transcription & Translation (Speech-to-Text)

{% tabs %}
{% tab title="NodeJS" %}
```typescript
import fs from "fs";
import Portkey from "portkey-ai";

const portkey = new Portkey({
    apiKey: "PORTKEY_API_KEY",
    virtualKey: "OPENAI_VIRTUAL_KEY"
  })
});

// Transcription

async function transcribe() {
  const transcription = await portkey.audio.transcriptions.create({
    file: fs.createReadStream("speech.mp3"),
    model: "whisper-1",
  });

  console.log(transcription.text);
}
transcribe();

// Translation

async function translate() {
    const translation = await portkey.audio.translations.create({
        file: fs.createReadStream("speech.mp3"),
        model: "whisper-1",
    });
    console.log(translation.text);
}
translate();
```
{% endtab %}

{% tab title="Python" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
  api_key="PORTKEY_API_KEY",
  virtual_key="OPENAI_VIRTUAL_KEY"
)

audio_file= open("speech.mp3", "rb")

transcription = portkey.audio.transcriptions.create(
  model="whisper-1", 
  file=audio_file
)

print(transcription.text)

translation = client.audio.translations.create(
  model="whisper-1", 
  file=audio_file
)
print(translation.text)
```
{% endtab %}
{% endtabs %}
