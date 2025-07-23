<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive AI-Native Sprint Playbook</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" crossorigin="anonymous" referrerpolicy="no-referrer" />
    <!-- Chosen Palette: Midnight Echo (Deep Purple/Black base, Electric Cyan/Magenta/Blue accents) -->
    <!-- Application Structure Plan: A single-page application with a fixed sidebar for navigation on desktop and a fixed tab bar for mobile. The main content area dynamically displays sections. This structure allows users to easily jump between the core philosophy, AI tool descriptions, key limitations, sprint phases, and best practices. The sprint phases will be presented as an interactive vertical timeline. Key concepts like the "Situational Awareness Gap" will be visualized as interactive diagrams. This non-linear, thematic approach enhances usability by letting users explore content based on their interests rather than following a rigid document structure. -->
    <!-- Visualization & Content Choices:
        - Core Philosophy: Presented with a prominent, large hero text layout. Includes an interactive text summarization demo using Gemini API and a Chart.js "Gemini Bell Curve" to visualize AI utility. Goal: Inform & Engage. Interaction: LLM demo, Chart interaction. Method: HTML/CSS with JS, Chart.js.
        - AI Companions: Interactive cards with icons and thematic images. Descriptions highlight specific use cases like JTBD Expert Gem and process map generation. Goal: Inform. Interaction: Hover effects on cards. Method: HTML/CSS with JS.
        - Situational Awareness Gap: A custom-built, multi-level interactive diagram using HTML/CSS/JS. Includes a new scenario comprehension demo and a scenario projection demo using Gemini API. Goal: Inform & Organize. Interaction: Click to reveal details, LLM demos. Method: HTML/CSS with JS.
        - Sprint Phases: An interactive vertical timeline. Goal: Show Change/Process. Descriptions are enhanced to detail UXR's role and AI tool integration in each phase. Interaction: Clicking on a phase highlights and adds an 'Ideation Partner' demo in Phase 4. Method: HTML/CSS with JS for navigation and LLM interaction.
        - Best Practices: Presented as an interactive accordion. Goal: Inform/Organize. Interaction: Toggling accordion reveals details, includes a 'Prompt Refiner' demo in 'Iterative Prompt Engineering'. Method: HTML/CSS with JS for toggling and LLM interaction.
        - NO SVG/Mermaid used. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        @keyframes glowing-border {
            0% { border-color: #00f0ff; box-shadow: 0 0 5px #00f0ff; }
            50% { border-color: #ff00ff; box-shadow: 0 0 15px #ff00ff; }
            100% { border-color: #00f0ff; box-shadow: 0 0 5px #00f0ff; }
        }

        @keyframes pulse-light {
            0% { text-shadow: 0 0 5px rgba(0,240,255,0.5); }
            50% { text-shadow: 0 0 15px rgba(255,0,255,0.7); }
            100% { text-shadow: 0 0 5px rgba(0,240,255,0.5); }
        }

        @keyframes background-gradient-move {
            0% { background-position: 0% 0%; }
            100% { background-position: 100% 100%; }
        }
        
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #1a0b2e 0%, #0d041e 100%);
            background-size: 200% 200%;
            animation: background-gradient-move 60s ease infinite alternate;
            color: #ffffff;
            overflow-x: hidden; /* Prevent horizontal scroll from subtle animations */
        }
        
        .glow-text {
            animation: pulse-light 3s infinite alternate;
        }

        .nav-link {
            transition: all 0.3s ease;
            background: linear-gradient(90deg, #3a1d5a 0%, #2f1847 100%);
        }
        .nav-link:hover {
            background: linear-gradient(90deg, #00f0ff 0%, #ff00ff 100%);
            color: #1a0b2e;
            transform: translateX(5px);
            box-shadow: 0 0 15px rgba(0,240,255,0.5);
        }
        .nav-link.active {
            background: linear-gradient(90deg, #00f0ff 0%, #ff00ff 100%);
            color: #1a0b2e;
            font-weight: 700;
            box-shadow: 0 0 10px rgba(0,240,255,0.5);
        }

        /* Mobile Tab Nav Styling */
        .mobile-tab-nav {
            position: sticky;
            top: 0;
            z-index: 50;
            background: linear-gradient(90deg, #2c1542 0%, #1a0b2e 100%);
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
            display: flex;
            justify-content: space-around;
            padding: 0.5rem 0;
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
            scrollbar-width: none; /* Firefox */
            -ms-overflow-style: none;  /* IE and Edge */
        }
        .mobile-tab-nav::-webkit-scrollbar { /* Chrome, Safari, Opera */
            display: none;
        }
        .mobile-tab-nav-item {
            flex-shrink: 0; /* Prevent items from shrinking */
            padding: 0.75rem 1rem;
            margin: 0 0.25rem;
            border-radius: 9999px; /* Pill shape */
            text-align: center;
            font-size: 0.875rem; /* text-sm */
            font-weight: 500;
            color: #ffffff;
            background: linear-gradient(90deg, #3a1d5a 0%, #2f1847 100%);
            transition: all 0.3s ease;
            white-space: nowrap; /* Prevent text wrapping */
            box-shadow: 0 2px 8px rgba(0,0,0,0.2);
        }
        .mobile-tab-nav-item:hover {
            background: linear-gradient(90deg, #00aaff 0%, #ff00ff 100%);
            color: #1a0b2e;
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0,240,255,0.4);
        }
        .mobile-tab-nav-item.active {
            background: linear-gradient(90deg, #00f0ff 0%, #ff00ff 100%);
            color: #1a0b2e;
            font-weight: 700;
            box-shadow: 0 0 15px rgba(0,240,255,0.6);
            position: relative;
        }
        .mobile-tab-nav-item.active::after {
            content: '';
            position: absolute;
            bottom: -5px; /* Adjust to position below button */
            left: 50%;
            transform: translateX(-50%);
            width: 80%; /* Width of the underline */
            height: 3px;
            background: linear-gradient(90deg, #ff00ff, #00f0ff);
            border-radius: 2px;
        }


        .content-section {
            display: none;
            opacity: 0;
            transform: translateY(10px);
        }
        .content-section.active {
            display: block;
            opacity: 1;
            transform: translateY(0);
            animation: fadeIn 0.5s ease-in-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .llm-button {
            background: linear-gradient(90deg, #00f0ff 0%, #ff00ff 100%);
            color: #1a0b2e;
            font-weight: 700;
            border-radius: 9999px; /* Pill shape */
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(0,240,255,0.3);
            display: inline-flex;
            align-items: center;
            justify-content: center;
        }
        .llm-button:hover {
            transform: translateY(-3px) scale(1.02);
            box-shadow: 0 8px 20px rgba(0,240,255,0.5);
        }
        .llm-button:active {
            transform: translateY(0) scale(0.98);
            box-shadow: 0 3px 10px rgba(0,240,255,0.2);
        }

        .card-glow {
            border: 1px solid transparent; /* Base for glowing border */
            box-shadow: 0 0 5px rgba(0,0,0,0); /* Base for glowing shadow */
            transition: all 0.3s ease;
        }
        .card-glow:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(0,240,255,0.3);
            border-image: linear-gradient(45deg, #00f0ff, #ff00ff) 1;
            animation: glowing-border 2s infinite alternate;
        }

        .section-title-gradient {
            background: linear-gradient(90deg, #00f0ff, #ff00ff);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            color: transparent;
        }

        .timeline-item::before {
            content: '';
            position: absolute;
            left: 19px;
            top: 0;
            bottom: 0;
            width: 2px;
            background: linear-gradient(to bottom, #00f0ff, #ff00ff);
        }
        .timeline-item:last-child::before {
            display: none;
        }
        .timeline-dot {
            position: absolute;
            left: 10px;
            top: 5px;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: linear-gradient(45deg, #00f0ff, #ff00ff);
            border: 3px solid #1a0b2e;
            box-shadow: 0 0 10px rgba(0,240,255,0.7);
        }
        .timeline-item:hover .timeline-dot {
            box-shadow: 0 0 20px rgba(255,0,255,0.9);
            transform: scale(1.1);
        }

        .accordion-content {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.4s ease-out;
        }
        .accordion-header {
            background: linear-gradient(90deg, #3a1d5a, #2f1847);
            border: 1px solid transparent;
            transition: all 0.3s ease;
        }
        .accordion-header:hover {
            background: linear-gradient(90deg, #00aaff, #ff00ff);
            color: #1a0b2e;
            box-shadow: 0 0 15px rgba(0,240,255,0.3);
            transform: translateY(-2px);
        }
        .accordion-header.active {
            background: linear-gradient(90deg, #00f0ff, #ff00ff);
            color: #1a0b2e;
            box-shadow: 0 0 20px rgba(0,240,255,0.5);
            font-weight: 700;
        }

        .typewriter-text {
            overflow: hidden;
            white-space: pre-wrap; /* Preserve whitespace and allow wrapping */
            word-wrap: break-word; /* Break long words */
            animation: typing 1.5s steps(40, end), blink-caret .75s step-end infinite;
        }

        /* The typing effect */
        @keyframes typing {
            from { width: 0 }
            to { width: 100% }
        }

        /* The caret blink effect */
        @keyframes blink-caret {
            from, to { border-color: transparent }
            50% { border-color: #00f0ff; }
        }

        /* Custom shape for hero image */
        .hero-image-container {
            position: relative;
            width: 100%;
            padding-bottom: 75%; /* Aspect ratio 4:3, adjust as needed */
            clip-path: ellipse(50% 40% at 50% 50%); /* Custom organic shape */
            overflow: hidden; /* Ensure image doesn't spill out */
        }
        .hero-image-container img {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        /* Card Image Styling */
        .card-image {
            width: 100%;
            height: 120px;
            object-fit: cover;
            border-radius: 0.75rem; /* rounded-xl */
            margin-bottom: 1rem;
            opacity: 0.8;
            transition: opacity 0.3s ease;
        }
        .card-glow:hover .card-image {
            opacity: 1;
        }

        /* Icon colors in sections */
        .icon-accent {
            color: #00f0ff;
        }

        .loader {
            border-top-color: #ff00ff;
            border-left-color: #00f0ff;
        }

        /* Chart Container Styling */
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 600px; /* Limits max width on large screens */
            margin-left: auto;
            margin-right: auto;
            height: 300px; /* Base height for the chart */
            max-height: 400px; /* Max height to prevent excessive vertical space */
            background-color: #1a0b2e; /* Match background for seamless look */
            border-radius: 1rem; /* rounded-xl */
            padding: 1.5rem; /* p-6 */
            box-shadow: 0 0 20px rgba(0, 240, 255, 0.2); /* Subtle glow */
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 350px;
            }
        }
    </style>
</head>
<body class="antialiased">

    <div class="flex min-h-screen">
        <!-- Sidebar Navigation (Desktop) -->
        <aside class="w-64 bg-[#2c1542] p-6 hidden lg:flex flex-col sticky top-0 h-screen shadow-lg">
            <h1 class="text-3xl font-extrabold section-title-gradient mb-10 text-center">Playbook</h1>
            <nav id="desktop-nav" class="flex flex-col space-y-3">
                <a href="#philosophy" class="nav-link p-3 rounded-lg text-white hover:text-[#1a0b2e] flex items-center"><i class="fas fa-brain mr-3 icon-accent"></i> Core Philosophy</a>
                <a href="#tools" class="nav-link p-3 rounded-lg text-white hover:text-[#1a0b2e] flex items-center"><i class="fas fa-tools mr-3 icon-accent"></i> Our AI Companions</a>
                <a href="#limitation" class="nav-link p-3 rounded-lg text-white hover:text-[#1a0b2e] flex items-center"><i class="fas fa-exclamation-triangle mr-3 icon-accent"></i> Situational Awareness Gap</a>
                <a href="#phases" class="nav-link p-3 rounded-lg text-white hover:text-[#1a0b2e] flex items-center"><i class="fas fa-rocket mr-3 icon-accent"></i> Sprint Phases</a>
                <a href="#practices" class="nav-link p-3 rounded-lg text-white hover:text-[#1a0b2e] flex items-center"><i class="fas fa-check-circle mr-3 icon-accent"></i> Best Practices</a>
            </nav>
        </aside>

        <!-- Main Content -->
        <main class="flex-1 p-4 sm:p-6 lg:p-10">
            
            <!-- Mobile Tab Navigation -->
            <nav id="mobile-tab-nav" class="mobile-tab-nav lg:hidden mb-6">
                <a href="#philosophy" class="mobile-tab-nav-item">Core Philosophy</a>
                <a href="#tools" class="mobile-tab-nav-item">AI Companions</a>
                <a href="#limitation" class="mobile-tab-nav-item">Situational Gap</a>
                <a href="#phases" class="mobile-tab-nav-item">Sprint Phases</a>
                <a href="#practices" class="mobile-tab-nav-item">Best Practices</a>
            </nav>

            <!-- Philosophy Section (Hero-like) -->
            <section id="philosophy" class="content-section min-h-screen flex items-center py-12">
                <div class="grid lg:grid-cols-1 gap-10 items-center w-full">
                    <div class="text-left">
                        <h2 class="text-5xl sm:text-6xl lg:text-7xl font-extrabold leading-tight section-title-gradient mb-6">
                            <span class="block glow-text">Welcome to the</span>
                            <span class="block">AI-Native World</span>
                        </h2>
                        <p class="text-lg sm:text-xl text-gray-200 mb-8 max-w-xl leading-relaxed">
                            Our journey revealed that while AI offers unprecedented acceleration, the most impactful outcomes stem from a strategic human-AI partnership. This is the core of the "Human-AI Collaborative Native" mindset.
                        </p>
                        <button onclick="window.location.hash='#practices'" class="llm-button px-8 py-3 text-lg">
                            Learn Best Practices <i class="fas fa-arrow-right ml-2"></i>
                        </button>
                    </div>
                </div>

                <!-- Gemini Bell Curve Chart -->
                <div class="chart-container mt-12 mb-8">
                    <canvas id="geminiBellCurveChart"></canvas>
                </div>
                <div class="bg-[#2f1847] p-8 rounded-xl shadow-lg border border-[#3a1d5a] card-glow w-full mt-12 mx-auto max-w-4xl">
                    <h3 class="font-bold text-2xl mb-4 text-[#00f0ff] flex items-center"><i class="fas fa-forward mr-3"></i> AI is an Accelerator</h3>
                    <p class="text-gray-200 mb-4">Experience how AI can quickly process information. Paste any text below for a rapid summary.</p>
                    <textarea id="summarize-input" class="w-full p-3 bg-[#1a0b2e] text-white rounded-lg border border-[#3a1d5a] text-sm mb-3 focus:outline-none focus:ring-2 focus:ring-[#00f0ff]" rows="4" placeholder="Enter text to summarize..."></textarea>
                    <button id="summarize-button" class="llm-button px-6 py-2.5 text-base">Summarize Text <i class="fas fa-compress-alt ml-2"></i></button>
                    <div id="summarize-output" class="mt-4 p-4 bg-[#1a0b2e] rounded-lg text-[#00f0ff] text-sm italic border border-[#ff00ff] min-h-[50px] typewriter-text-container"></div>
                    <div id="summarize-loading" class="mt-3 text-center hidden">
                        <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-[#00f0ff] loader mx-auto"></div>
                        <p class="text-sm text-gray-400 mt-2">AI is accelerating...</p>
                    </div>
                    <div id="summarize-error" class="mt-3 p-3 bg-red-800 text-red-200 text-sm rounded-md hidden"></div>
                </div>
            </section>

            <!-- AI Companions Section -->
            <section id="tools" class="content-section py-12">
                <h2 class="text-3xl font-bold section-title-gradient mb-8 flex items-center"><i class="fas fa-robot mr-4"></i> Our AI Companions in Sprint 0</h2>
                <p class="text-lg text-gray-200 mb-10">We actively incorporated specific AI tools as integral members of our sprint team, treating them as "AI companions" to accelerate various stages of our work.</p>
                <div class="grid md:grid-cols-2 gap-8">
                    <div class="bg-[#2f1847] p-8 rounded-xl shadow-lg border border-[#3a1d5a] card-glow">
                        <img src="https://placehold.co/600x300/2f1847/ff00ff?text=NotebookLM+Data" alt="NotebookLM data processing" class="card-image">
                        <h3 class="font-bold text-2xl mb-3 text-[#00f0ff] flex items-center"><i class="fas fa-book-open mr-3"></i> NotebookLM</h3>
                        <p class="text-gray-400 font-medium mb-4">Multi-modal Input Summarizer & Process Mapper</p>
                        <p class="text-gray-200 mb-4">This tool served as our central knowledge repository, capturing all info in a consolidated way (from recordings, meeting transcripts, framework documents). It was crucial for:</p>
                        <ul class="list-disc list-inside space-y-2 text-gray-200">
                            <li>Answering domain-specific questions instantly.</li>
                            <li>**Quickly generating Process maps and flows from recordings/transcripts, saving significant time reading through extensive documentation.**</li>
                        </ul>
                    </div>
                    <div class="bg-[#2f1847] p-8 rounded-xl shadow-lg border border-[#3a1d5a] card-glow">
                        <img src="https://placehold.co/600x300/2f1847/00f0ff?text=JTBD+AI+Guidance" alt="Gemini guiding JTBD process" class="card-image">
                        <h3 class="font-bold text-2xl mb-3 text-[#00f0ff] flex items-center"><i class="fas fa-gem mr-3"></i> Gemini's Expert Gems</h3>
                        <p class="text-gray-400 font-medium mb-4">Framework Expert & Brainstorming Partner</p>
                        <p class="text-gray-200 mb-4">We leveraged Gemini for deeper analysis and ideation, finding it a very helpful teammate for:</p>
                        <ul class="list-disc list-inside space-y-2 text-gray-200">
                            <li>Acting as a **JTBD Expert Gem, trained to learn the method and participate as a virtual team member guiding the team by asking and gathering information required to identify key Jobs.**</li>
                            <li>Brainstorming different categories of solutions (e.g., "opportunities in current processes," "ideal AV space," "transformative, AI-first opportunities") with frameworks like **5-Whys** for "blue-sky opportunities."</li>
                            <li>Assisting in summarizing complex findings and enabling rapid recall.</li>
                        </ul>
                    </div>
                </div>
            </section>

            <!-- Situational Awareness Gap Section -->
            <section id="limitation" class="content-section py-12">
                <h2 class="text-3xl font-bold section-title-gradient mb-8 flex items-center"><i class="fas fa-frown-open mr-4"></i> A Key Limitation: AI's Situational Awareness Gap</h2>
                <p class="text-lg text-gray-200 mb-10">A significant challenge identified was the AI's limited situational awareness. While AI excels at perception, it struggles with higher-order cognitive functions essential for true problem-solving. Interact with the levels below to learn more, and see a demo of its projection limitations.</p>
                <div class="space-y-6">
                    <!-- Level 1 -->
                    <div class="awareness-level bg-[#2f1847] p-6 rounded-xl shadow-lg border border-[#3a1d5a] card-glow cursor-pointer">
                        <div class="flex justify-between items-center">
                            <h3 class="font-bold text-xl text-[#00f0ff]">Level 1: Perception <i class="fas fa-eye ml-2"></i></h3>
                            <span class="text-green-400 font-semibold text-sm">AI Strength</span>
                        </div>
                        <div class="awareness-details hidden mt-4 text-gray-300">
                            <p>AI tools are excellent at taking raw inputs (transcripts, documents) and identifying discrete pieces of information. They "see" the data.</p>
                        </div>
                    </div>
                    <!-- Level 2 -->
                    <div class="awareness-level bg-[#2f1847] p-6 rounded-xl shadow-lg border border-[#3a1d5a] card-glow cursor-pointer">
                        <div class="flex justify-between items-center">
                            <h3 class="font-bold text-xl text-[#ff00ff]">Level 2: Comprehension <i class="fas fa-lightbulb ml-2"></i></h3>
                            <span class="text-yellow-400 font-semibold text-sm">Partial Strength / Human Needed</span>
                        </div>
                        <div class="awareness-details hidden mt-4 text-gray-300">
                            <p>AI can connect related pieces of information and summarize themes. However, it often misses implicit meanings, nuances, and the underlying *why* or *how* that human experience brings. It understands the *what*, but less of the *so what*.</p>
                            <div class="mt-6 pt-6 border-t border-[#3a1d5a]">
                                <h4 class="font-semibold text-[#ff00ff] mb-3 flex items-center"><i class="fas fa-robot mr-2"></i> Demo: Identify Key Themes</h4>
                                <textarea id="comprehension-input" class="w-full p-3 bg-[#1a0b2e] text-white rounded-lg border border-[#3a1d5a] text-sm mb-3 focus:outline-none focus:ring-2 focus:ring-[#00f0ff]" rows="4" placeholder="Enter text for theme identification (e.g., 'During shadowing, the technician repeatedly mentioned tools being left in other rooms, leading to significant time wasted in searching and retrieving them.')."></textarea>
                                <button id="comprehension-button" class="llm-button px-6 py-2.5 text-base">Get Key Themes <i class="fas fa-tags ml-2"></i></button>
                                <div id="comprehension-output" class="mt-4 p-4 bg-[#1a0b2e] rounded-lg text-[#00f0ff] text-sm italic border border-[#ff00ff] min-h-[50px] typewriter-text-container"></div>
                                <div id="comprehension-loading" class="mt-3 text-center hidden">
                                    <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-[#00f0ff] loader mx-auto"></div>
                                    <p class="text-sm text-gray-400 mt-2">AI is comprehending...</p>
                                </div>
                                <div id="comprehension-error" class="mt-3 p-3 bg-red-800 text-red-200 text-sm rounded-md hidden"></div>
                                <p class="mt-3 text-xs text-gray-500">Observe how the AI identifies themes, but consider if it fully grasps the human impact or subtle context.</p>
                            </div>
                        </div>
                    </div>
                    <!-- Level 3 -->
                    <div class="awareness-level bg-[#2f1847] p-6 rounded-xl shadow-lg border border-[#3a1d5a] card-glow cursor-pointer">
                        <div class="flex justify-between items-center">
                            <h3 class="font-bold text-xl text-[#00aaff]">Level 3: Projection <i class="fas fa-rocket ml-2"></i></h3>
                            <span class="text-red-400 font-semibold text-sm">Human Essential</span>
                        </div>
                        <div class="awareness-details hidden mt-4 text-gray-300">
                            <p>This is where AI significantly lags. It struggles to anticipate future states, consequences, or potential issues based on incomplete information. It lacks the ability to "read between the lines," which is crucial for identifying truly proactive or transformative opportunities.</p>
                            <div class="mt-6 pt-6 border-t border-[#3a1d5a]">
                                <h4 class="font-semibold text-[#00f0ff] mb-3 flex items-center"><i class="fas fa-robot mr-2"></i> Demo: AI's Projection Challenge</h4>
                                <textarea id="scenario-input" class="w-full p-3 bg-[#1a0b2e] text-white rounded-lg border border-[#3a1d5a] text-sm mb-3 focus:outline-none focus:ring-2 focus:ring-[#ff00ff]" rows="4" placeholder="Describe a simple scenario (e.g., 'A technician enters a meeting room where the projector is off, but all other devices are powered on.')."></textarea>
                                <button id="predict-button" class="llm-button px-6 py-2.5 text-base">Predict Outcome <i class="fas fa-paper-plane ml-2"></i></button>
                                <div id="predict-output" class="mt-4 p-4 bg-[#1a0b2e] rounded-lg text-[#00f0ff] text-sm italic border border-[#ff00ff] min-h-[50px] typewriter-text-container"></div>
                                <div id="predict-loading" class="mt-3 text-center hidden">
                                    <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-[#00f0ff] loader mx-auto"></div>
                                    <p class="text-sm text-gray-400 mt-2">AI is projecting...</p>
                                </div>
                                <div id="predict-error" class="mt-3 p-3 bg-red-800 text-red-200 text-sm rounded-md hidden"></div>
                                <p class="mt-3 text-xs text-gray-500">Observe how the AI might provide a plausible but generic prediction, lacking the nuanced foresight a human expert would have based on implicit context.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <!-- Sprint Phases Section -->
            <section id="phases" class="content-section py-12">
                <h2 class="text-3xl font-bold section-title-gradient mb-8 flex items-center"><i class="fas fa-sitemap mr-4"></i> Sprint Phases: A UXR's Strategic Engagement</h2>
                <p class="text-lg text-gray-200 mb-10">The AI-Native sprint is structured into distinct phases, each with a specific role for the UXR and strategic engagement of AI tools.</p>
                <div class="relative pl-6">
                    <!-- Phase 1 -->
                    <div class="timeline-item pl-12 pb-10">
                        <div class="timeline-dot"></div>
                        <h3 class="font-bold text-xl mb-2 text-[#00f0ff] flex items-center"><i class="fas fa-cogs mr-3"></i> Phase 1: Sprint Setup & Framing</h3>
                        <p class="text-gray-300">UXR collaborates with PMs to define the problem space, ensuring AI project expectations are set clearly. AI Tool: **NotebookLM** is key here, ingesting existing documentation (e.g., AVM Process overview, business alignment docs) to build a foundational knowledge base that all team members can quickly query.</p>
                    </div>
                    <!-- Phase 2 -->
                    <div class="timeline-item pl-12 pb-10">
                        <div class="timeline-dot"></div>
                        <h3 class="font-bold text-xl mb-2 text-[#00f0ff] flex items-center"><i class="fas fa-magnifying-glass-chart mr-3"></i> Phase 2: Discovery & Immersion</h3>
                        <p class="text-gray-300">UXR leads rich user research through technician shadowing and interviews (Day-in-life, Proactive Inspection, Break-fix, Escalation). AI Tool: **NotebookLM** is immediately used for automated transcription of recordings and summarizing meeting insights. It's crucial for quickly generating basic process maps and flows from these unstructured inputs, recovering context from internal documents, and allowing the team to grasp the basic AV maintenance process in seconds instead of reading extensive documentation.</p>
                    </div>
                    <!-- Phase 3 -->
                    <div class="timeline-item pl-12 pb-10">
                        <div class="timeline-dot"></div>
                        <h3 class="font-bold text-xl mb-2 text-[#00f0ff] flex items-center"><i class="fas fa-chart-line mr-3"></i> Phase 3: Analysis & Opportunity ID</h3>
                        <p class="text-gray-300">UXR leads the team in human-centric analysis using frameworks. AI Tools: A **Gemini Gem** is trained on the **Jobs-to-be-Done (JTBD)** framework to act as an expert, guiding the team by asking questions and gathering information required to identify key Jobs-to-be-Done in the current process. This helps identify opportunities. The **5-Whys framework** is leveraged for exploring "blue-sky" and transformative opportunities, where AI can assist in tracing root causes or generating different categories of solutions. NotebookLM supports by allowing quick recall of various aspects of collected data. This phase involves critical "human checkpoints" to ensure grounding and overcome AI's generic outputs.</p>
                    </div>
                    <!-- Phase 4 -->
                    <div class="timeline-item pl-12 pb-10">
                        <div class="timeline-dot"></div>
                        <h3 class="font-bold text-xl mb-2 text-[#00f0ff] flex items-center"><i class="fas fa-lightbulb mr-3"></i> Phase 4: Ideation & Solutioning</h3>
                        <p class="text-gray-300">UXR facilitates collaborative brainstorming, ensuring the team translates insights into actionable solutions. AI Tool: **Gemini** acts as a "divergent thinking partner" to challenge assumptions and generate micro-opportunities. However, the UXR's role is crucial in refining these often generic or futuristic AI outputs into grounded, implementable solutions.</p>
                        <div class="mt-6 pt-6 border-t border-[#3a1d5a]">
                            <h4 class="font-semibold text-[#ff00ff] mb-3 flex items-center"><i class="fas fa-brain mr-2"></i> Demo: Ideation Partner</h4>
                            <textarea id="ideation-input" class="w-full p-3 bg-[#1a0b2e] text-white rounded-lg border border-[#3a1d5a] text-sm mb-3 focus:outline-none focus:ring-2 focus:ring-[#00f0ff]" rows="4" placeholder="Enter a problem (e.g., 'How to reduce technician travel time for routine inspections?')."></textarea>
                            <button id="ideation-button" class="llm-button px-6 py-2.5 text-base">Brainstorm Ideas <i class="fas fa-rocket ml-2"></i></button>
                            <div id="ideation-output" class="mt-4 p-4 bg-[#1a0b2e] rounded-lg text-[#00f0ff] text-sm italic border border-[#ff00ff] min-h-[50px] typewriter-text-container"></div>
                            <div id="ideation-loading" class="mt-3 text-center hidden">
                                <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-[#00f0ff] loader mx-auto"></div>
                                <p class="text-sm text-gray-400 mt-2">AI is brainstorming...</p>
                            </div>
                            <div id="ideation-error" class="mt-3 p-3 bg-red-800 text-red-200 text-sm rounded-md hidden"></div>
                        </div>
                    </div>
                    <!-- Phase 5 -->
                    <div class="timeline-item pl-12">
                        <div class="timeline-dot"></div>
                        <h3 class="font-bold text-xl mb-2 text-[#00f0ff] flex items-center"><i class="fas fa-vial mr-3"></i> Phase 5: MVT & Prototyping</h3>
                        <p class="text-gray-300">UXR, in collaboration with the team, defines the Minimum Viable Test (MVT) scope. AI Tool: AI can assist in classifying data for validation sets and brainstorming Key Performance Indicators (KPIs). All proposed solutions and insights are then rigorously validated with real users to ensure they are grounded in reality and actionable.</p>
                    </div>
                </div>
            </section>

            <!-- Best Practices Section -->
            <section id="practices" class="content-section py-12">
                <h2 class="text-3xl font-bold section-title-gradient mb-8 flex items-center"><i class="fas fa-star mr-4"></i> Best Practices</h2>
                <p class="text-lg text-gray-200 mb-10">Key principles for effective AI engagement sprints.</p>
                <div id="accordion" class="space-y-5">
                    <div class="accordion-item bg-[#2f1847] rounded-xl border border-[#3a1d5a] shadow-lg card-glow">
                        <button class="accordion-header w-full p-5 text-left font-semibold text-lg flex justify-between items-center rounded-xl">
                            Iterative Prompt Engineering
                            <svg class="w-6 h-6 transform transition-transform" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg>
                        </button>
                        <div class="accordion-content px-5 pb-5 text-gray-300">
                            <p>Treat prompts as design artifacts. Continuously refine them based on output quality and observed "AI behavior" (e.g., repetition, generic responses).</p>
                            <div class="mt-6 pt-6 border-t border-[#3a1d5a]">
                                <h4 class="font-semibold text-[#ff00ff] mb-3 flex items-center"><i class="fas fa-magic mr-2"></i> Demo: Prompt Refiner</h4>
                                <textarea id="prompt-refine-input" class="w-full p-3 bg-[#1a0b2e] text-white rounded-lg border border-[#3a1d5a] text-sm mb-3 focus:outline-none focus:ring-2 focus:ring-[#00f0ff]" rows="4" placeholder="Enter a generic prompt (e.g., 'Tell me about user needs.')."></textarea>
                                <button id="prompt-refine-button" class="llm-button px-6 py-2.5 text-base">Refine Prompt <i class="fas fa-pen-nib ml-2"></i></button>
                                <div id="prompt-refine-output" class="mt-4 p-4 bg-[#1a0b2e] rounded-lg text-[#00f0ff] text-sm italic border border-[#ff00ff] min-h-[50px] typewriter-text-container"></div>
                                <div id="prompt-refine-loading" class="mt-3 text-center hidden">
                                    <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-[#00f0ff] loader mx-auto"></div>
                                    <p class="text-sm text-gray-400 mt-2">AI is refining...</p>
                                </div>
                                <div id="prompt-refine-error" class="mt-3 p-3 bg-red-800 text-red-200 text-sm rounded-md hidden"></div>
                            </div>
                        </div>
                    </div>
                     <div class="accordion-item bg-[#2f1847] rounded-xl border border-[#3a1d5a] shadow-lg card-glow">
                        <button class="accordion-header w-full p-5 text-left font-semibold text-lg flex justify-between items-center rounded-xl">
                            Explicit Context & Constraints
                            <svg class="w-6 h-6 transform transition-transform" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg>
                        </button>
                        <div class="accordion-content px-5 pb-5 text-gray-300">
                            <p>The more specific and detailed the context you provide, the better the output. Include details about user personas, technical limitations, business goals, and current processes.</p>
                        </div>
                    </div>
                     <div class="accordion-item bg-[#2f1847] rounded-xl border border-[#3a1d5a] shadow-lg card-glow">
                        <button class="accordion-header w-full p-5 text-left font-semibold text-lg flex justify-between items-center rounded-xl">
                            Human Over the Loop
                            <svg class="w-6 h-6 transform transition-transform" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg>
                        </button>
                        <div class="accordion-content px-5 pb-5 text-gray-300">
                            <p>Never blindly accept AI outputs. Always apply human judgment, critical thinking, and contextual understanding. AI is an augment, not a replacement for human intellect and empathy.</p>
                        </div>
                    </div>
                    <div class="accordion-item bg-[#2f1847] rounded-xl border border-[#3a1d5a] shadow-lg card-glow">
                        <button class="accordion-header w-full p-5 text-left font-semibold text-lg flex justify-between items-center rounded-xl">
                            Champion Collaborative Tooling
                            <svg class="w-6 h-6 transform transition-transform" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg>
                        </button>
                        <div class="accordion-content px-5 pb-5 text-gray-300">
                            <p>Advocate for and experiment with AI platforms that support shared workspaces, collaborative prompting, and multi-modal outputs (beyond just text) to break the isolating "prompt and answer loop."</p>
                        </div>
                    </div>
                </div>
            </section>

        </main>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const sections = document.querySelectorAll('.content-section');
            const desktopNavLinks = document.querySelectorAll('#desktop-nav .nav-link');
            const mobileTabNavItems = document.querySelectorAll('#mobile-tab-nav .mobile-tab-nav-item');

            function showSection(hash) {
                const targetHash = hash || '#philosophy';
                
                sections.forEach(section => {
                    if ('#' + section.id === targetHash) {
                        section.classList.add('active');
                    } else {
                        section.classList.remove('active');
                    }
                });

                desktopNavLinks.forEach(link => {
                    if (link.getAttribute('href') === targetHash) {
                        link.classList.add('active');
                    } else {
                        link.classList.remove('active');
                    }
                });

                mobileTabNavItems.forEach(item => {
                    if (item.getAttribute('href') === targetHash) {
                        item.classList.add('active');
                    } else {
                        item.classList.remove('active');
                    }
                });
            }

            function handleNavClick(e) {
                e.preventDefault();
                const targetHash = e.target.getAttribute('href');
                window.location.hash = targetHash;
            }

            desktopNavLinks.forEach(link => {
                link.addEventListener('click', handleNavClick);
            });
            
            mobileTabNavItems.forEach(item => {
                item.addEventListener('click', handleNavClick);
            });

            window.addEventListener('hashchange', () => {
                showSection(window.location.hash);
            });
            
            showSection(window.location.hash);

            // Accordion functionality
            const accordionHeaders = document.querySelectorAll('.accordion-header');
            accordionHeaders.forEach(header => {
                header.addEventListener('click', () => {
                    const content = header.nextElementSibling;
                    const svg = header.querySelector('svg');

                    if (content.style.maxHeight) {
                        content.style.maxHeight = null;
                        svg.classList.remove('rotate-180');
                        header.classList.remove('active');
                    } else {
                        // Close other accordions
                        document.querySelectorAll('.accordion-content').forEach(c => c.style.maxHeight = null);
                        document.querySelectorAll('.accordion-header svg').forEach(s => s.classList.remove('rotate-180'));
                        document.querySelectorAll('.accordion-header').forEach(h => h.classList.remove('active'));

                        content.style.maxHeight = content.scrollHeight + "px";
                        svg.classList.add('rotate-180');
                        header.classList.add('active');
                    }
                });
            });

            // Situational Awareness functionality
            const awarenessLevels = document.querySelectorAll('.awareness-level');
            awarenessLevels.forEach(level => {
                level.addEventListener('click', () => {
                    const details = level.querySelector('.awareness-details');
                    details.classList.toggle('hidden');
                });
            });

            // Typewriter effect function
            function typeWriter(element, text, i = 0, speed = 30) {
                if (element.typingTimeout) {
                    clearTimeout(element.typingTimeout);
                }
                element.textContent = ''; // Clear content before starting
                element.style.width = '0%'; // Reset width for animation

                function type() {
                    if (i < text.length) {
                        element.textContent += text.charAt(i);
                        element.style.width = ((i + 1) / text.length) * 100 + '%'; // Update width
                        i++;
                        element.typingTimeout = setTimeout(type, speed);
                    } else {
                         element.style.width = '100%'; // Ensure full width after typing
                    }
                    // Stop blinking caret after typing is done
                    if (i === text.length) {
                        element.style.animation = 'none';
                        element.style.borderRight = 'none';
                    } else {
                        element.style.animation = 'typing 1.5s steps(40, end), blink-caret .75s step-end infinite';
                        element.style.borderRight = '2px solid #00f0ff'; /* Show blinking caret */
                    }
                }
                type();
            }

            // Gemini API Integration Logic
            async function callGeminiAPI(prompt, outputElement, loadingElement, errorElement, isTypewriter = true) {
                // Stop any ongoing typewriter animation
                if (outputElement.typingTimeout) {
                    clearTimeout(outputElement.typingTimeout);
                }
                outputElement.textContent = '';
                outputElement.style.width = '0%'; // Reset width for next animation
                outputElement.style.animation = 'none'; // Stop blinking caret immediately
                outputElement.style.borderRight = 'none'; // Hide caret

                errorElement.textContent = '';
                loadingElement.classList.remove('hidden');
                errorElement.classList.add('hidden'); 
                
                let chatHistory = [];
                chatHistory.push({ role: "user", parts: [{ text: prompt }] });
                const payload = { contents: chatHistory };
                const apiKey = ""; 
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

                let retryCount = 0;
                const maxRetries = 5;
                const baseDelay = 1000; // 1 second

                while (retryCount < maxRetries) {
                    try {
                        const response = await fetch(apiUrl, {
                            method: 'POST',
                            headers: { 'Content-Type': 'application/json' },
                            body: JSON.stringify(payload)
                        });

                        if (!response.ok) {
                            if (response.status === 429) { 
                                retryCount++;
                                const delay = baseDelay * Math.pow(2, retryCount - 1);
                                await new Promise(res => setTimeout(res, delay));
                                continue;
                            } else {
                                throw new Error(`API error: ${response.status} ${response.statusText}`);
                            }
                        }

                        const result = await response.json();
                        if (result.candidates && result.candidates.length > 0 &&
                            result.candidates[0].content && result.candidates[0].content.parts &&
                            result.candidates[0].content.parts.length > 0) {
                            const text = result.candidates[0].content.parts[0].text;
                            if (isTypewriter) {
                                outputElement.textContent = ''; 
                                typeWriter(outputElement, text);
                            } else {
                                outputElement.textContent = text;
                            }
                        } else {
                            outputElement.textContent = "No valid response from AI.";
                        }
                        return; 
                    } catch (error) {
                        errorElement.textContent = `Error: ${error.message}. Please try again.`;
                        errorElement.classList.remove('hidden');
                        return; 
                    } finally {
                        loadingElement.classList.add('hidden');
                    }
                }
                loadingElement.classList.add('hidden');
                errorElement.textContent = "Max retries exceeded. Please try again later.";
                errorElement.classList.remove('hidden');
            }

            // Helper to get and create typewriter output element
            function getOrCreateTypewriterOutput(containerId) {
                const container = document.getElementById(containerId);
                let typewriterDiv = container.querySelector('.typewriter-text');
                if (!typewriterDiv) {
                    typewriterDiv = document.createElement('div');
                    typewriterDiv.classList.add('typewriter-text');
                    container.innerHTML = ''; // Clear container to add new typewriter div
                    container.appendChild(typewriterDiv);
                } else {
                    typewriterDiv.textContent = ''; // Clear existing content
                }
                return typewriterDiv;
            }

            // Summarize Text Feature
            const summarizeInput = document.getElementById('summarize-input');
            const summarizeButton = document.getElementById('summarize-button');
            const summarizeOutput = getOrCreateTypewriterOutput('summarize-output');
            const summarizeLoading = document.getElementById('summarize-loading');
            const summarizeError = document.getElementById('summarize-error');

            summarizeButton.addEventListener('click', () => {
                const textToSummarize = summarizeInput.value.trim();
                if (textToSummarize) {
                    const prompt = `Summarize the following text concisely:\n\n${textToSummarize}`;
                    callGeminiAPI(prompt, summarizeOutput, summarizeLoading, summarizeError);
                } else {
                    summarizeError.textContent = "Please enter some text to summarize.";
                    summarizeError.classList.remove('hidden');
                }
            });

            // Predict Scenario Feature
            const scenarioInput = document.getElementById('scenario-input');
            const predictButton = document.getElementById('predict-button');
            const predictOutput = getOrCreateTypewriterOutput('predict-output');
            const predictLoading = document.getElementById('predict-loading');
            const predictError = document.getElementById('predict-error');

            predictButton.addEventListener('click', () => {
                const scenarioText = scenarioInput.value.trim();
                if (scenarioText) {
                    const prompt = `Given the following scenario, predict a plausible immediate outcome or consequence. Be concise and specific, do not explain your reasoning:\n\nScenario: "${scenarioText}"`;
                    callGeminiAPI(prompt, predictOutput, predictLoading, predictError);
                } else {
                    predictError.textContent = "Please describe a scenario to predict its outcome.";
                    predictError.classList.remove('hidden');
                }
            });

            // Comprehension Feature (New)
            const comprehensionInput = document.getElementById('comprehension-input');
            const comprehensionButton = document.getElementById('comprehension-button');
            const comprehensionOutput = getOrCreateTypewriterOutput('comprehension-output');
            const comprehensionLoading = document.getElementById('comprehension-loading');
            const comprehensionError = document.getElementById('comprehension-error');

            comprehensionButton.addEventListener('click', () => {
                const textForComprehension = comprehensionInput.value.trim();
                if (textForComprehension) {
                    const prompt = `Based on the following text, identify 3-5 key themes or main ideas:\n\nText: "${textForComprehension}"`;
                    callGeminiAPI(prompt, comprehensionOutput, comprehensionLoading, comprehensionError);
                } else {
                    comprehensionError.textContent = "Please enter text for theme identification.";
                    comprehensionError.classList.remove('hidden');
                }
            });

            // Ideation Partner Feature
            const ideationInput = document.getElementById('ideation-input');
            const ideationButton = document.getElementById('ideation-button');
            const ideationOutput = getOrCreateTypewriterOutput('ideation-output');
            const ideationLoading = document.getElementById('ideation-loading');
            const ideationError = document.getElementById('ideation-error');

            ideationButton.addEventListener('click', () => {
                const problem = ideationInput.value.trim();
                if (problem) {
                    const prompt = `Brainstorm 5 distinct and creative ideas to solve the following problem:\n\nProblem: "${problem}"`;
                    callGeminiAPI(prompt, ideationOutput, ideationLoading, ideationError);
                } else {
                    ideationError.textContent = "Please enter a problem to brainstorm ideas.";
                    ideationError.classList.remove('hidden');
                }
            });

            // Prompt Refiner Feature
            const promptRefineInput = document.getElementById('prompt-refine-input');
            const promptRefineButton = document.getElementById('prompt-refine-button');
            const promptRefineOutput = getOrCreateTypewriterOutput('prompt-refine-output');
            const promptRefineLoading = document.getElementById('prompt-refine-loading');
            const promptRefineError = document.getElementById('prompt-refine-error');

            promptRefineButton.addEventListener('click', () => {
                const promptToRefine = promptRefineInput.value.trim();
                if (promptToRefine) {
                    const prompt = `Refine the following prompt to make it more specific, provide better context, and include persona-based instructions if applicable. Suggest improvements rather than rewriting completely:\n\nOriginal Prompt: "${promptToRefine}"`;
                    callGeminiAPI(prompt, promptRefineOutput, promptRefineLoading, promptRefineError);
                } else {
                    promptRefineError.textContent = "Please enter a prompt to refine.";
                    promptRefineError.classList.remove('hidden');
                }
            });

            // Chart.js Bell Curve
            const ctx = document.getElementById('geminiBellCurveChart').getContext('2d');
            const gradientStroke = ctx.createLinearGradient(0, 0, 0, 300);
            gradientStroke.addColorStop(0, 'rgba(0, 240, 255, 0.8)');
            gradientStroke.addColorStop(0.5, 'rgba(255, 0, 255, 0.6)');
            gradientStroke.addColorStop(1, 'rgba(0, 240, 255, 0.1)');

            new Chart(ctx, {
                type: 'line',
                data: {
                    labels: ['Sprint Start', 'Initial Momentum', 'Peak Utility', 'Diminishing Returns', 'Stagnation/Repetition', 'Human Intervention', 'Sustained Value'],
                    datasets: [{
                        label: 'AI Utility',
                        data: [1, 3, 5, 4, 2, 3.5, 4], // Data points mimicking a bell curve shape
                        borderColor: '#00f0ff',
                        backgroundColor: gradientStroke,
                        fill: true,
                        tension: 0.4,
                        pointBackgroundColor: ['#00f0ff', '#00f0ff', '#ff00ff', '#ff00ff', '#ff00ff', '#00f0ff', '#00f0ff'], // Highlight points
                        pointBorderColor: ['#00f0ff', '#00f0ff', '#ff00ff', '#ff00ff', '#ff00ff', '#00f0ff', '#00f0ff'],
                        pointRadius: 6,
                        pointHoverRadius: 8
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false // Hide legend
                        },
                        tooltip: {
                            callbacks: {
                                title: function(context) {
                                    return context[0].label;
                                },
                                label: function(context) {
                                    const stage = context.label;
                                    const value = context.raw;
                                    let description = '';
                                    if (stage === 'Initial Momentum') description = 'AI provides significant momentum, summarizing & generating initial ideas.';
                                    else if (stage === 'Peak Utility') description = 'AI is highly effective, accelerating tasks quickly.';
                                    else if (stage === 'Diminishing Returns') description = 'AI starts to slow the team down, requiring human input.';
                                    else if (stage === 'Stagnation/Repetition') description = 'AI starts repeating itself, leading to a feeling of stagnation.';
                                    else if (stage === 'Human Intervention') description = 'Human input becomes crucial to break loops and refine insights.';
                                    else if (stage === 'Sustained Value') description = 'Balanced Human-AI collaboration leads to continuous value.';
                                    return [description, `Utility Score: ${value}`];
                                }
                            },
                            backgroundColor: '#2f1847',
                            titleColor: '#00f0ff',
                            bodyColor: '#ffffff',
                            borderColor: '#ff00ff',
                            borderWidth: 1,
                            cornerRadius: 8,
                            padding: 12
                        }
                    },
                    scales: {
                        x: {
                            title: {
                                display: true,
                                text: 'Sprint Progression',
                                color: '#ffffff',
                                font: {
                                    size: 14,
                                    weight: 'bold'
                                }
                            },
                            ticks: {
                                color: '#ccc'
                            },
                            grid: {
                                color: 'rgba(255,255,255,0.1)'
                            }
                        },
                        y: {
                            title: {
                                display: true,
                                text: 'AI Utility / Effectiveness',
                                color: '#ffffff',
                                font: {
                                    size: 14,
                                    weight: 'bold'
                                }
                            },
                            ticks: {
                                color: '#ccc'
                            },
                            grid: {
                                color: 'rgba(255,255,255,0.1)'
                            },
                            min: 0
                        }
                    }
                }
            });
        });
    </script>
</body>
</html>
```
