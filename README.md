# Axylo-v1
Modern desktops are powerful, but interacting with them still depends heavily on manual clicking, searching, scrolling, typing, and switching between apps. Even simple tasks like opening applications, writing structured content,live research, or user attention and context switching. This creates friction, slows productivity, and fragments focus.

Voice assistants exist, but most are:

Cloud-dependent
Slow or unreliable for real desktop control
Poor at reasoning or multi-step tasks
Limited to simple commands
Unable to integrate with coding, automation, or deep research workflows
What’s missing is a local, intelligent, private, voice-driven concierge agent that understands natural commands, reasons with an LLM, uses structured tools to control the device, performs web research, and interacts through a clean GUI.

Axylo solves this by combining real-time speech, desktop automation, LLM reasoning, web intelligence, and a rich interface into one coherent agent built to feel like a personal digital concierge.

Why?
Axylo’s capabilities cannot be implemented with simple scripts or static logic. The system must:

Interpret ambiguous natural-language commands
Decide when it should use a tool (scrolling, launching apps, sending messages, smart writing)
Perform web research and produce multi-step reasoning
Maintain conversational context
Interact with users through both voice and GUI
These requirements align directly with agent-based architectures:

Action Selection The LLM-based agent chooses when to call tools like control_app, intelligent_web_search, control_scroll, or the voice-messaging and voice-typing flows.

Multi-step Task Execution Axylo has sub-agents (ResearchAgent and CodeAgent) invoked when reasoning-heavy tasks demand deeper analysis or code manipulation.

Autonomous Voice Loop The system listens continuously through VoiceHandler.listen_async() and sends user speech into the agent loop.

Tool-Oriented Reasoning The agent uses a carefully designed system instruction describing available tools (app control, media control, auto-scroll, YouTube control, web search, ResearchAgent, CodeAgent, and the GUI launcher). This allows decisions to be made through structured agent reasoning rather than hardcoded heuristics.

Real-Time Feedback The agent updates GUI chips such as "LLM: Thinking," logs its internal operations, and speaks short TTS-friendly summaries.

Agents shine where dynamic reasoning, uncertainty, and tool orchestration are needed-making this the ideal blueprint for Axylo.

The overall architecture
Axylo is a local, voice-controlled desktop agent built around several core subsystems:

1. Real-Time Voice Interaction
Continuous microphone listening using SpeechRecognition.

TTS using edge-tts for high-quality output.

Auto-muting logic prevents the microphone from hearing Axylo’s own voice while speaking.

Keyword-based triggers:

"voice typing" → Notepad typing session
"write …" → AI writing tool
"send a message" → guided messaging flow
Fully hands-free operation.

2. Main LLM Agent (Axylo Agent)
The agent is constructed in agent.py with:

System-level instructions defining safety, clarity, correctness, and tool behavior.
Integration with Gemini 2.0 Flash via Google ADK.
Structured tool definitions (app control, scrolling, media, YouTube, search, sub-agents).
Thoughtful constraints: the agent avoids hallucinating screen vision or unsupported actions.
The agent decides:

Whether to respond directly
Whether a tool is needed
Whether a deep sub-agent (ResearchAgent, CodeAgent) must be invoked
3. Desktop Automation Tools
Located in tools.py and app_launcher.py, Axylo supports:

Opening and closing apps via fuzzy-matched application indexing
Dynamic scrolling and continuous auto-scroll
YouTube playback control
System media control (volume, play/pause)
Closing the current browser tab
Smart web search using DuckDuckGo + webpage extraction
Intelligent summarization of results
All actions are logged clearly in the GUI.

4. Sub-Agents for Deep Reasoning
Axylo employs two internal sub-agents:

ResearchAgent → used when long web pages or research results require deep analysis
CodeAgent → used for refactoring, debugging, or code explanation tasks
These agents run synchronously and return structured outputs.

5. AI Smart Writer
Through smart_writer.py, Axylo can:

Generate professional emails
Draft documents
Produce formatted technical writing
Autosave results as .docx or .txt
Triggered by natural commands: "Write a report on…" or "Write an email to…"

6. Voice Typing
voice_typing.py opens Notepad and types dictated text continuously.

