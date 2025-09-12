# Enterprise Agentic AI: Scalable Meeting Orchestration with n8n 

#### An Agentic AI–powered workflow built with n8n, designed to automate meeting scheduling across an enterprise using OpenAI + Google Calendar.

#### This workflow demonstrates how Agentic AI can seamlessly integrate with enterprise tools to provide scalable, intelligent, and context-aware scheduling assistance. It can be adapted for broader enterprise use cases beyond just calendar automation.

#### Here is a figure of the Agent:

<img width="995" height="360" alt="Image" src="https://github.com/user-attachments/assets/148ba70b-0b80-462f-b414-55142ae3e711" />


---

#### For AI Architects that are interest in step-by-step detail of how the Agent is created in n8n, it is given here:

#### Instructions regarding how the Agent can be cloned and deployed using its json file (available here: https://github.com/manuelbomi/Enterprise-Agentic-AI---Scalable-Meeting-Orchestration-with-n8n/blob/main/workflows/agentic_ai_enterprise_scheduling.json) is given under the deployment section in the Read Me. 

#### Further details regarding how the Agent's deployment can be scaled for other applications in an enterprise setting are also given in later segments of the Read Me. They could also be obtained on files included in the repository. 



---

## Introduction 📖

#### In enterprise environments, scheduling meetings across teams can be repetitive and time-consuming. This repository provides a ready-to-use Agentic AI workflow in n8n that:

* Acts as an AI-powered scheduling assistant

* Reads availability from Google Calendar

* Confirms timeslots interactively with users

* Creates events automatically once finalized

* It showcases how Agentic AI principles can be applied to real-world enterprise workflows.

---

## Workflow Overview 🛠️ 

#### The workflow is composed of the following components:

* Chat Input – Entry point for the user to interact with the assistant.

* AI Agent – Orchestrates the conversation, manages tools, and ensures correct action flow.

* OpenAI Chat Model – Provides reasoning and natural language capabilities (powered by gpt-4o-mini).

* Simple Memory – Stores context across interactions.

* Google Calendar Tools

* Get Availability: Searches for free time slots.

* Create Event: Books the meeting once a slot is confirmed.

* This is the n8n Agentic AI workflow that powers the scheduling assistant:

<img width="1100" height="344" alt="Image" src="https://github.com/user-attachments/assets/1361cf67-feed-43fe-bcc3-5a89505dec0c" />
  
---

#### Below is a step-by-step interaction diagram showing how the AI Agent interact with the uset to help schedule a meeting:
  * participant U as User
    
  * participant A as AI Agent
    
  * participant G as Google Calendar



  

    U->>A: "Can you schedule a 30-min meeting with John tomorrow?"
    A->>G: Check availability for tomorrow
    G-->>A: Free slots: 2:00 PM, 3:30 PM, 4:15 PM
    A-->>U: "You are free at 2:00, 3:30, and 4:15. Which works best?"
    U->>A: "Let’s go with 3:30 PM."
    A->>U: "Should I add a description?"
    U->>A: "Yes, 'Project Roadmap Discussion'."
    A->>G: Create event at 3:30 PM with description
    G-->>A: Event created
    A-->>U: "Meeting scheduled with John at 3:30 PM tomorrow."

---

#### Below are some figures of how the Agent interact with users via the n8n UI
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/0d2b2e3e-8580-47cf-9db2-954faa2bba46" />

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/792d2cf7-2235-4c8b-b8f6-1c91f80403f7" />


---

## Workflow Diagram 🔗:

🎥 Demo
1. Workflow Architecture




2. Example Conversation Flow

Below is a step-by-step interaction diagram showing how the AI assistant helps schedule a meeting:

sequenceDiagram
    participant U as User
    participant A as AI Agent
    participant G as Google Calendar

    U->>A: "Can you schedule a 30-min meeting with John tomorrow?"
    A->>G: Check availability for tomorrow
    G-->>A: Free slots: 2:00 PM, 3:30 PM, 4:15 PM
    A-->>U: "You are free at 2:00, 3:30, and 4:15. Which works best?"
    U->>A: "Let’s go with 3:30 PM."
    A->>U: "Should I add a description?"
    U->>A: "Yes, 'Project Roadmap Discussion'."
    A->>G: Create event at 3:30 PM with description
    G-->>A: Event created
    A-->>U: "Meeting scheduled with John at 3:30 PM tomorrow."

3. Scheduled Event in Google Calendar

Once confirmed, the workflow creates a Google Calendar event automatically:


 
## How to Deploy the Current Workflow 🚀
* 1. Clone the Repository
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>

* 2. Import Workflow into n8n

Open your n8n instance
.

Go to Workflows → Import from File.

Upload Agentic_AI_workflow.json from this repo.

* 3. Set Up Credentials

Configure your OpenAI API key under Credentials → OpenAI.

Configure your Google Calendar OAuth2 credentials under Credentials → Google Calendar.

* 4. Run the Workflow

Activate the workflow.

Start interacting with the chat input node.

The AI assistant will:

Fetch availability

Suggest slots

Book events automatically


💬 Example Conversation Flow

Here’s how a typical interaction looks once the workflow is deployed:

User:
"Can you schedule a 30-minute meeting with John tomorrow afternoon?"

AI Agent:
"Sure! Let me check your calendar for availability tomorrow afternoon..."

(Agent uses Google Calendar to fetch free slots)

AI Agent:
"You are free at 2:00 PM, 3:30 PM, and 4:15 PM. Which works best for you?"

User:
"Let’s go with 3:30 PM."

AI Agent:
"Perfect. Scheduling the meeting with John at 3:30 PM tomorrow. Do you want me to add a description?"

User:
"Yes — add 'Project Roadmap Discussion'."

AI Agent:
"Done! I’ve scheduled 'Project Roadmap Discussion' with John tomorrow at 3:30 PM, and added it to your Google Calendar."











⚙️ Scalability for Enterprise Applications

While this workflow focuses on meeting scheduling, the same architecture can be extended to other enterprise use cases:

Human Resources: Automate interview scheduling & reminders

Project Management: Book sprint reviews, stand-ups, and cross-team syncs

Customer Support: Schedule client calls, follow-ups, and escalations

Sales & Marketing: Auto-schedule demos, campaigns, and recurring check-ins

Scalability Features:

Plug-and-play integration with multiple calendars or services (Google Workspace, Microsoft Outlook, etc.)

Extendable with enterprise authentication (SSO, OAuth2)

Reusable Agentic AI pattern: chat input → memory → reasoning → tool execution




🔮 Future Enhancements

Add multi-calendar / multi-user support for enterprise teams

Support for Microsoft Teams / Outlook Calendar

Integration with Slack / MS Teams bots for chat-driven scheduling

Automated rescheduling and conflict resolution"# Enterprise-Agentic-AI---Scalable-Meeting-Orchestration-with-n8n" 
