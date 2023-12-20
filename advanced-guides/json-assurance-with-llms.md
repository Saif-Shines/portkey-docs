---
description: Portkey SDK & AnyScale
---

# JSON Assurance with LLMs

Getting assured JSON responses in your LLM applications using Portkey SDK is easy. This cookbook will teach us to ensure the JSON schema in response to our prompts from Anyscale's `Mistral-7B-Instruct-v0.1` model using Portkey SDK.

You may have attempted few-shot prompting, function calling, or fine-tuning techniques to obtain a valid JSON response. You may want to ensure that the JSON conforms to your defined schema.

Anyscale Endpoints offers managed API endpoints for open-source LLMs, freeing you from infrastructure worries while building LLM-powered apps. Anyscale has recently introduced an extension to the `response_format` option, allowing you to specify the JSON schema, which is a step up from current OpenAI's JSON Mode.

{% tabs %}
{% tab title="Nodejs" %}
<pre class="language-javascript" data-overflow="wrap"><code class="lang-javascript">// import "portkey-ai" and set up a Anyscale <a data-footnote-ref href="#user-content-fn-1">virtual key</a> 

const chatCompletion = await portkey.chat.completions.create({
  messages,
  model: "mistralai/Mistral-7B-Instruct-v0.1",
<strong>  response_format: {
</strong><strong>    type: "json_object",
</strong><strong>    schema: {
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
</strong><strong>    },
</strong><strong>  },
</strong>});

const response = chatCompletion.choices[0].message.content;

console.log(response);
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



<table data-header-hidden><thead><tr><th></th><th></th><th>Required</th><th data-hidden></th><th data-hidden></th></tr></thead><tbody><tr><td><code>response_format</code></td><td>An object specifying the format that the model must output.</td><td>Optional</td><td>senEeT</td><td>afdsafds</td></tr><tr><td></td><td></td><td></td><td></td><td></td></tr><tr><td><code>model</code></td><td>ID of the model to use. Find the list of <a href="https://portkey.ai/docs/welcome/integration-guides/anyscale-llama2-mistral-zephyr#list-of-models-supported">supported models</a>.</td><td>Yes</td><td>safds</td><td></td></tr></tbody></table>

#### Using playground

Experiment with different prompts in the Portkey playground to identify the one that best suits your app. The playground will display an option for Response Format for all the supported models.&#x20;

<figure><img src="../.gitbook/assets/JSON Prompt UI.png" alt=""><figcaption></figcaption></figure>

And that's it! You're now equipped to start using JSON response format for your prompts to LLMs. Get out there and try playing around this feature! Incase you have more questions, we are up for a discussion along with Anyscale team in the _LLMs in Production_ community. [Join us](https://discord.gg/DD7vgKK299)!

[^1]: Portkey allows you to manage multiple API keys through virtual keys securely.&#x20;
