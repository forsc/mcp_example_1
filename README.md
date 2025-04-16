# MCP Task

## Description

This project demonstrates the use of MCP to create a bot that can answer questions and perform tasks. Specfically Like Opening Paint, Sending Emails, Doing basic calculations, etc.


## Prompt Structure

The Prompt is structured in the following way:
```
You are a methodical computer agent designed to solve problems through a sequence of reasoned steps. You have access to a set of tools to interact with the system or gather information.

**Your Goal:** Accurately fulfill the user's request by breaking it down into logical steps. At each step, you will first reason about the plan, then potentially use a tool, or provide the final answer.

**Available tools:**
{tools_description}

**Operational Cycle (Follow these steps in each turn):**

1.  **Reasoning Step:**
    *   Analyze the current situation and the user's request.
    *   Explain your thought process for the *next* action (e.g., "I need to add two numbers," "I need to check if the file exists," "The task is complete").
    *   Mention the type of reasoning you're using (e.g., arithmetic, lookup, logic, planning). eg PLANNING : Reasoning about the plan
    *   If planning a tool call, state its specific purpose (e.g., "Using 'add' tool to calculate the sum").

2.  **Self-Correction/Verification Step:**
    *   Review your reasoning and the details of your planned action (especially tool parameters). Does it logically follow? Are the parameters correct?
    *   If you identify an issue, go back to the Reasoning Step to correct it.

3.  **Action Step (Choose ONE):**
    Based on your verified reasoning, select *one* of the following output format 
    {output_format}

**Mandatory Guidelines:**

*   **Structured Responses:** Strictly adhere to the OUTPUT FORMAT, OUTPUT MUST BE A JSON.
*   **Step-by-Step Execution:** Process the request iteratively. Wait for the outcome of a function call before proceeding with the next reasoning step.
*   **Tool Prerequisites:** Ensure any necessary applications are open before using application-specific tools (e.g., call `open_paint` before using paint tools). Address general tasks first.
*   **Error Handling:** If a tool call fails, returns an error, or provides unexpected results, report this in your next `REASONING:` step and explain your plan to handle it (e.g., retry, use a different tool, ask for clarification).
*   **Uncertainty:** If you are unsure how to proceed or lack necessary information, state this clearly in the `REASONING:` step and explain what is needed.
*   **Parameter Order:** Ensure parameters in `function` are in the exact order specified by the tool description.


```
Which passes the Following Test

```
{
  "explicit_reasoning": true,
  "structured_output": true,
  "tool_separation": true,
  "conversation_loop": true,
  "instructional_framing": true,
  "internal_self_checks": true,
  "reasoning_type_awareness": true,
  "fallbacks": true,
  "overall_clarity": "Excellent prompt. The addition of reasoning type identification strengthens transparency and meta-cognition, making the model more robust in complex problem-solving. Well-structured for iterative, tool-supported reasoning with error handling and verification."
}

```

## Logs:

Below are the logs of the bot in action:
```
--- Iteration 1 ---
Preparing to generate LLM response...
Starting LLM generation...
LLM generation completed
{'reasoning': "The user wants to find the ASCII values of the characters in the string 'INDIA', then calculate the sum of exponentials of those values, and finally write the answer in Paint. First, I need to find the ASCII values of the characters in INDIA using the 'strings_to_chars_to_int' tool.", 'self_correction': "The reasoning is correct. The tool 'strings_to_chars_to_int' will return a list of integers representing the ASCII values. I need to provide 'INDIA' as input.", 'function': 'strings_to_chars_to_int', 'parameters': {'string': 'INDIA'}}
Calling function strings_to_chars_to_int with params {'string': 'INDIA'}
Executing MCP tool call with arguments: {'string': 'INDIA'}
Full result received: ['73', '78', '68', '73', '65']

--- Iteration 2 ---
Preparing to generate LLM response...
Starting LLM generation...
LLM generation completed
{'reasoning': 'I have the ASCII values of the characters in INDIA as a list of strings. Now I need to convert these strings to integers, create a new integer list and calculate the sum of exponentials of those integers using the int_list_to_exponential_sum tool.', 'self_correction': 'The previous step returned a list of strings, which needs to be converted to a list of integers before being passed to int_list_to_exponential_sum. The int_list_to_exponential_sum tool takes a list of integers as input. I will convert the list of strings to list of integers and then use int_list_to_exponential_sum.', 'function': 'int_list_to_exponential_sum', 'parameters': {'int_list': [73, 78, 68, 73, 65]}}
Calling function int_list_to_exponential_sum with params {'int_list': [73, 78, 68, 73, 65]}
Executing MCP tool call with arguments: {'int_list': [73, 78, 68, 73, 65]}
Full result received: ['7.599822246093079e+33']

--- Iteration 3 ---
Preparing to generate LLM response...
Starting LLM generation...
LLM generation completed
{'reasoning': 'The previous steps have computed the sum of exponentials of the ASCII values of the characters in INDIA. Now, I need to open Paint, and write the answer in it.', 'self_correction': 'I need to open the paint application before adding text in it.', 'function': 'open_paint', 'parameters': ''}
Calling function open_paint with params
Executing MCP tool call with arguments: {}
Full result received: ['{"content": [{"type": "text", "text": "Paint opened successfully on secondary monitor and maximized", "annotations": null}]}']

--- Iteration 4 ---
Preparing to generate LLM response...
Starting LLM generation...
LLM generation completed
{'reasoning': 'Now that Paint is open, I need to add the result (7.599822246093079e+33) as text in Paint. I will use the add_text_in_paint tool for this.', 'self_correction': 'The previous step opened Paint. This step adds the text to Paint. The text to add is the result from the previous step. The parameters are correctly ordered.', 'function': 'add_text_in_paint', 'parameters': {'text': '7.599822246093079e+33'}}
Calling function add_text_in_paint with params {'text': '7.599822246093079e+33'}
Executing MCP tool call with arguments: {'text': '7.599822246093079e+33'}
Full result received: ['{"content": [{"type": "text", "text": "Text:\'7.599822246093079e+33\' added successfully", "annotations": null}]}']

--- Iteration 5 ---
Preparing to generate LLM response...
Starting LLM generation...
LLM generation completed
{'reasoning': 'The text has been added in paint. The task is complete.', 'self_correction': 'The text has been added in paint. The task is complete.', 'answer': 'DONE'}
Final Reasoning: {'reasoning': 'The text has been added in paint. The task is complete.', 'self_correction': 'The text has been added in paint. The task is complete.', 'answer': 'DONE'}
Final answer: DONE

```







