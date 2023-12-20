---
description: Portkey SDK for JSON Responses with Anyscale's open-source models
---

# JSON Assurance with LLMs

Getting assured JSON responses in your LLM applications using Portkey SDK is easy. This cookbook, the first in the series, will teach you to ensure the JSON schema in response to our prompts from Anyscale's `Mistral-7B-Instruct-v0.1` model using Portkey SDK.

You may have attempted few-shot prompting, function calling, or fine-tuning techniques to obtain a valid JSON response. You may want to ensure that the JSON conforms to your defined schema.

Anyscale Endpoints offers managed API endpoints for open-source LLMs, freeing you from infrastructure worries while building LLM-powered apps. Anyscale has recently [introduced JSON Mode ](https://www.anyscale.com/blog/anyscale-endpoints-json-mode-and-function-calling-features)with the `response_format` option, **allowing you to specify the JSON schema**, which is a step up from current OpenAI's JSON Mode only to ensure valid JSON.&#x20;

{% tabs %}
{% tab title="Nodejs" %}
<pre class="language-javascript" data-overflow="wrap" data-full-width="true"><code class="lang-javascript">// import "portkey-ai" and set up a Anyscale <a data-footnote-ref href="#user-content-fn-1">virtual key</a> 

const chatCompletion = await portkey.chat.completions.create({
  messages: [{
    role: "user",
    content: `As culinary master,provide fried rice recipes in JSON format`
  }],
  model: "mistralai/Mistral-7B-Instruct-v0.1",
  response_format: {
    type: "json_object",
<strong>    schema: {
</strong><strong>      type: "object",
</strong><strong>      properties: {
</strong><strong>        title: {
</strong><strong>          type: "string",
</strong><strong>        },
</strong><strong>        description: {
</strong><strong>          type: "string",
</strong><strong>        },
</strong><strong>        steps: {
</strong><strong>          type: "array",
</strong><strong>        },
</strong><strong>      },
</strong>    },
  },
});

const response = chatCompletion.choices[0].message.content;
</code></pre>
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```javascript
import Portkey from "portkey-ai";

const portkey = new Portkey({
  apiKey: process.env.PORTKEY_API_KEY,
  virtualKey: process.env.ANYSCALE_VIRTUAL_KEY,
});

const messages = [
  {
    role: "system",
    content: `As culinary master, you provide structured recipes in JSON format.`,
  },
  {
    role: "user",
    content: `Suggest me how to make fried rice`,
  },
];

const chatCompletion = await portkey.chat.completions.create({
  messages,
  model: "mistralai/Mistral-7B-Instruct-v0.1",
  response_format: {
    type: "json_object",
    schema: {
      type: "object",
      properties: {
        title: {
          type: "string",
        },
        description: {
          type: "string",
        },
        steps: {
          type: "array",
        },
      },
    },
  },
});

const response = chatCompletion.choices[0].message.content;

console.log(response);
```
{% endcode %}
{% endtab %}
{% endtabs %}

<table data-header-hidden><thead><tr><th></th><th></th><th data-hidden></th><th data-hidden></th></tr></thead><tbody><tr><td><code>response_format</code></td><td>An object specifying the format that the model must output.</td><td>senEeT</td><td>afdsafds</td></tr><tr><td><code>schema</code></td><td>An object specifies expected properties in a JSON response and their corresponding data types.</td><td></td><td></td></tr><tr><td><code>model</code></td><td>ID of the model to use. Find the list of <a href="https://portkey.ai/docs/welcome/integration-guides/anyscale-llama2-mistral-zephyr#list-of-models-supported">supported models</a>.</td><td>safds</td><td></td></tr></tbody></table>

The response will adhere to the schema

{% code overflow="wrap" %}
```json
{
  "title": "Fried Rice with Chicken and Vegetables",
  "description": "A delicious recipe for fried rice that includes chicken and a mix of colorful vegetables. Perfect for a healthy and satisfying meal. yum yum yum yum yum",
  "steps": [
    "1. Cook rice and set aside",
    "2. In a large skillet or wok, sauté sliced chicken in oil until browned",
    "3. Add diced bellpepper, carrot, onion, and frozen peas to the skillet and stir fry for 2-3 minutes until vegetables are soft",
    "4. Add cooked rice to the skillet and mix well with the chicken and vegetables",
    "5. Stir in soy sauce, sesame oil, and minced garlic and ginger for flavor",
    "6. Cook for another 2-3 minutes until rice is heated through and slightly crispy",
    "7. Serve and enjoy!"
  ]
}
```
{% endcode %}

{% hint style="success" %}
Portkey SDK provides uniform code interfaces consistent with OpenAI SDK, allowing seamless switching between LLM providers without the need for provider-specific adjustments.
{% endhint %}

### Playground

You can achieve the best possible prompt for your use case by confidently experimenting with various JSON schema and LLM parameters using the Portkey prompt playground.

Here is how the user interface looks like.

<figure><img src="../.gitbook/assets/JSON Prompt UI.png" alt=""><figcaption><p>A Glimpse into the Portkey Prompt Playground</p></figcaption></figure>

Note, that parameters are available to experiment with based on LLM supported parameters.And that's it! You're now equipped to start using JSON response format for your prompts to LLMs.&#x20;

\<Should I invite what's next in anyscale + portkey series>

As you continue to deploy cutting-edge LLM applications in production, we warmly [invite](https://discord.gg/DD7vgKK299) you to join LLMs in Production community.&#x20;

[^1]: Portkey allows you to manage multiple API keys through virtual keys securely.&#x20;
