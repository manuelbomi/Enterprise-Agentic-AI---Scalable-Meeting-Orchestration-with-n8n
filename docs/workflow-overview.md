📘 Workflow Overview

This document provides a detailed overview of the Enterprise Agentic AI Scheduler workflow built in n8n.

The goal of this workflow is to create an Agentic AI Calendar Assistant that can:

Understand natural language requests.

Check for availability in Google Calendar.

Confirm timeslots interactively.

Create events automatically.

🧩 Workflow Components
1. Chat Input

Node Type: chatTrigger

Purpose: Entry point for user interaction.

Notes:

Captures user queries (e.g., “Schedule a meeting with John tomorrow”).

Provides a conversational interface for the assistant.

2. AI Agent

Node Type: langchain.agent

Purpose: Core reasoning engine of the workflow.

Features:

Takes user input.

Uses tools (Google Calendar actions).

Delegates tasks like checking availability or creating events.

Configured With:

System Message → Defines assistant role as a “calendar assistant.”

Memory & LLM → Keeps track of context across the conversation.

3. OpenAI Chat Model

Node Type: lmChatOpenAi

Model Used: gpt-4o-mini

Purpose: Provides natural language reasoning and response generation.

Why GPT-4o-mini?

Optimized for speed and cost.

Provides sufficient intelligence for scheduling tasks.

4. Simple Memory

Node Type: memoryBufferWindow

Purpose: Stores the history of recent messages.

Benefit:

Helps the AI maintain context (e.g., remembering the meeting details during confirmation).

5. Google Calendar Tools
a) Get Availability

Node Type: googleCalendarTool

Purpose: Checks the user’s Google Calendar for free time slots.

Inputs:

start_time and end_time from AI reasoning.

Outputs:

List of free blocks that the AI can propose to the user.

b) Create Event

Node Type: googleCalendarTool

Purpose: Books the confirmed meeting.

Inputs:

start_date, end_date, summary, and description.

Outputs:

Confirmation that an event has been created.

🔗 Workflow Flow

User sends a message (via Chat Input).

AI Agent interprets the request.

Memory ensures context is preserved.

OpenAI Chat Model generates responses and reasoning steps.

If checking schedule → Google Calendar (Get Availability) is used.

If confirming a booking → Google Calendar (Create Event) is used.

AI responds back with the confirmation.

🖼️ Visual Diagram

💡 Key Highlights

Agentic AI Pattern: Combines reasoning (LLM), context (Memory), and action execution (Calendar Tools).

Enterprise-Ready: Can scale to multiple calendars, integrate with SSO, or extend to HR/CRM use cases.

Reusable Design: The same structure can be repurposed for other enterprise workflows (ticketing, approvals, task automation).