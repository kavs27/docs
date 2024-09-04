# Build a chatbot using Portkey's Prompt Templates

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1BZGkDisia\_beCibB3eaep0n87cIcqShR?usp=sharing)

Portkey's prompt templates offer a powerful solution for testing and building chatbots.  You can easily input your model prompt, adjust settings like **model type** and **temperature**, and instantly view outputs. Portkey's robust **versioning system** ensures that you can experiment freely with your prompts, allowing for easy rollback.  Try out new things without the fear of breaking your production. This seamless iteration process allows you to refine your chatbot's performance until you're satisfied.

## Setting Up Your Chatbot

Go to [Portkey's Prompts Dashboard](https://app.portkey.ai/prompts).&#x20;

Click on the **Create** button. You are now on the Prompt Playground.

### Step 1: Define Your System Prompt

Start by defining your system prompt. This sets the initial context and behavior for your chatbot. You can set this up in your Portkey's Prompt Library using the **JSON View**

<figure><img src="../../.gitbook/assets/Screenshot 2024-08-01 at 6.24.01 PM.png" alt=""><figcaption></figcaption></figure>

```json
[
  {
    "content": "You're a helpful assistant.",
    "role": "system"
  },
  {{chat_history}}
]
```

`{{chat_history}}` - will be used in the next step

### Step 2: Create a Variable to Store Conversation History

In the Portkey UI, set the variable type: Look for two icons next to the variable name: "T" and "{..}". Click the "{...}" icon to switch to **JSON** **mode**.

**Initialize the variable:** This array will store the conversation history, allowing your chatbot to maintain context. We can just initialize the variable with `[]`.

{% hint style="info" %}
**Note**: As your chatbot interacts with users, it will append new messages to this array, building a comprehensive conversation history.
{% endhint %}

<figure><img src="../../.gitbook/assets/Screenshot 2024-08-01 at 6.21.48 PM.png" alt=""><figcaption></figcaption></figure>

### Step 3: Implementing the Chatbot

Use Portkey's API to generate responses based on your prompt template. Here's a Python example::

```python
from portkey_ai import Portkey

client = Portkey(
    api_key="YOUR_PORTKEY_API_KEY"  # You can also set this as an environment variable
)

def generate_response(conversation_history):
    prompt_completion = client.prompts.completions.create(
        prompt_id="YOUR_PROMPT_ID",  # Replace with your actual prompt ID
        variables={
            "variable": conversation_history
        }
    )
    return prompt_completion.choices[0].message.content

# Example usage
conversation_history = [
    {
        "content": "Hello, how can I assist you today?",
        "role": "assistant"
    },
    {
        "content": "What's the weather like?",
        "role": "user"
    }
]

response = generate_response(conversation_history)
print(response)
```

### Step 4: Append the Response

After generating a response, append it to your conversation history:

```python
def append_response(conversation_history, response):
    conversation_history.append({
        "content": response,
        "role": "assistant"
    })
    return conversation_history

# Continuing from the previous example
conversation_history = append_response(conversation_history, response)
```

### Step 5: Take User Input to Continue the Conversation

Implement a loop to continuously take user input and generate responses:

```python
# Continue the conversation
while True:
    user_input = input("You: ")
    if user_input.lower() == 'exit':
        break
    
    conversation_history.append({
        "content": user_input,
        "role": "user"
    })
    
    response = generate_response(conversation_history)
    conversation_history = append_response(conversation_history, response)
    
    print("Bot:", response)

print("Conversation ended.")
```

### Complete Example

Here's a complete example that puts all these steps together:

```python
from portkey_ai import Portkey

client = Portkey(
    api_key="YOUR_PORTKEY_API_KEY"
)

def generate_response(conversation_history):
    prompt_completion = client.prompts.completions.create(
        prompt_id="YOUR_PROMPT_ID",
        variables={
            "variable": conversation_history
        }
    )
    return prompt_completion.choices[0].message.content

def append_response(conversation_history, response):
    conversation_history.append({
        "content": response,
        "role": "assistant"
    })
    return conversation_history

# Initial conversation
conversation_history = [
    {
        "content": "Hello, how can I assist you today?",
        "role": "assistant"
    }
]

# Generate and append response
response = generate_response(conversation_history)
conversation_history = append_response(conversation_history, response)

print("Bot:", response)

# Continue the conversation
while True:
    user_input = input("You: ")
    if user_input.lower() == 'exit':
        break
    
    conversation_history.append({
        "content": user_input,
        "role": "user"
    })
    
    response = generate_response(conversation_history)
    conversation_history = append_response(conversation_history, response)
    
    print("Bot:", response)

print("Conversation ended.")
```

## Conclusion

Voilà! You've successfully set up your chatbot using Portkey's prompt templates. Portkey enables you to experiment with various LLM providers. It acts as a definitive source of truth for your team, and it versions each snapshot of model parameters, allowing for easy rollback. Here's a snapshot of the Prompt Management UI. To learn more about Prompt Management [**click here**](../../product/prompt-library.md)**.**

<div align="left">

<figure><img src="../../.gitbook/assets/Group 1 from Figma (1).png" alt="" width="375"><figcaption></figcaption></figure>

</div>
