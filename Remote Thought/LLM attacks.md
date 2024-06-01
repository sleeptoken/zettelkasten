
2024-06-01 09:27

Source: #portswigger 

Tags: [[web]] 

Some LLMs may require the client to call a separate function endpoint (effectively a private API) in order to generate valid requests that can be sent to those APIs. 

    - The client calls the LLM with the user's prompt.
    - The LLM detects that a function needs to be called and returns a JSON object containing arguments adhering to the external API's schema.
    - The client calls the function with the provided arguments.
    - The client processes the function's response.
    - The client calls the LLM again, appending the function response as a new message.
    - The LLM calls the external API with the function response.
    - The LLM summarizes the results of this API call back to the user.

excessive agency - a situation in which an LLM has access to APIs that can access sensitive information and can be persuaded to use those APIs unsafely

APIs that send emails sometimes use operating system commands that offer a pathway to RCE.
if the API takes email as an argument then try injecting commands in the username part of the email ID for example - $(whoami)@gmail.com



### References
https://portswigger.net/web-security/learning-paths/llm-attacks