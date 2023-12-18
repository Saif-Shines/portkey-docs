# Enable JSON responses

JSON is one of the popular notions for the applications to talk to each other since the request & response is structured. Many developers would go on to use techniques such as _few shot examples, function calling,_ and _fine-tuning_ their LLMs to optimize for JSON responses. Recently, OpenAI has provided this option exclusively.&#x20;

If you use an open-source model, you won't fall behind. Anyscale's open-source models now support JSON mode through Portkey SDK.

#### Prerequisites

{% content-ref url="../api-reference/portkey-sdk-client.md" %}
[portkey-sdk-client.md](../api-reference/portkey-sdk-client.md)
{% endcontent-ref %}

{% content-ref url="../getting-started/integration-guides/anyscale-llama2-mistral-zephyr.md" %}
[anyscale-llama2-mistral-zephyr.md](../getting-started/integration-guides/anyscale-llama2-mistral-zephyr.md)
{% endcontent-ref %}

***

Import the Portkey SDK Client and set an environment variable to the Anyscale's [virtual key](../api-reference/virtual-keys.md). The keys should be passed as an argument to initialise the Portkey class as follows:

{% tabs %}
{% tab title="Nodejs" %}
{% code fullWidth="true" %}
```javascript
import Portkey from "portkey-ai";

const portkey = new Portkey({
  apiKey: process.env.PORTKEY_API_KEY,
  virtualKey: process.env.ANYSCALE_VIRTUAL_KEY,
});
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
from portkey_ai import Portkey
import os

portkey = Portkey(api_key=os.getenv('PORTKEY_API_KEY'), virtual_key=os.getenv('ANYSCALE_VIRTUAL_KEY'))
```
{% endcode %}
{% endtab %}
{% endtabs %}

When making an chat completions call, pass an additional property as follows to enable the JSON response from the Mistral AI model.

{% tabs %}
{% tab title="Nodejs" %}
{% code overflow="wrap" fullWidth="true" %}
```javascript
const messages = [
  {
    role: "system",
    content: `As culinary master, you provide structured recipes (title, description, steps) in JSON format.`,
  },
  {
    role: "user",
    content: `Suggest me how to make fried rice`,
  },
];

const chatCompletion = await portkey.chat.completions.create({
  messages,
  model: "mistralai/Mixtral-8x7B-Instruct-v0.1",
  response_format: {
    type: "json_object",
  }
});
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
messages = [
    {
        "role": "system",
        "content": "As culinary master, you provide structured recipes (title, description, steps) in JSON format."
    },
    {
        "role": "user",
        "content": "Suggest me how to make fried rice"
    }
]

chat_completion = portkey.chat.completions.create(
    messages=messages,
    model="mistralai/Mixtral-8x7B-Instruct-v0.1",
    response_format={
        "type": "json_object"
    }
)
```
{% endcode %}
{% endtab %}
{% endtabs %}

<table data-header-hidden><thead><tr><th></th><th></th><th>Required</th><th data-hidden></th><th data-hidden></th></tr></thead><tbody><tr><td><code>response_format</code></td><td>An object specifying the format that the model must output.</td><td>Optional</td><td>senEeT</td><td>afdsafds</td></tr><tr><td><code>model</code></td><td>ID of the model to use. Find the list of <a href="https://portkey.ai/docs/welcome/integration-guides/anyscale-llama2-mistral-zephyr#list-of-models-supported">supported models</a>.</td><td>&#x3C;Yes?></td><td>safds</td><td></td></tr></tbody></table>

<< work in progress - playground & logs | feels optional >>

Incase you have more questions, we are up for a discussion along with Anyscale team in the _LLMs in Production_ community. [Join us](https://discord.gg/DD7vgKK299)!
