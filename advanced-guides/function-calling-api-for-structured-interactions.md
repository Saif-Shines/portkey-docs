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

# Function Calling API for structured interactions

This cookbook will guide you through the process of using Anyscale's Mistral models' function calling API to interface with external tools and APIs via Portkey.&#x20;

The usual procedure of leveraging function calling API:

1. Your app can utilize an LLM chat completions call with an optional `tools` property, encompassing specifications and labeled _functions_. The response from LLMs aligns with the defined specification, serving as ideal arguments for methods and functions within the application logic.
2. This allows for your app to extract of entities from text, the creation of structured outputs, to integrate with other systems and tools, such as retrieving data from databases or APIs, and interacting with external tools and APIs.&#x20;
3. Integrate the data with an LLM chat completions call to produce a natural language output for the user or proceed with additional steps in your process.

For example, when building a weather app, you can use JSON web requests to obtain real-time temperature information from a third-party web API. Once powered by LLM, the app can understand and respond to user queries in plain English, while the Function Calling API can streamline interactions with the third-party web API service.

<img src="../.gitbook/assets/file.excalidraw.svg" alt="" class="gitbook-drawing">

## Generate Function Arguments

To generate function arguments, we call chat completions and define _function_ parameters and descriptions with `tools` argument to Anyscale's `mistralai/Mixtral-8x7B-Instruct-v0.1` model.&#x20;

1. **Import Portkey Client SDK**

{% tabs %}
{% tab title="NodeJS" %}
```javascript
import Portkey from "portkey-ai";

const portkey = new Portkey({
  apiKey: process.env.PORTKEY_API_KEY,
  virtualKey: process.env.ANYSCALE_VIRTUAL_KEY,
});

```
{% endtab %}

{% tab title="Python" %}
```python
from portkey_ai import Portkey

portkey = Portkey(
    api_key="x2trk",
    virtual_key="anyscale-3d8c65"
)
```
{% endtab %}
{% endtabs %}

2. **Declare `messages` and `tools`**

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">// Define the messages
let messages = [
    {"role": "system", "content": "You are helpful assistant."},
    {"role": "user", "content": "What's the weather like in Delhi"}
];

// Define the tools
let tools = [
    {
        "type": "function",
        "function": {
            "name": "get_current_weather",
            "description": "Get the current weather in a given location",
            "parameters": {
                "type": "object",
                "properties": {
<strong>                    "location": {
</strong><strong>                        "type": "string",
</strong><strong>                        "description": "The city and state",
</strong>                    },
<strong>                    "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]},
</strong>                },
<strong>                "required": ["location"],
</strong>            },
        },
    }
];
</code></pre>
{% endtab %}

{% tab title="Python" %}
<pre class="language-python"><code class="lang-python"><strong># Define the messages
</strong>messages = [
    {"role": "system", "content": "You are helpful assistant."},
    {"role": "user", "content": "What's the weather like in Delhi"}
]
# Define the tools
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_current_weather",
            "description": "Get the current weather in a given location",
            "parameters": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "The city and state",
                    },
                    "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]},
                },
                "required": ["location"],
            },
        },
    }
]

</code></pre>
{% endtab %}
{% endtabs %}

You can notice the the a specification of expected argument. In this case, we rely on a external web service APIs to give us real time temperature information, only if the API request is structured as it expects — `{"location": "City Name, Country",  "format": "scale"}`

3. **Initiate chat completions with function calls activated**

{% tabs %}
{% tab title="NodeJS" %}
```javascript
const response = await portkey.chat.completions.create({
  model: "mistralai/Mixtral-8x7B-Instruct-v0.1",
  tools,
  messages,
  tool_choice: "auto", // auto is default, yet explicit
});

console.log(response.choices[0].message)
console.log(response.choices[0].finish_reason) // tool_calls
```
{% endtab %}

{% tab title="Python" %}
```python
response = client.chat.completions.create(
    model="mistralai/Mixtral-8x7B-Instruct-v0.1",
    messages=messages,
    tools=tools,
    tool_choice="auto",  # auto is default, yet explicit
)
print(response.choices[0].message)
```
{% endtab %}
{% endtabs %}

We will see the following response

{% code overflow="wrap" %}
```json
{
    "role": "assistant",
    "content": null,
    "tool_calls": [
        "id": "call_...", 
        "type": "function", 
        "function": {
            "name": "get_current_weather", 
            "arguments": '{\n  "location": "San Fransisco, USA",\n  "format": "celsius"\n}'
        }
    ],
}
```
{% endcode %}

The response tells us that `get_current_weather` function specified in the `tool` is activated and the `arguments` adhere to the specification we provided. Note that the if the _temperature_ is non-zero, the output will be non-deterministic.

You can force the LLM's choice of tools by changing the `tool_choice` value

<table><thead><tr><th width="390">tool_choice</th><th></th></tr></thead><tbody><tr><td><code>auto</code></td><td>LLM automatically chooses relevant tool</td></tr><tr><td><code>none</code></td><td>LLM does not use any tool </td></tr><tr><td><p>Specify<br><br><code>{</code> </p><p><code>"type": "function",</code> </p><p><code>"function":</code></p><p><code>{"name":"get_current_weather"}</code></p><p><code>}</code></p></td><td>LLM chooses the selected function from the list of tools</td></tr></tbody></table>

The `finish_reason` is `tool_calls`  if an chat completions call used tools.