7. Voice Messaging
voice_messaging.py helps the user send structured text messages (WhatsApp/web/E-mail formatting). The system guides the user through:

recipient
content
confirmation
final output
8. GUI-Based Agent Interface
chatbot_ui.py implements a polished CustomTkinter GUI with:

Chat-style message history
Copy button for each AI reply
Quick actions (Smart writing, Voice typing, Send message)
Full log panel with filters (User / Agent / Tools / Voice / Errors / Search)
Agent status chips (Mic, LLM, TTS, Last response time)
Start / Stop / Restart agent controls
Diagnostics button
Separate chat window for text-only interactions
The GUI provides an excellent hybrid experience combining convenience and transparency.

9. Terminal-Based Agent Loop
main.py and Start_Agent.py contain the classic voice loop:

Continuous speech recognition
Request correlation IDs for observability
Graceful shutdown
Automatic sleep when the user is silent
Demo - How Axylo works
Example demo sequence:

User says: "Open Chrome and search for affordable laptops."

Axylo opens Chrome
Performs intelligent web search
Summarizes results
Speaks back top insights
User says: "Scroll down slowly."

Auto-scroll begins
User says: "Stop scrolling."

Auto-scroll stops
User says: "Write an email to my professor about the project submission."

Smart Writer generates a well-structured email
Saves it locally
Reads a concise spoken summary
User says: "Send a message to XYZ telling him I’ll be late."

Message flow begins
Confirms recipient + content
Generates clean formatted text
User opens GUI:

Views logs
Copies AI responses
Uses quick actions
Restarts agent
Everything is orchestrated by the LLM agent through structured tools.

The Build - Tools, technologies, and approach
Axylo is built entirely with local Python technologies and LLM reasoning:

Core Tech Stack
Python (primary runtime)
Google ADK + Gemini 2.0 Flash for the reasoning engine
SpeechRecognition + PyAudio for voice input
edge-tts and gTTS for speech output
CustomTkinter for GUI
PyAutoGUI for automation
DuckDuckGo search + Trafilatura for web extraction
python-docx for file outputs
Development Strategy
Agent-first design Clear system instructions + strict tool boundaries.

Separation of responsibilities

Voice engine
Desktop tools
Sub-agents
GUI
Smart writing
Messaging
Strong logging & debugging A detailed logger feeds both the terminal and GUI panels.

Human-friendly voice communication _sanitize_for_speech and _shorten_text ensure brief, clear replies.

Scalable architecture Every component is modular and replaceable.

If I had more time, this is what I'd do
1. Autonomous Recursive Self-Improvement (RSI) Loop
*(Enhanced using the RSI concepts you provided) *

My long-term vision is to evolve Axylo into a system that can repair, optimize, and evolve itself-a lightweight Autonomous Recursive Self-Improving Agent (RSI Agent).

This background "Engineer Agent" would:

Monitor Axylo during execution
Detect runtime failures, slowdowns, or anomalies
Analyze logs and stack traces to locate the root cause
Use Gemini to generate a code-level patch
Apply the patch in a sandboxed environment
Run automated regression tests
If validated, hot-swap the improved module into the live agent
This creates a future where Axylo can:

Self-heal
Self-optimize
Evolve into progressively more capable versions
Reduce human maintenance effort
Improve reliability after every cycle
It is inspired by the theoretical foundations of Recursive Self-Improvement, where each improved version becomes better at improving itself. While dangerous in unrestricted settings, a carefully sandboxed and governed version for personal desktop automation is an exciting and achievable next step.

2. Multimodal Vision Integration
Right now, Axylo acts through text and app names only. Future integration with Gemini’s multimodal screen understanding would allow:

"Click the blue Submit button."
"Select the second item on the left."
"Read this webpage and summarize it visually."
This removes dependence on fixed coordinates or OS automation shortcuts and converts Axylo into a true visual concierge.

3. Long-Term Contextual Memory
I would integrate a vector database (like ChromaDB or Pinecone) enabling:

Persistent personal preferences
Cross-day task continuity
Personalized reminders
Advanced contextual reasoning
Example behavior:

"Send the report to the same person I emailed last Tuesday."
"Write the summary in the tone I prefer for my research notes."
This would elevate Axylo from a session-bound agent to a personalized long-term assistant.

