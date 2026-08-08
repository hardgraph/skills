> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Single/Multi Prompt Agent Overview

> Choose between Retell single-prompt and multi-prompt agents based on call complexity — simple single prompt or staged prompts for state-driven conversations.

## Introduction

Retell offers two prompt-based approaches for building conversational agents, each suited to different complexity levels and use cases. Understanding when to use each approach is crucial for creating effective AI phone agents.

## Agent Architecture Options

<Frame>
  <img height="300" src="https://mintcdn.com/retellai/__ejvYecdVsguhU2/images/create_agent.png?fit=max&auto=format&n=__ejvYecdVsguhU2&q=85&s=2106bd94b9fb2692c4a8e530c82dc417" alt="Create Agent Interface" data-path="images/create_agent.png" />
</Frame>

1. Single Prompt: Design your agent with one comprehensive prompt
2. Multi-Prompt Tree: Structure your agent with multiple organized prompts

### Single Prompt Agent

A single prompt agent uses one comprehensive prompt to define all agent behaviors, making it the simplest approach to get started.

<Frame>
  <img height="300" src="https://mintcdn.com/retellai/pRGcctz_zOqy0mSt/images/single_prompt.png?fit=max&auto=format&n=pRGcctz_zOqy0mSt&q=85&s=d6e9732f018667bdd2357552afaad217" alt="Single prompt configuration interface showing a unified prompt field" data-path="images/single_prompt.png" />
</Frame>

#### When to Use Single Prompt

✅ **Best for:**

* Simple, straightforward conversations
* Quick prototypes and testing
* Agents with 1-3 functions
* Linear conversation flows

#### Limitations at Scale

As complexity increases, single prompt agents may experience:

1. **Behavioral Drift**: Agent deviates from instructions in edge cases
2. **Function Calling Issues**: Unreliable tool usage with multiple functions
3. **Maintenance Challenges**: Large prompts become difficult to debug and update
4. **Context Confusion**: Agent struggles to track conversation state

<Note>
  Consider using conversation flow agent or multi-prompt agent when your single prompt exceeds 1000 words or uses more than 5 functions.
</Note>

### Multi-Prompt Agent

Multi-prompt agents organize conversations into a structured tree of states, each with its own focused prompt and behavior.

<Frame>
  <img height="300" src="https://mintcdn.com/retellai/rxvYffEkEJPRL1KD/images/d_2.png?fit=max&auto=format&n=rxvYffEkEJPRL1KD&q=85&s=9005f7c8238d0c3e361717831b377319" alt="Multi-prompt tree structure showing connected conversation states" data-path="images/d_2.png" />
</Frame>

#### Key Features

Each state in a multi-prompt agent includes:

* **Focused Prompt**: Specific instructions for that conversation phase
* **State-Specific Functions**: Only relevant tools available in each state
* **Transition Logic**: Clear conditions for moving between states
* **Context Preservation**: Variables and information flow between states

#### Real-World Example: Lead Qualification

<Frame>
  <img height="300" src="https://mintcdn.com/retellai/M9QYKZE4hbt00HfL/images/multi.png?fit=max&auto=format&n=M9QYKZE4hbt00HfL&q=85&s=6e40e61c6d297d119c4983acc9071f9e" alt="Lead qualification template showing two-state structure" data-path="images/multi.png" />
</Frame>

This template demonstrates effective state separation:

**State 1: Lead Qualification**

* Gather customer information
* Validate requirements
* No appointment booking functions available

**State 2: Appointment Scheduling**

* Only accessible after qualification is complete
* Booking functions enabled
* Context from qualification available

#### Benefits of Multi-Prompt Structure

1. **Predictable Behavior**: Each state has a clear, focused purpose
2. **Easier Debugging**: Issues isolated to specific states
3. **Better Function Control**: Tools available only when appropriate
4. **Scalable Design**: Add new states without affecting existing ones
5. **Team Collaboration**: Different team members can work on different states

<Tip>
  Start with our templates to see multi-prompt best practices in action, then customize for your use case.
</Tip>

## Navigating between agents

When you open a **transfer agent** from a single- or multi-prompt agent (see [Transfer call](/build/single-multi-prompt/transfer-call)), it opens in its own tab and a **selector** appears at the top of the builder — showing **Main flow** by default. Use it to switch between your main agent and any transfer agents you've opened, and to return to your main agent at any time.

Unlike conversation flow agents, single- and multi-prompt agents don't use [Components](/build/conversation-flow/components), so this selector only moves between agents.

<Frame>
  <img src="https://mintcdn.com/retellai/pYRxYvv70IUyhP6N/images/flow-selector-single-prompt.png?fit=max&auto=format&n=pYRxYvv70IUyhP6N&q=85&s=33a19e4bd9bc37393e05dd65272ef2ca" width="3830" height="1804" data-path="images/flow-selector-single-prompt.png" />
</Frame>
