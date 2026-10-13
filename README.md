# NEXUS-AI
**NEXUS AI** is a single-file, serverless web chatbot powered by Google's Gemini API. Styled in a purple-and-white glassmorphic interface, it features Markdown rendering, local memory, voice interaction, dark mode, prompt chips, and Hinglish support—running instantly inside any browser.
<!DOCTYPE html>
<html lang="en">
<head>
<!-- ===== STEP 1: PASTE YOUR API KEY BETWEEN THE QUOTES ON LINE 6 ===== -->
<script>
const API_KEY = "AQ.Ab8RN6IE0rJaGnSxlK34IamPgILLqJQY8gM54u5hO_iiGOCxDw";
const MODEL = "gemini-3.5-flash-lite";
</script>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NEXUS AI - Adaptive Intelligence System</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-gradient: linear-gradient(135deg, #f5f3ff 0%, #ede9fe 50%, #fdf4ff 100%);
            --panel-bg: rgba(255, 255, 255, 0.85);
            --text-main: #1f2937;
            --text-muted: #6b7280;
            --primary: #7c3aed;
            --primary-hover: #6d28d9;
            --primary-light: #f3e8ff;
            --border-color: rgba(124, 58, 237, 0.2);
            --bubble-user: #7c3aed;
            --bubble-user-text: #ffffff;
            --bubble-bot: #ffffff;
            --bubble-bot-text: #1f2937;
            --shadow: 0 10px 25px -5px rgba(124, 58, 237, 0.15), 0 8px 10px -6px rgba(124, 58, 237, 0.1);
            --code-bg: #f3f4f6;
            --code-text: #1f2937;
        }

        [data-theme="dark"] {
            --bg-gradient: linear-gradient(135deg, #0f051d 1e%, #1e1b4b 50%, #09090b 100%);
            --panel-bg: rgba(17, 12, 34, 0.85);
            --text-main: #f3f4f6;
            --text-muted: #9ca3af;
            --primary: #8b5cf6;
            --primary-hover: #7c3aed;
            --primary-light: rgba(139, 92, 246, 0.15);
            --border-color: rgba(139, 92, 246, 0.3);
            --bubble-user: #7c3aed;
            --bubble-user-text: #ffffff;
            --bubble-bot: rgba(30, 27, 75, 0.7);
            --bubble-bot-text: #f3f4f6;
            --shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.5), 0 8px 10px -6px rgba(0, 0, 0, 0.3);
            --code-bg: #111827;
            --code-text: #e5e7eb;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Poppins', sans-serif;
            transition: background 0.3s ease, color 0.3s ease, border-color 0.3s ease;
        }

        body {
            background: var(--bg-gradient);
            color: var(--text-main);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 16px;
        }

        .app-container {
            width: 100%;
            max-width: 900px;
            height: 92vh;
            background: var(--panel-bg);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid var(--border-color);
            border-radius: 24px;
            box-shadow: var(--shadow);
            display: flex;
            flex-direction: column;
            overflow: hidden;
            position: relative;
        }

        /* DivyanSh Aggarwal Unique Signature Badge Style */
        .divyansh-badge {
            background: linear-gradient(135deg, #7c3aed, #a855f7);
            color: #ffffff;
            font-size: 10px;
            font-weight: 600;
            padding: 3px 10px;
            border-radius: 20px;
            letter-spacing: 0.5px;
            text-transform: uppercase;
            box-shadow: 0 2px 8px rgba(124, 58, 237, 0.3);
            display: inline-block;
            margin-bottom: 4px;
        }

        /* Header */
        header {
            padding: 16px 24px;
            background: rgba(124, 58, 237, 0.05);
            border-bottom: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .header-left {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .logo-box {
            width: 42px;
            height: 42px;
            background: linear-gradient(135deg, #7c3aed, #c084fc);
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #fff;
            font-size: 20px;
            font-weight: 700;
            box-shadow: 0 4px 12px rgba(124, 58, 237, 0.3);
            position: relative;
            animation: pulse-glow 3s infinite;
        }

        @keyframes pulse-glow {
            0%, 100% { box-shadow: 0 0 15px rgba(124, 58, 237, 0.4); }
            50% { box-shadow: 0 0 25px rgba(168, 85, 247, 0.7); }
        }

        .logo-title h1 {
            font-size: 18px;
            font-weight: 600;
            letter-spacing: 0.5px;
            color: var(--text-main);
        }

        .logo-title p {
            font-size: 11px;
            color: var(--text-muted);
        }

        .header-actions {
            display: flex;
            gap: 10px;
        }

        .btn-icon {
            background: var(--primary-light);
            border: 1px solid var(--border-color);
            color: var(--primary);
            width: 38px;
            height: 38px;
            border-radius: 10px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 16px;
            transition: all 0.2s;
        }

        .btn-icon:hover {
            background: var(--primary);
            color: #fff;
            transform: translateY(-2px);
        }

        /* Chat Body */
        .chat-messages {
            flex: 1;
            padding: 24px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 20px;
            scroll-behavior: smooth;
        }

        .message {
            display: flex;
            gap: 12px;
            max-width: 80%;
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .message.user {
            margin-left: auto;
            flex-direction: row-reverse;
        }

        .avatar {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
            flex-shrink: 0;
            background: var(--primary-light);
            border: 1px solid var(--border-color);
        }

        .message.user .avatar {
            background: var(--primary);
            color: white;
        }

        .bubble-wrapper {
            display: flex;
            flex-direction: column;
            gap: 4px;
        }

        .message.user .bubble-wrapper {
            align-items: flex-end;
        }

        .bubble {
            padding: 14px 18px;
            border-radius: 16px;
            font-size: 14px;
            line-height: 1.5;
            word-break: break-word;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
        }

        .message.bot .bubble {
            background: var(--bubble-bot);
            color: var(--bubble-bot-text);
            border: 1px solid var(--border-color);
            border-top-left-radius: 4px;
        }

        .message.user .bubble {
            background: var(--bubble-user);
            color: var(--bubble-user-text);
            border-top-right-radius: 4px;
        }

        .message.error .bubble {
            background: #fee2e2;
            color: #991b1b;
            border: 1px solid #fca5a5;
        }

        .message-footer {
            display: flex;
            gap: 8px;
            font-size: 11px;
            color: var(--text-muted);
            margin-top: 2px;
            align-items: center;
        }

        .action-btn {
            background: none;
            border: none;
            color: var(--text-muted);
            cursor: pointer;
            font-size: 11px;
            padding: 2px 6px;
            border-radius: 4px;
            transition: background 0.2s;
        }

        .action-btn:hover {
            background: var(--primary-light);
            color: var(--primary);
        }

        /* Starter Chips */
        .starter-container {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            margin-top: 12px;
        }

        .starter-chip {
            background: var(--primary-light);
            border: 1px solid var(--border-color);
            color: var(--primary);
            padding: 8px 14px;
            border-radius: 20px;
            font-size: 12px;
            cursor: pointer;
            font-weight: 500;
            transition: all 0.2s;
        }

        .starter-chip:hover {
            background: var(--primary);
            color: white;
            transform: translateY(-2px);
        }

        /* Typing Indicator */
        .typing-indicator {
            display: flex;
            gap: 5px;
            padding: 10px 14px;
            align-items: center;
        }

        .typing-dot {
            width: 8px;
            height: 8px;
            background: var(--primary);
            border-radius: 50%;
            animation: bounce 1.4s infinite ease-in-out both;
        }

        .typing-dot:nth-child(1) { animation-delay: -0.32s; }
        .typing-dot:nth-child(2) { animation-delay: -0.16s; }

        @keyframes bounce {
            0%, 80%, 100% { transform: scale(0); }
            40% { transform: scale(1.0); }
        }

        /* Input Area */
        .input-container {
            padding: 16px 24px;
            background: rgba(124, 58, 237, 0.03);
            border-top: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .input-wrapper {
            flex: 1;
            background: var(--panel-bg);
            border: 1px solid var(--border-color);
            border-radius: 14px;
            display: flex;
            align-items: center;
            padding: 6px 14px;
            box-shadow: inset 0 2px 4px rgba(0,0,0,0.02);
        }

        textarea {
            flex: 1;
            background: transparent;
            border: none;
            outline: none;
            color: var(--text-main);
            font-size: 14px;
            resize: none;
            max-height: 120px;
            padding: 8px 0;
            line-height: 1.4;
        }

        textarea::placeholder {
            color: var(--text-muted);
        }

        .input-actions {
            display: flex;
            gap: 8px;
            align-items: center;
        }

        .btn-send {
            background: var(--primary);
            color: white;
            border: none;
            width: 42px;
            height: 42px;
            border-radius: 12px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 16px;
            transition: all 0.2s;
            box-shadow: 0 4px 12px rgba(124, 58, 237, 0.3);
        }

        .btn-send:hover:not(:disabled) {
            background: var(--primary-hover);
            transform: translateY(-2px);
        }

        .btn-send:disabled {
            opacity: 0.6;
            cursor: not-allowed;
        }

        .mic-active {
            background: #ef4444 !important;
            color: white !important;
            animation: pulse-mic 1.5s infinite;
        }

        @keyframes pulse-mic {
            0% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.4); }
            70% { box-shadow: 0 0 0 10px rgba(239, 68, 68, 0); }
            100% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0); }
        }

        /* Markdown elements formatting */
        .bubble p { margin-bottom: 8px; }
        .bubble p:last-child { margin-bottom: 0; }
        .bubble ul, .bubble ol { margin-left: 20px; margin-bottom: 8px; }
        .bubble code { background: var(--code-bg); color: var(--code-text); padding: 2px 5px; border-radius: 4px; font-family: monospace; font-size: 0.9em; }
        .bubble pre { background: var(--code-bg); color: var(--code-text); padding: 10px; border-radius: 8px; overflow-x: auto; margin: 8px 0; font-family: monospace; font-size: 0.9em; }
        .bubble strong { font-weight: 600; }
        .bubble em { font-style: italic; }

        @media (max-width: 600px) {
            .app-container { height: 100vh; border-radius: 0; border: none; }
            body { padding: 0; }
            .message { max-width: 90%; }
        }
    </style>
</head>
<body>

    <div class="app-container">
        <!-- Header -->
        <header>
            <div class="header-left">
                <div class="logo-box">N</div>
                <div class="logo-title">
                    <span class="divyansh-badge">DivyanSh Aggarwal Style</span>
                    <h1>NEXUS AI</h1>
                    <p>Adaptive Intelligence System</p>
                </div>
            </div>
            <div class="header-actions">
                <button class="btn-icon" id="themeToggle" title="Toggle Theme">🌙</button>
                <button class="btn-icon" id="clearChat" title="Clear Chat">🗑️</button>
            </div>
        </header>

        <!-- Chat Messages -->
        <div class="chat-messages" id="chatMessages">
            <!-- Dynamic Messages populated here -->
        </div>

        <!-- Input Area -->
        <div class="input-container">
            <div class="input-wrapper">
                <textarea id="userInput" placeholder="Ask Nexus AI anything or chat in Hinglish..." rows="1"></textarea>
                <div class="input-actions">
                    <button class="btn-icon" id="micBtn" title="Voice Input" style="display:none;">🎤</button>
                </div>
            </div>
            <button class="btn-send" id="sendBtn" title="Send Message">➤</button>
        </div>
    </div>

    <!-- SYSTEM PROMPT -->
    <script>
    const SYSTEM_PROMPT = `
ROLE: Act as a world-class team consisting of:  An AI systems architect specializing in Small Language Models.  A senior HTML, CSS, and JavaScript developer.  A futuristic interface and interaction designer.  An expert in AI orchestration, memory, retrieval, and tool calling.  A security-conscious engineer who avoids fake functionality and insecure credential handling.  Think like a product designer building an entirely new category of AI assistant, not a clone of existing chatbots.
TASK: Build NEXUS AI: Adaptive Intelligence System, a chatbot that combines a futuristic interface with efficient AI capabilities.  Its defining innovation is Adaptive Intelligence: the system should select the most appropriate available model, tool, or workflow for each task instead of using the same approach for everything.  The application should include the following features.  1. Futuristic AI command center  Dark obsidian interface with electric cyan, violet, and subtle blue accents.  Animated central neural sphere or AI energy core.  Glassmorphism panels, soft neon borders, subtle particles, and smooth animations.  Responsive layout for desktop, tablet, and mobile.  Premium typography, carefully controlled spacing, and polished transitions.  Light and dark appearance options if practical.  2. Intelligent conversational engine  Functional chat input and send button.  Streaming responses when supported by the chosen model API.  Conversation history and new-chat functionality.  Markdown rendering, code blocks, copy buttons, and readable formatting.  Loading indicators and clear error messages.  Stop-generation control when supported.  Context-aware follow-up conversations.  3. Adaptive Intelligence Router Classify each incoming task into categories such as:  General conversation.  Coding and debugging.  Mathematics and calculations.  Document understanding.  Research and information retrieval.  Summarization and planning.  Choose a suitable available model or tool based on task complexity, capability, latency, and cost.  Use a small model for suitable lightweight tasks. Use a stronger model only when configured and justified. Use deterministic code for arithmetic whenever appropriate.  If routing is unavailable, fall back gracefully to the configured model. Never pretend that a model, tool, or secondary agent was used when it was not.  4. User-controlled AI memory  Optional persistent preferences and project notes.  A memory panel showing saved items.  Add, edit, inspect, and delete controls.  A clear distinction between temporary conversation context and saved memory.  User consent before storing sensitive or personal information.  No claims of permanent memory unless storage actually works.  5. AI tools and workspace  Calculator for reliable numerical results.  Code-writing workspace with syntax-highlighted output where supported.  Document upload and text extraction where technically available.  Summaries, checklists, and structured outputs.  A visual knowledge graph for connecting ideas.  Export and copy conversation features.  Implement actual working features first. Label unfinished or unsupported capabilities honestly.  6. Voice interaction  Microphone input using browser speech recognition when supported.  Optional text-to-speech output.  Clear permission and availability indicators.  Text chat must remain usable if voice features are unavailable.  7. AI activity panel Show meaningful operational statuses such as:  Understanding request.  Selecting a model or tool.  Retrieving relevant information.  Generating response.  Completed.  Display real model and tool information when available. Do not invent hidden reasoning, confidence scores, live token counts, or system activity.  8. SLM performance dashboard When actual measurements are available, display:  Selected model.  Response latency.  Input and output token counts.  Estimated API cost, if supported by trustworthy pricing data.  Task category and selected tool.  Local versus remote processing status.  Clearly label estimates and unavailable metrics.
CONTEXT: NEXUS AI is designed for a beginner who wants to start with a manageable chatbot and gradually evolve it into a sophisticated SLM application.  The initial version should prioritize reliable core functionality over an excessive number of unfinished features.  Preferred first version:  One file named index.html.  HTML, CSS, and JavaScript in the same file.  No npm, frameworks, build tools, or additional source files.  Must be easy to save and open in a modern browser.  Use readable, organized, well-commented code.  Include helpful setup instructions and troubleshooting guidance.  For the initial prototype, use a configurable AI provider API if a compatible service is selected. Explain that an API-backed chatbot needs an internet connection.  Never place a real production API key in publicly distributed frontend code. If a direct browser connection is supported for a prototype, clearly warn that the key can be exposed. Recommend a secure backend or an appropriate local inference runtime for production.  For offline operation, explain that a standalone HTML file does not itself contain a working SLM runtime. Plan a later version that uses a compatible local model and inference engine.  Do not hardcode an unverified model identifier. Make the provider and model configurable, and explain how to choose a currently supported model.
CONSTRAINTS: Follow these requirements strictly:  Deliver complete, usable code rather than pseudocode.  Do not omit sections with phrases such as “implement this later,” “rest of code here,” or ellipses.  Do not invent working API integrations, model capabilities, or tool results.  Do not claim the AI has memory, internet access, file-reading capability, or offline inference unless the feature is implemented.  Do not expose private API credentials in a production deployment.  Keep the interface responsive, accessible, and keyboard-friendly.  Validate inputs and handle network errors, rate limits, and malformed responses.  Prevent unsafe HTML rendering and sanitize untrusted model output.  Respect reduced-motion preferences and provide accessible labels for controls.  Use local storage only for appropriate user-controlled data, and provide a way to clear it.  Keep code organized into clearly labeled sections despite using a single file.  Avoid unnecessary dependencies and external services.  Do not use fake analytics or fabricated performance statistics.  If a requested feature cannot work under the chosen architecture, explain the limitation and implement the closest honest alternative.
OUTPUT: Deliver the project in the following order:  Part 1: Product overview Explain the NEXUS AI concept, its standout features, and the architecture.  Part 2: Complete application Provide the entire index.html file in one code block. Include all HTML, CSS, and JavaScript required for the chosen working implementation. Do not omit code.  Part 3: Setup instructions Explain step by step how a beginner should:  Create index.html.  Paste and save the code.  Configure the selected model provider.  Open the application.  Send the first message.  Diagnose common errors.  Part 4: Feature verification List each feature and classify it as:  Fully implemented.  Implemented with browser or provider limitations.  Planned for a later version.  Part 5: SLM evolution roadmap Describe how to progress from the initial chatbot to:  Model routing.  Retrieval-augmented generation (RAG).  User-controlled long-term memory.  Local SLM inference.  Centered AI agents.  Secure deployment and evaluation.  Part 6: Final quality check Check for JavaScript errors, broken controls, incomplete event handlers, missing elements, unsafe rendering, incorrect API assumptions, and misleading status indicators.  Prioritize a working, polished core experience over feature quantity.  FINAL PRODUCT VISION  NEXUS AI should feel like a personal intelligence operating system: elegant, adaptive, transparent about its capabilities, efficient with resources, and ready to grow.  Its core promise is:  “One interface. Adaptive intelligence. Unlimited possibilities.”  Build it as a credible, extensible AI product—not merely a futuristic-looking mockup.
LANGUAGE: If the user writes in Hindi or Hinglish, reply in the same language.
`;
    </script>

    <!-- API CONNECTION -->
    <script>
    async function callAI(history) {
        const url = "https://generativelanguage.googleapis.com/v1beta/models/" + MODEL + ":generateContent";
        const res = await fetch(url, {
            method: "POST",
            headers: { "Content-Type": "application/json", "x-goog-api-key": API_KEY.trim() },
            body: JSON.stringify({
                system_instruction: { parts: [{ text: SYSTEM_PROMPT }] },
                contents: history.map(m => ({
                    role: m.role === "assistant" ? "model" : "user",
                    parts: [{ text: m.text }]
                }))
            })
        });
        const data = await res.json().catch(() => ({}));
        if (!res.ok) throw new Error("Error " + res.status + ": " + ((data.error && data.error.message) || "Request failed"));
        const parts = (data.candidates && data.candidates[0] && data.candidates[0].content && data.candidates[0].content.parts) || [];
        const text = parts.filter(p => p.text && !p.thought).map(p => p.text).join("");
        if (!text) throw new Error("The AI sent an empty reply. Try asking again.");
        return text;
    }
    </script>

    <!-- APP LOGIC -->
    <script>
        let chatHistory = [];
        let messages = [];

        const chatMessages = document.getElementById('chatMessages');
        const userInput = document.getElementById('userInput');
        const sendBtn = document.getElementById('sendBtn');
        const clearChatBtn = document.getElementById('clearChat');
        const themeToggleBtn = document.getElementById('themeToggle');
        const micBtn = document.getElementById('micBtn');

        // Load saved state from localStorage
        function loadStorage() {
            const savedMessages = localStorage.getItem('nexus_messages');
            const savedTheme = localStorage.getItem('nexus_theme');
            
            if (savedTheme === 'dark') {
                document.documentElement.setAttribute('data-theme', 'dark');
                themeToggleBtn.textContent = '☀️';
            }

            if (savedMessages) {
                try {
                    messages = JSON.parse(savedMessages);
                    chatHistory = messages.map(m => ({
                        role: m.sender === 'user' ? 'user' : 'assistant',
                        text: m.text
                    }));
                    renderMessages();
                } catch(e) {
                    messages = [];
                }
            }
            
            if (messages.length === 0) {
                appendWelcomeMessage();
            }
        }

        function saveStorage() {
            localStorage.setItem('nexus_messages', JSON.stringify(messages));
        }

        function appendWelcomeMessage() {
            const welcomeText = "Hello! I am **NEXUS AI**, your adaptive intelligence system crafted in DivyanSh Aggarwal's signature style. How can I assist you today?";
            const starterChips = [
                "What is adaptive intelligence?",
                "Write a quick JavaScript snippet",
                "Kaise ho? Explain quantum computing in Hinglish"
            ];
            
            messages.push({ sender: 'bot', text: welcomeText, chips: starterChips });
            saveStorage();
            renderMessages();
        }

        function escapeHTML(str) {
            return str.replace(/[&<>'"]/g, 
                tag => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;', "'": '&#39;', '"': '&quot;' }[tag] || tag)
            );
        }

        function formatMarkdown(text) {
            let safe = escapeHTML(text);
            // Code blocks
            safe = safe.replace(/```([\s\S]*?)```/g, '<pre><code>$1</code></pre>');
            // Inline code
            safe = safe.replace(/`([^`]+)`/g, '<code>$1</code>');
            // Bold
            safe = safe.replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>');
            // Italic
            safe = safe.replace(/\*(.*?)\*/g, '<em>$1</em>');
            // Bullet lists
            safe = safe.replace(/^\s*[-*]\s+(.*)$/gm, '<ul><li>$1</li></ul>');
            // Line breaks
            safe = safe.replace(/\n/g, '<br>');
            return safe;
        }

        function renderMessages() {
            chatMessages.innerHTML = '';
            messages.forEach((msg, idx) => {
                const messageDiv = document.createElement('div');
                messageDiv.className = `message ${msg.sender === 'user' ? 'user' : 'bot'} ${msg.isError ? 'error' : ''}`;
                
                const avatar = document.createElement('div');
                avatar.className = 'avatar';
                avatar.textContent = msg.sender === 'user' ? '🙂' : '🤖';

                const bubbleWrapper = document.createElement('div');
                bubbleWrapper.className = 'bubble-wrapper';

                const bubble = document.createElement('div');
                bubble.className = 'bubble';
                bubble.innerHTML = formatMarkdown(msg.text);

                bubbleWrapper.appendChild(bubble);

                // Starters if present
                if (msg.chips && msg.chips.length > 0) {
                    const chipsDiv = document.createElement('div');
                    chipsDiv.className = 'starter-container';
                    msg.chips.forEach(chipText => {
                        const chip = document.createElement('button');
                        chip.className = 'starter-chip';
                        chip.textContent = chipText;
                        chip.onclick = () => submitMessage(chipText);
                        chipsDiv.appendChild(chip);
                    });
                    bubbleWrapper.appendChild(chipsDiv);
                }

                // Footer with actions (copy/speak)
                const footer = document.createElement('div');
                footer.className = 'message-footer';
                
                if (msg.sender === 'bot' && !msg.isError) {
                    const copyBtn = document.createElement('button');
                    copyBtn.className = 'action-btn';
                    copyBtn.textContent = '📋 Copy';
                    copyBtn.onclick = () => {
                        navigator.clipboard.writeText(msg.text);
                        copyBtn.textContent = '✅ Copied';
                        setTimeout(() => copyBtn.textContent = '📋 Copy', 2000);
                    };
                    footer.appendChild(copyBtn);

                    if ('speechSynthesis' in window) {
                        const speakBtn = document.createElement('button');
                        speakBtn.className = 'action-btn';
                        speakBtn.textContent = '🔊 Read';
                        speakBtn.onclick = () => {
                            window.speechSynthesis.cancel();
                            const utterance = new SpeechSynthesisUtterance(msg.text);
                            utterance.lang = 'en-IN';
                            window.speechSynthesis.speak(utterance);
                        };
                        footer.appendChild(speakBtn);
                    }
                }

                bubbleWrapper.appendChild(footer);
                messageDiv.appendChild(avatar);
                messageDiv.appendChild(bubbleWrapper);
                chatMessages.appendChild(messageDiv);
            });

            chatMessages.scrollTop = chatMessages.scrollHeight;
        }

        async function submitMessage(customText) {
            const text = customText || userInput.value.trim();
            if (!text) return;

            if (API_KEY === "PASTE_YOUR_API_KEY_HERE" || !API_KEY) {
                messages.push({ sender: 'user', text: text });
                messages.push({ sender: 'bot', text: "Paste your API key on line 6 of index.html, save, and refresh.", isError: true });
                saveStorage();
                renderMessages();
                userInput.value = '';
                return;
            }

            userInput.value = '';
            userInput.style.height = 'auto';

            messages.push({ sender: 'user', text: text });
            chatHistory.push({ role: 'user', text: text });
            saveStorage();
            renderMessages();

            // Show typing indicator
            const typingId = 'typing_' + Date.now();
            messages.push({ sender: 'bot', text: '<div class="typing-indicator"><div class="typing-dot"></div><div class="typing-dot"></div><div class="typing-dot"></div></div>', id: typingId, isTyping: true });
            renderMessages();
            sendBtn.disabled = true;

            try {
                const reply = await callAI(chatHistory);
                // Remove typing indicator
                messages = messages.filter(m => m.id !== typingId);
                messages.push({ sender: 'bot', text: reply });
                chatHistory.push({ role: 'assistant', text: reply });
                saveStorage();
            } catch (err) {
                messages = messages.filter(m => m.id !== typingId);
                messages.push({ sender: 'bot', text: err.message, isError: true });
            } finally {
                sendBtn.disabled = false;
                renderMessages();
            }
        }

        // Event Listeners
        sendBtn.onclick = () => submitMessage();

        userInput.onkeydown = (e) => {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                submitMessage();
            }
        };

        userInput.oninput = function() {
            this.style.height = 'auto';
            this.style.height = (this.scrollHeight) + 'px';
        };

        clearChatBtn.onclick = () => {
            if (confirm("Are you sure you want to clear chat history?")) {
                localStorage.removeItem('nexus_messages');
                messages = [];
                chatHistory = [];
                appendWelcomeMessage();
            }
        };

        themeToggleBtn.onclick = () => {
            const isDark = document.documentElement.getAttribute('data-theme') === 'dark';
            if (isDark) {
                document.documentElement.removeAttribute('data-theme');
                localStorage.setItem('nexus_theme', 'light');
                themeToggleBtn.textContent = '🌙';
            } else {
                document.documentElement.setAttribute('data-theme', 'dark');
                localStorage.setItem('nexus_theme', 'dark');
                themeToggleBtn.textContent = '☀️';
            }
        };

        // Speech Recognition Setup
        const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
        if (SpeechRecognition) {
            micBtn.style.display = 'flex';
            const recognition = new SpeechRecognition();
            recognition.lang = 'en-IN';
            recognition.interimResults = false;

            let isRecording = false;
            micBtn.onclick = () => {
                if (isRecording) {
                    recognition.stop();
                } else {
                    recognition.start();
                }
            };

            recognition.onstart = () => {
                isRecording = true;
                micBtn.classList.add('mic-active');
            };

            recognition.onresult = (e) => {
                const transcript = e.results[0][0].transcript;
                userInput.value += (userInput.value ? ' ' : '') + transcript;
                userInput.style.height = 'auto';
                userInput.style.height = (userInput.scrollHeight) + 'px';
            };

            recognition.onerror = () => {
                isRecording = false;
                micBtn.classList.remove('mic-active');
            };

            recognition.onend = () => {
                isRecording = false;
                micBtn.classList.remove('mic-active');
            };
        }

        // Initialize App
        loadStorage();
    </script>
</body>
</html>
