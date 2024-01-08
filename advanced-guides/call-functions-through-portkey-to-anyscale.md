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

# Call functions through portkey to Anyscale

This cookbook will guide you through the process of using Anyscale's Mistral models' function calling API to interface with external tools and APIs via Portkey.&#x20;

For example, if you are building a weather app, you may make JSON web requests to 3rd party web API service to get the temperature information. An LLM-powered application would be capable of comprehending and replying to user queries in plain English, enhancing the user experience and making the interaction more natural and intuitive.

<img src="../.gitbook/assets/file.excalidraw.svg" alt="" class="gitbook-drawing">

The usual procedure of leveraging function calling API:

1. Your app can utilize an LLM chat completions call with an optional `tools` property, encompassing specifications and labeled _functions_. The response from LLMs aligns with the defined specification, serving as ideal arguments for methods and functions within the application logic.
2. This allows for your app to extract of entities from text, the creation of structured outputs, to integrate with other systems and tools, such as retrieving data from databases or APIs, and interacting with external tools and APIs.&#x20;
3. Integrate the data with an LLM chat completions call to produce a natural language output for the user or proceed with additional steps in your process.





