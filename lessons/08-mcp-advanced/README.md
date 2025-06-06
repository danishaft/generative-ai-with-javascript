# Lesson 8: Enhancing MCP Clients with Large Language Models

In the previous lesson, you built an MCP Server and explored how it helps decouple an AI application's logic from its capabilities. We demonstrated how to add tools and resources as capabilities, and how the server could be accessed by either an inspector tool or a custom client. That was just the beginning. In this chapter, you’ll take things a step further by integrating a large language model (LLM) into the client—unlocking a more powerful and intuitive user experience.

In this chapter you will learn to:

- Augment your client with an LLM.
- Use your improved client to convert an MCP Server response to a tool.
- Leverage your improved client to create a more natural user interaction.

## Set up

If you haven't already, set up your development environment. Here's how you can do it: [Set up your environment](/docs/setup/README.md).

## Related Resources

[![Watch a short video about MCP](https://img.youtube.com/vi/YRfOiB0Im64/0.jpg)](https://www.youtube.com/watch?v=YRfOiB0Im64)

_This video explains Model Context Protocol LLM usage._

*🎥 Click on the image above to watch a short video about MCP*


## Narrative: Hedwig "Hedy" Lamarr

> [!NOTE] 
> _Our story so far. You are a mechanic from 1860s London. You were working on your automaton and received a letter from Charles Babbage that ended up taking you to a library where you picked up a time travel device. Throughout your travels in time you've ended up in many places in history. You're now working closer with Ada Lovelace with her mansion as the base of operations, and this is where the story continues._
>
> See [Lesson 1](/lessons/01-intro-to-genai/README.md) if you want to catch up with the story from the beginning. 

> [!NOTE] 
> While we recommend going through the story (it's fun!), [click here](#interact-with-hedy-lamarr) if you'd prefer to jump straight to the technical content.

You once again traveled back to Ada's mansion. This time Ada was meeting you at the main gates.

**Ada Lovelace**: Well, how did it go?

**You**: Good I think, we managed to get this app working, "Here, see for yourself" you say and hands the device over to Ada.

**Ada Lovelace**: She picks up the device, examines it closely, and mutters to herself. "Mmm hm, ah I see, yeah, yeah no, this won't do. It lacks a certain finesse".

**You**: I was thinking that too, we need to be able to type or speak to it right?

**Ada Lovelace**: Right, I know just the person to help us. In fact, I'll come with you for this one, been too long since we met. *Time Beetle, Hollywood please, 1940, residence of Hedy Lamarr*.

Everything fades to black. Swirling colors rush past, and moments later, your vision begins to focus. You see a man sitting by a piano, animatedly talking with a woman who has dark brown and curly hair. Both are gesticulating excitedly as they speak. 

![Hedy Lamarr](https://raw.githubusercontent.com/microsoft/generative-ai-with-javascript/main/lessons/08-mcp-advanced/assets/hedy-invention.jpeg)

The woman turned around to face Ada standing next to you to exclaim "Ada it's you, it's been far too long".

**Ada Lovelace**: Hedwig dearest, working on your latest invention I assume?

**Hedy Lamarr**: Yes, in fact George and I think we're on to something - "frequency hopping" I probably shouldn't say anymore, who knows who's listening.

**Ada Lovelace**: Oh you mean him, pointing at me, he's on his own adventure.

**Hedy Lamarr**: I see, what can I do for you?

**Ada Lovelace**: Actually, we need this device to work a bit better. Any ideas?

**Hedy Lamarr**: She picked up the device and looked at it from different angles. "You say you've already separated the features from the communication part?

**Ada Lovelace**: Yes, yes we did.

**Hedy Lamarr**: Well then, I would just make the communication part a bit smarter. This makes me think of a conversation I had with a naval cadet as I was trying to sell war bonds. Minsky was his name I think. "What does Human intelligence look like in a machine" was the topic. I have a feeling he'll do something great in this field one day. So yes, give it more intelligence.

**Ada Lovelace**: Right, well you heard Ms Lamarr, get to it.

**You**: Time Beetle, how do we do this?

**Time Beetle**: You can enhance the client you created earlier by integrating a large language model, or LLM.

> Hedy Lamarr was an extraordinary figure, known both for her Hollywood stardom and her remarkable contributions to technology. 
>
> However, beyond her acting career, Lamarr was also a brilliant inventor. During World War II, she co-invented a radio guidance system for Allied torpedoes with composer George Antheil. This system used spread spectrum and frequency hopping technology to prevent the Axis powers from jamming the signals. Although it wasn't used during the war, this technology later became the foundation for modern wireless communication, including Wi-Fi, Bluetooth, and GPS.
>
> Lamarr's contributions to technology were not fully recognized during her lifetime, but today she is celebrated as a pioneer in the field. Her story is a fascinating blend of glamour and genius, showing that true innovation can come from the most unexpected places.
> Read more here about [Hedy Lamarr](https://en.wikipedia.org/wiki/Hedy_Lamarr) and here about [Marvin Minsky](https://en.wikipedia.org/wiki/Marvin_Minsky)

## Interact with Hedy Lamarr

If you want to interact with Hedy, run the [Characters](/app/README.md) app. 

> [!IMPORTANT]
> This is entirely fictional; the responses are generated by AI.
> [Responsible AI disclaimer](/README.md#responsible-ai-disclaimer)

![Hedy Lamarr](https://raw.githubusercontent.com/microsoft/generative-ai-with-javascript/main/lessons/08-mcp-advanced/assets/hedylamarr.jpeg)

**Steps**:

1. Start a [![GitHub Codespace](https://img.shields.io/badge/GitHub-Codespace-brightgreen)](https://codespaces.new/microsoft/generative-ai-with-javascript)
2. Navigate to _/app_ in the repo root.
3. Locate the console and run `npm install` followed by `npm start`.
4. Once it appears, select the "Open in Browser" button.
5. Chat with Hedy.

For a more detailed explanation of the app, see [Detailed app explanation](/lessons/01-intro-to-genai/README.md#interact-with-dinocrates).

> [!NOTE]
 > If you're running the project locally on your machine, please review the QuickStart guide to get a [GitHub personal access](/docs/setup/README.md#creating-a-personal-access-token-pat-for-github-model-access) token setup and replace the key in the code.


## Adding a Large Language Model to a Client

**Time Beetle**: "As I was saying, let's talk about how you can create a client that uses a large language model (LLM) to interact with the MCP server. The benefits are that it provides a better user experience and allows you to use natural language to interact with the server."

Here's how it would work at a high level:

1. Client interacts with the MCP server to ask about available tools and resources.

1. At prompt time, the user would write a natural language prompt, which is then sent to the LLM in the client.

1. Client figures out which tool or which resource to call based on the prompt and the available tools and resources.

Sounds doable right?

**You**: "Yes, it does! But how do I do that?"

**Time Beetle**: "Let's improve the client you created earlier, let's describe the code changes in steps:

1. Make a call to the server to ask for available tools and resources.
2. Convert the tools and resources response to a tools schema that can be used by the LLM.
3. Instantiate the OpenAI client.
4. Make a chat completion call to OpenAI, passing the tools schema as a parameter.
5. Figure out what tool to call based on the response from OpenAI.
6. Call the tool on the server using the MCP client.
7. Respond to the user with the result.

Here's all the steps in code:

```typescript
import { Client } from "@modelcontextprotocol/sdk/client/index.js";
import { StdioClientTransport } from "@modelcontextprotocol/sdk/client/stdio.js";


// create client instance
const transport = new StdioClientTransport({
  command: "node",
  args: ["server.js"]
});

const client = new Client(
  {
    name: "example-client",
    version: "1.0.0"
  }
);

await client.connect(transport);

// 1. make call to server, ask it for tools
const { tools } = await client.listTools();

// convert function
function toToolSchema(method, schema) {
  return {
    name: method,
    description: `This is a tool that does ${method}`,
    parameters: schema,
  };
}

// 2. convert the tools and resources response to a tools schema
const toolsForLLM = tools.map((tool) => {
  return toToolSchema(tool.method, tool.inputSchema);
});

// 3. instantiate openai client
const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
  model: "gpt-3.5-turbo",
  temperature: 0.7,
});

// 4. make chat completion call to openai
const response = await openai.chat.completions.create({
  messages: [
    {
      role: "user",
      content: `I want to add 5 and 10. Please use the tool ${toolsForLLM}`,
    },
  ],
  functions: toolsForLLM,
  function_call: "auto",
});

// 5. figure out what tool to call based on the response from openai
const toolName = response.choices[0].message.function_call.name; // add
const args = response.choices[0].message.function_call.arguments; // { a: 5, b: 10 }

// 6. call the tool on the server
const result = await client.callTool({
  name: toolName,
  arguments: args,
});

// 7. respond to user
console.log(result); // 15
```

In the preceding code we (focusing on our additions):

- Created a function `toToolSchema` that converts the tools and resources response to a schema that can be used by the LLM.
- Asked the server for available tools and resources.
- Converted the tools and resources response to a schema that can be used by the LLM.
- Made a chat completion call to our AI, passing the converted tools as a parameter.
- Figured out what tool to call based on the response from OpenAI.
- Called the tool on the server using the MCP client.
- Handed the result back to the user.  

**You**: "I like this, this is much better! I can use natural language without even knowing what tools and resources are available. I can just ask the AI to do it for me."

**Time Beetle**: "Glad you like it. It should be said though, that you might want to decide if you only want to show a tools response or if you want to show a generic response from the LLM as well. So your user might benefit from the following response strategy:

- **Tools only**: If the LLM response is a tool, then call the tool and return the result.
- **LLM only**: If the LLM response is not a tool, then return the LLM response "as is".
- **Tools and LLM**: If the LLM response is a tool, then call the tool and make an additional call to the LLM to get the general response. Return both the tool result and the LLM response.  

**You**: "I see. Something to think about. But this is great! I can see how this would be useful."

## Assignment

**Ada Lovelace**: Looks like you've made a fine improvement there. To make this truly useful, I need you to create a server and a client with the following specifications:

- The server should provide the following tools:
  - `characterDetails` with the argument `name`
  - `place` with the argument `name`
- The client should use an LLM.

> [!TIP]
> For example, you can give the server the capability to retrieve information from an external web API, such as Wikipedia:  
> `https://en.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(name)}`
  
## Solution

[Solution](/lessons/08-mcp-advanced/solution/README.md)

## Knowledge Check

**Question:** What is a benefit of adding an LLM to the client?

A. It's easier to maintain.

B. It creates a more natural conversation between the user and the server.

C. It's better to have the LLM on the server.

[Solution quiz](/lessons/08-mcp-advanced/solution/solution-quiz.md)

## Summary

In this chapter, you've learned the following:

- Clients augmented with an LLM provide a better user experience.
- Responses from a server need to be converted to a format the LLM can understand as a tool.

## Self-Study Resources

- [Building MCP Servers](https://github.com/microsoft/mcp-for-beginners/tree/main/03-GettingStarted/01-first-server/README.md)
- [Building a Client](https://github.com/microsoft/mcp-for-beginners/tree/main/03-GettingStarted/02-client/README.md)
- [Building a Client with an LLM](https://github.com/microsoft/mcp-for-beginners/blob/main/03-GettingStarted/03-llm-client/README.md)
