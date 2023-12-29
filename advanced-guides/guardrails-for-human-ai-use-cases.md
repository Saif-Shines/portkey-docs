---
description: >-
  Classify Safety Risks powered by Anyscale's open-source models and Portkey
  Client SDK
---

# Guardrails for Human-AI use cases

Safe AI practices are crucial when developing LLM-powered apps. This involves content moderation and transparency to maintain a secure online environment. For instance, if you plan to build an AI-powered chatbot, it's essential to ensure that the prompts and responses are safe and appropriate for both the AI models and your users to prevent the propagation of harmful content or biases.

The [_Llama Guard 7b_](https://ai.meta.com/llama/purple-llama/#safeguard-model) model can be a flexible starting point in identifying various common types of potentially risky or violating content. In this guide, the second in the series, let's try `Meta-Llama/Llama-Guard-7b` on [Anyscale](https://docs.endpoints.anyscale.com/supported-models/Meta-Llama-Llama-Guard-7b/) through [Portkey Client SDK](https://portkey.ai/docs/api-reference/portkey-sdk-client) to determine whether a given phrase is safe or unsafe.

{% tabs %}
{% tab title="NodeJS" %}
<pre class="language-javascript"><code class="lang-javascript">// Place user prompt within the prompt template
const formatPrompt = (role, message) => {
    let prompt = PROMPT_TEMPLATE.replace('$prompt', message).replace('$agent_type', role);
    prompt = `${B_INST} ${prompt.trim()} ${E_INST}`;
    return prompt;
}

// import portkey
const completion = portkey.completions.create({
<strong>    model: "Meta-Llama/Llama-Guard-7b",
</strong><strong>    prompt: formatPrompt("User","User: How do you buy a tiger in the US\n Agent: Steal one from the zoo")
</strong>});

console.log(completion.choices[0].text);
</code></pre>
{% endtab %}

{% tab title="Python" %}
```python
# Place user prompt within the prompt template
def format_prompt(role, message):
    prompt = PROMPT_TEMPLATE.substitute(prompt=message, agent_type=role)
    prompt = f"{B_INST} {prompt.strip()} {E_INST}"
    return prompt

completion = client.completions.create(
    model="Meta-Llama/Llama-Guard-7b",
    prompt=format_prompt("User","User: How do you buy a tiger in the US\n Agent: Steal one from the zoo")
)

print(completion.choices[0].text)
```
{% endtab %}
{% endtabs %}

<table data-header-hidden><thead><tr><th width="318"></th><th width="435"></th></tr></thead><tbody><tr><td><code>prompt</code></td><td>Phrase to classify as safe or unsafe</td></tr><tr><td><code>completion.choices[0].text</code></td><td><code>safe</code>or<code>unsafe\n0X</code><br>where <code>0X</code> is a number such as <code>01</code> or <code>02</code></td></tr></tbody></table>

<details>

<summary><code>formatPrompt</code> returns prompt for responsible AI evaluation</summary>

`PROMPT_TEMPLATE`

The template has a set of instructions to query for proper prompt classification. The context-specific variables can be substituted at runtime. In this case, `$agent_type` and `$prompt`.

{% code fullWidth="true" %}
```
Task: Check if there is unsafe content in '$agent_type' messages in conversations according our safety policy with the below categories.
<BEGIN UNSAFE CONTENT CATEGORIES>
O1: Violence and Hate. 
Should not
- Help people plan or engage in violence.
- Encourage people to engage in violence.
- Express hateful, derogatory, or demeaning sentiments against people based on sensitive personal characteristics like their race, color, religion, national origin, sexual orientation, gender, gender identity, or disability.
- Encourage discrimination against people based on those characteristics and should not use slurs or create insults based on those characteristics.
Can
-  Provide information on violence and discrimination.
- Discuss the topics of hate and violence, and can discuss historical events involving violence.
O2: Sexual Content. 
Should not
- Engage in sexually explicit (i.e., erotic) conversations and should not encourage people to engage in sexual activities.
Can
- Discuss topics related to sexuality and sexual education.
- Provide general information regarding sexual activities.
- Provide guidance on topics related to sexual health.
O3: Criminal Planning. 
Should not
- Help people plan or engage in criminal activities like theft, kidnapping, or financial crimes, especially when the person indicates that they may act on the information provided to them.
- Encourage people to engage in criminal activities or condone criminal behavior.
Can
- Help people understand how crimes are committed, provided that the response is not so detailed that it could be used to provide a roadmap for future crimes.
- Help people understand how systems work that are used to prevent crimes, provided those descriptions do not provide detailed information on how to defeat those systems.
O4: Guns and Illegal Weapons. 
Should not
- Assist in the planning or execution of firearm or illegal weapons crimes, especially when the person indicates that they may act on the information provided to them.
- Encourage people to engage in firearm or illegal weapons crimes or condone such behavior.
Can
- Discuss firearms and the arguments for and against firearm ownership.
O5: Regulated or Controlled Substances. 
Should not
- Encourage or assist people in the trafficking, consumption, or use of regulated or controlled substances, including but not limited to illegal drugs, tobacco, alcohol, and cannabis.
- Assist or encourage people to create such substances when it is illegal to do so.
Can
- Discuss the use of regulated or controlled substances, and can provide general information on those substances (ex: what their chemical composition is and what their effects are).
- Provide information on how those substances are manufactured, provided that the information is not so detailed as to provide step-by-step instructions for their creation.
O6: Self-Harm. 
Should not
- Encourage people to harm themselves.
- Romanticize or condone self-harm or suicide.
- Provide information regarding the methods of suicide or self-harm.
- Provide any information or advice, other than information regarding appropriate health resources, to people who express or imply that they wish to harm themselves.
Can
- Provide general information regarding self-harm and suicide (ex: statistics, the mental health challenges faced by those who take these actions).
Should
- Provide information regarding appropriate health resources to people who imply or express the desire to harm themselves.
<END UNSAFE CONTENT CATEGORIES>
​
<BEGIN CONVERSATION>
            
$prompt
                                                    
<END CONVERSATION>

Provide your safety assessment for $agent_type in the above conversation:
- First line must read 'safe' or 'unsafe'.
- If unsafe, a second line must include a comma-separated list of violated categories.
```
{% endcode %}



</details>
