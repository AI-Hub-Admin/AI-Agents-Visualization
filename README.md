
# AI Agents Visualization Tutorials

## OpenAI Swarm AI Agents Workflow Orchestrating Visualization

[OpenAI Swarm Github](https://github.com/openai/swarm) <br>
[OpenAI Swarm Agents Session Json](https://github.com/openai/swarm/blob/main/examples/customer_service_lite/logs/session_20240425-175112.json) <br>

#### Visualized Output <br>

![Swarm Agents Visualization in ChatGPT style](https://github.com/AI-Hub-Admin/AI-Agents-Visualization/blob/main/static_files/swarm_dialogue_visualization_1.jpg?raw=true)


## 1. Introduction to AI Agents Swarm Framework

### 1.1 OpenAI AI Orchestrating Agents Swarm and AI Agents Logs Dialogue Visualization

OpenAI just released a AI Agent package Swarm (https://github.com/openai/swarm) and highlight two concepts Routines and Handoffs when orchestrating multiple AI Agents. In their blog post, Routines is defined as a list of instructions in natural langauge for an agent. And handoff is defined as an agent handing off an active conversation to another agent. And in their blog post, they give an example of a customer seeking Sales and Refund process, which happens in a conversation among a customer and two AI agents (Sales Agent and and Refund Agent). When the task is getting more and more complicated, the conversation and messages are getting more large and there is a need to visualize and debug the session logs often in json format. In this blog, we will use the Swarm package to generate the AI Agents session logs and DeepNLP AI Agents Visualization Tool (http://www.deepnlp.org/workspace/dialogue_visualization) to visualize and debug the session logs. 


Let's take a session log json file in github repo for example. The session logs contains 9 conversation messages, wrapped in a long json file as below.

https://github.com/openai/swarm/blob/main/examples/customer_service_lite/logs/session_20240425-175112.json

You can generate the session logs by running the test examples (https://github.com/openai/swarm/tree/main/examples)


The session log contains 9 conversation messages, wrapped in a long json file as below.

```
[{"task_id": "8870de5c-b121-4d76-94c4-be39ff75823c", "role": "user", "content": "What is the square root of 16?"}, {"task_id": "8870de5c-b121-4d76-94c4-be39ff75823c", "role": "assistant", "content": "Response to user: 4"}, {"task_id": "8ded2126-0899-4bba-a136-36f9ddc003fd", "role": "user", "content": "Is phone verification required for new OpenAI account creation or ChatGPT usage"}, {"task_id": "8ded2126-0899-4bba-a136-36f9ddc003fd", "role": "user", "tool": {"tool": "query_docs", "args": {"query": "Is phone verification required for new OpenAI account creation or ChatGPT usage"}}}, {"task_id": "8ded2126-0899-4bba-a136-36f9ddc003fd", "role": "assistant", "content": "The task failed because the system was unable to execute the necessary tool to determine if phone verification is required for new OpenAI account creation or ChatGPT usage."}, {"task_id": "a4c57692-d131-45b1-8ac8-69023d2b5d34", "role": "user", "content": "How many free tokens do I get when I sign up for an OpenAI account? Send an email to me@gmail.com containing that answer"}, {"task_id": "a4c57692-d131-45b1-8ac8-69023d2b5d34", "role": "user", "tool": {"tool": "query_docs", "args": {"query": "free tokens for new OpenAI account"}}}, {"task_id": "a4c57692-d131-45b1-8ac8-69023d2b5d34", "role": "user", "tool": {"tool": "submit_ticket", "args": {"description": "Inquiring about the number of free tokens received when signing up for an OpenAI account."}}}, {"task_id": "a4c57692-d131-45b1-8ac8-69023d2b5d34", "role": "assistant", "content": "The task was to find out and communicate the number of free tokens given with a new OpenAI account. Although a ticket was created to inquire about this, the actual information was not obtained or relayed via email as requested."}]

```


The json is from the "messages" variable which keep track of multi-turn dialogue or activities list. The data format of the json is a list of dict, which contains some important keys, including


| KEY | Type |  Explaination    |
| ---- | ----- | ----- |
| task_id | string |  message.tool_calls list of tool_call.id  |
| role | string |  user or assistant, system, etc.  |
| content | string |  natural language of a message  |
| tool | map   | OpenAI API responded {"tool": "query_docs", "args": {"arg1": "value1"}  |


To visualize the logs, you can use copy and paste the json files to the online website (http://www.deepnlp.org/workspace/dialogue_visualization). 

1. Choose the template as OpenAI (You can also try other template such as Whatsapp)
2. Choose the "JSON" input mode rather than the GUI mode.
2. Copy your json data in the textbox and Click "Run"

And the complete AI Agents running process is displayed clearly as:


![Swarm Agents Visulization in ChatGPT format](https://github.com/AI-Hub-Admin/AI-Agents-Visualization/blob/main/static_files/swarm_dialogue_visualization_1.jpg?raw=true)



You can also change to the whatsapp style as displayed:


![Swarm Agents Visulization in Whatsapp format](https://github.com/AI-Hub-Admin/AI-Agents-Visualization/blob/main/static_files/swarm_dialogue_visualization_2.jpg?raw=true)


The generated messages are from swarm package by calling the swarm package multiple OpenAI API multiple times.


#### Complicated Example

Instead of Returning a Response or Message, This function return a new Agent as the transfered Agent to continue the action.


```
from swarm import Swarm, Agent

client = Swarm()

english_agent = Agent(
    name="English Agent",
    instructions="You only speak English.",
)

spanish_agent = Agent(
    name="Spanish Agent",
    instructions="You only speak Spanish.",
)


def transfer_to_spanish_agent():
    """Transfer spanish speaking users immediately."""
    return spanish_agent


english_agent.functions.append(transfer_to_spanish_agent)

messages = [{"role": "user", "content": "Hola. ¿Como estás?"}]
response = client.run(agent=english_agent, messages=messages)

print(response.messages[-1]["content"])

```



### 1.2 Introduction to DeepNLP AI Agents Visualization Tool

You can just use the messages json format, or put the list in the field "messages" and customize the avatars of the conversation.



| KEY | Type |  Explaination    |
| ---- | ----- | ----- |
| agent | dict |  {"${agentid}": {"id": "${agentid}", "avatar": ""}} |
| agent.${id}.id | str |  ID of the agent |
| agent.${id}.avatar | string |  Url of the avatar of the agent |
| messages | list | list of ${message} json, such as the output of swarm package. You can also just use the messages and default avatars |
| messages.message | dict | detailed information of a message |
| messages.message.role | string | The role of the message, either user(human) or assistant(ai,system) |
| messages.message.content | string | natural language of response |
| messages.message.id | string | the id of the agent who generate the response |
| messages.message.timestamp | string | the timestamp of agent generating the message |
| dialogue_title |  string |The title of the conversation displayed  |
| dialogue_background_url | string |  The background url image of the conversation displayed  |


The complete input json format is

```
{
  "agent": {
    "Rebecca": {
      "id": "Rebacca",
      "avatar": "/scripts/img/dialogue_visualization/icon_female_1.png"
    },
    "Shirley": {
      "id": "Shirley",
      "avatar": "/scripts/img/dialogue_visualization/icon_female_2.webp"
    },
    "Human": {
      "id": "Human",
      "avatar": "/scripts/img/dialogue_visualization/male_icon_1.webp"
    }
  },
  "messages": [
    {
      "timestamp": "2024-09-19 17:06:11",
      "id": "Rebecca",
      "content": "This is first message",
      "role": "ai"
    },
    {
      "timestamp": "2024-09-19 17:06:19",
      "id": "Shirley",
      "content": "This is second message",
      "role": "ai"
    },
    {
      "timestamp": "2024-09-19 17:07:11",
      "id": "Human",
      "content": "This is first message from human",
      "role": "human"
    }
  ],
  "dialogue_title": "Chat",
  "dialogue_background_url": ""
}
```

### Reference

[Swarm Github Repo](https://github.com/openai/swarm) <br>
[Dialogue AI Agents Json Visualization Tool](http://www.deepnlp.org/workspace/dialogue_visualization) <br>
[Asynchronous Multi Agent Simulation](http://www.deepnlp.org/workspace/agent_visualization) <br>
[OpenAI Cookbook Orchestrating Agents](https://cookbook.openai.com/examples/orchestrating_agents) <br>
[Dialogue Agent Visualization Tools for AI systems](http://www.deepnlp.org/blog/dialogue-agent-multimodal-visualization-tools-for-ai-systems) <br>
[AI Agents Visualization Reviews Asynchronous Multi Agent Simulation](http://www.deepnlp.org/blog/ai-agent-visualization-review-asynchronous-multi-agent-simulation) <br>

