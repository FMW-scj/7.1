<html lang="en" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #fdfaf6;
            color: #4a4a4a;
        }
        .flashcard-inner {
            transition: transform 0.6s;
            transform-style: preserve-3d;
        }
        .flashcard.flipped .flashcard-inner {
            transform: rotateY(180deg);
        }
        .flashcard-front, .flashcard-back, .matching-card-content {
            -webkit-backface-visibility: hidden;
            backface-visibility: hidden;
        }
        .flashcard-back {
            transform: rotateY(180deg);
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 450px;
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 400px;
        }
        @media (min-width: 640px) {
            .chart-container {
                height: 350px;
            }
        }
        .tab-active {
            background-color: #3b82f6;
            color: #ffffff;
            border-color: #3b82f6;
        }
        .tab-inactive {
            background-color: #ffffff;
            color: #3b82f6;
            border-color: #3b82f6;
        }
        .matching-card {
            width: 100px;
            height: 100px;
            cursor: pointer;
            position: relative;
            background-color: #3b82f6;
            color: white;
            border-radius: 0.75rem;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            font-weight: 700;
            transition: background-color 0.3s ease;
        }
        .matching-card.is-revealed {
            background-color: #fef3c7;
            color: #4a4a4a;
        }
        .matched {
            visibility: hidden;
        }
        .hidden {
            display: none;
        }
        .chat-message {
            padding: 0.75rem;
            border-radius: 0.75rem;
            margin-bottom: 0.5rem;
            max-width: 85%;
        }
        .user-message {
            background-color: #dbeafe;
            margin-left: auto;
        }
        .ai-message {
            background-color: #e5e7eb;
            margin-right: auto;
        }
        .loader {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #3b82f6;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
</head>
<body class="antialiased">

    <header class="bg-white/80 backdrop-blur-lg sticky top-0 z-50 shadow-sm">
        <div class="container mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <h1 class="text-xl sm:text-2xl font-bold text-blue-600">Unit 7.1: Air</h1>
                <nav class="hidden md:flex space-x-6 text-sm font-medium">
                    <a href="#composition" class="text-gray-600 hover:text-blue-600 transition-colors">Composition of Air</a>
                    <a href="#properties" class="text-gray-600 hover:text-blue-600 transition-colors">Gas Properties</a>
                    <a href="#flashcards" class="text-gray-600 hover:text-blue-600 transition-colors">Flashcards</a>
                    <a href="#games" class="text-gray-600 hover:text-blue-600 transition-colors">Games</a>
                </nav>
            </div>
        </div>
    </header>

    <main class="container mx-auto px-4 sm:px-6 lg:px-8 py-8 sm:py-12">

        <section id="introduction" class="text-center mb-16">
            <h2 class="text-3xl font-bold text-gray-800 mb-4">Welcome, F.2 Scientists!</h2>
            <p class="max-w-3xl mx-auto text-lg text-gray-600">This interactive guide will help you explore the essential topic of "Living Things and Air". We'll learn what air is made of, discover the properties of different gases, and even learn how to identify them. Use the navigation above or scroll down to begin your journey!</p>
        </section>

        <section id="composition" class="mb-16 scroll-mt-20">
            <h2 class="text-2xl sm:text-3xl font-bold text-center text-gray-800 mb-2">What is Air Made Of?</h2>
            <p class="text-center text-gray-600 mb-8 max-w-2xl mx-auto">Air is a mixture of different gases. The chart below shows the main components. Hover over any section to see the details. You'll see that air is mostly nitrogen!</p>
            <div class="chart-container">
                <canvas id="airCompositionChart"></canvas>
            </div>
        </section>

        <section id="properties" class="mb-16 scroll-mt-20">
            <h2 class="text-2xl sm:text-3xl font-bold text-center text-gray-800 mb-2">Properties & Uses of Gases</h2>
            <p class="text-center text-gray-600 mb-8 max-w-2xl mx-auto">The gases in the air have unique properties that make them very useful. Select a gas below to learn more about it.</p>
            <div class="flex justify-center space-x-2 sm:space-x-4 mb-8">
                <button data-gas="oxygen" class="gas-tab tab-active py-2 px-4 rounded-full font-semibold border-2 transition-all">Oxygen</button>
                <button data-gas="carbon-dioxide" class="gas-tab tab-inactive py-2 px-4 rounded-full font-semibold border-2 transition-all">Carbon Dioxide</button>
                <button data-gas="nitrogen" class="gas-tab tab-inactive py-2 px-4 rounded-full font-semibold border-2 transition-all">Nitrogen</button>
            </div>
            <div id="gas-info-container" class="bg-white p-6 sm:p-8 rounded-xl shadow-lg border border-gray-200 transition-all duration-300">
                <div id="oxygen-info" class="gas-info">
                    <h3 class="text-2xl font-bold text-blue-600 mb-4">Oxygen</h3>
                    <div class="space-y-4 text-gray-700">
                        <div>
                            <p class="font-semibold text-lg mb-1">Test:</p>
                            <ul class="list-disc list-inside space-y-2">
                                <li>Relights a glowing splint</li>
                            </ul>
                        </div>
                        <div>
                            <p class="font-semibold text-lg mb-1">Properties:</p>
                            <ul class="list-disc list-inside space-y-2">
                                <li>Supports burning</li>
                            </ul>
                        </div>
                        <div>
                            <p class="font-semibold text-lg mb-1">Examples of Uses:</p>
                            <ul class="list-disc list-inside space-y-2">
                                <li>Mixed with a fuel gas to produce a flame for cutting and welding metals.</li>
                                <li>Used to help patients with breathing difficulties.</li>
                            </ul>
                        </div>
                    </div>
                </div>
                <div id="carbon-dioxide-info" class="gas-info hidden">
                    <h3 class="text-2xl font-bold text-green-600 mb-4">Carbon Dioxide</h3>
                    <div class="space-y-4 text-gray-700">
                        <div>
                            <p class="font-semibold text-lg mb-1">Test:</p>
                            <ul class="list-disc list-inside space-y-2">
                                <li>Turns hydrogencarbonate indicator from red to yellow</li>
                                <li>Turns lime water from colourless to milky</li>
                            </ul>
                        </div>
                        <div>
                            <p class="font-semibold text-lg mb-1">Properties:</p>
                            <ul class="list-disc list-inside space-y-2">
                                <li>Does not support burning</li>
                                <li>Denser than air</li>
                                <li>Changes from solid to gas directly at –78 °C</li>
                            </ul>
                        </div>
                        <div>
                            <p class="font-semibold text-lg mb-1">Examples of Uses:</p>
                            <ul class="list-disc list-inside space-y-2">
                                <li>Used in some fire extinguishers</li>
                                <li>Used on the stage to produce the effect of dense fog</li>
                                <li>Used to keep frozen food cold</li>
                            </ul>
                        </div>
                    </div>
                </div>
                <div id="nitrogen-info" class="gas-info hidden">
                    <h3 class="text-2xl font-bold text-purple-600 mb-4">Nitrogen</h3>
                    <div class="space-y-4 text-gray-700">
                        <div>
                            <p class="font-semibold text-lg mb-1">Test:</p>
                            <ul class="list-disc list-inside space-y-2">
                                <li>No simple test</li>
                            </ul>
                        </div>
                        <div>
                            <p class="font-semibold text-lg mb-1">Properties:</p>
                            <ul class="list-disc list-inside space-y-2">
                                <li>Unreactive</li>
                                <li>Very low boiling point (–196 °C)</li>
                            </ul>
                        </div>
                        <div>
                            <p class="font-semibold text-lg mb-1">Examples of Uses:</p>
                            <ul class="list-disc list-inside space-y-2">
                                <li>Used in food packaging</li>
                                <li>Used in the storage of living cells or tissues</li>
                                <li>Used to freeze food rapidly</li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </section>
        
        <section id="flashcards" class="scroll-mt-20 mb-16">
            <h2 class="text-2xl sm:text-3xl font-bold text-center text-gray-800 mb-2">Vocabulary Flashcards</h2>
            <p class="text-center text-gray-600 mb-8 max-w-2xl mx-auto">Click on a card to flip it and see the Chinese translation. Use the buttons to practice all the key terms from this unit. Click the "Listen" button to hear the English word.</p>
            <div class="w-full max-w-md mx-auto">
                <div id="flashcard" class="flashcard w-full h-52 perspective-1000 cursor-pointer">
                    <div id="flashcard-inner" class="flashcard-inner relative w-full h-full text-center">
                        <div id="flashcard-front" class="flashcard-front absolute w-full h-full flex items-center justify-center bg-white rounded-xl shadow-xl border border-blue-200">
                            <p class="text-3xl font-bold text-blue-700"></p>
                        </div>
                        <div id="flashcard-back" class="flashcard-back absolute w-full h-full flex items-center justify-center bg-blue-500 rounded-xl shadow-xl">
                             <p class="text-3xl font-bold text-white"></p>
                        </div>
                    </div>
                </div>
                <div class="flex justify-between items-center mt-6">
                    <button id="prev-btn" class="py-2 px-5 bg-white border-2 border-gray-300 rounded-lg font-semibold text-gray-700 hover:bg-gray-100 transition-colors">Previous</button>
                    <p id="card-counter" class="text-sm font-medium text-gray-500">1 / 24</p>
                    <button id="next-btn" class="py-2 px-5 bg-white border-2 border-gray-300 rounded-lg font-semibold text-gray-700 hover:bg-gray-100 transition-colors">Next</button>
                </div>
                 <div class="flex justify-center items-center space-x-4 mt-4">
                     <button id="shuffle-btn" class="py-2 px-5 bg-blue-600 text-white rounded-lg font-semibold hover:bg-blue-700 transition-colors">Shuffle Cards</button>
                     <button id="audio-btn" class="py-2 px-5 bg-green-500 text-white rounded-lg font-semibold hover:bg-green-600 transition-colors flex items-center justify-center space-x-2">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.293-4.293A1 1 0 0112 5v14a1 1 0 01-1.707.707L5.586 15z"></path></svg>
                        <span>Listen</span>
                     </button>
                 </div>
            </div>
        </section>

        <section id="games" class="scroll-mt-20 mb-16">
            <h2 class="text-2xl sm:text-3xl font-bold text-center text-gray-800 mb-2">Interactive Games</h2>
            <p class="text-center text-gray-600 mb-8 max-w-2xl mx-auto">Challenge yourself with a multiple-choice quiz or a fun matching game to test your knowledge of Unit 7!</p>

            <div class="grid md:grid-cols-2 gap-8">
                <!-- Quiz Game Section -->
                <div class="bg-white p-6 sm:p-8 rounded-xl shadow-lg border border-gray-200">
                    <h3 class="text-xl font-bold text-blue-600 mb-4 text-center">Knowledge Quiz</h3>
                    <div id="quiz-intro">
                        <p class="mb-4 text-gray-700 text-center">Test your knowledge on the properties and tests of different gases.</p>
                        <form id="quiz-name-form" class="flex flex-col items-center">
                            <label for="quiz-name" class="block mb-2 font-semibold">Please enter your name:</label>
                            <input type="text" id="quiz-name" class="w-full max-w-xs px-4 py-2 mb-4 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Your Name" required>
                            <button type="submit" class="py-2 px-6 bg-blue-600 text-white rounded-lg font-semibold hover:bg-blue-700 transition-colors">Start Quiz</button>
                        </form>
                    </div>
                    <div id="quiz-game" class="hidden">
                        <div id="question-container" class="mb-6">
                            <p id="quiz-question" class="text-lg font-semibold text-gray-800 mb-4"></p>
                            <div id="quiz-options" class="space-y-3"></div>
                            <div id="quiz-feedback" class="mt-4 text-sm"></div>
                        </div>
                        <div class="text-center">
                            <button id="quiz-next-btn" class="py-2 px-6 bg-blue-600 text-white rounded-lg font-semibold hover:bg-blue-700 transition-colors" disabled>Next Question</button>
                        </div>
                    </div>
                    <div id="quiz-result" class="hidden text-center">
                        <p id="quiz-message" class="text-xl font-bold text-gray-800 mb-4"></p>
                        <p id="quiz-score" class="text-lg text-gray-700"></p>
                    </div>
                </div>

                <!-- Matching Game Section -->
                <div class="bg-white p-6 sm:p-8 rounded-xl shadow-lg border border-gray-200">
                    <h3 class="text-xl font-bold text-blue-600 mb-4 text-center">Flashcard Matching Game</h3>
                    <div id="matching-intro">
                        <p class="mb-4 text-gray-700 text-center">In this memory game, click on a card to reveal the term. Find the matching English and Chinese pairs to win! How fast can you do it?</p>
                        <form id="matching-name-form" class="flex flex-col items-center">
                            <label for="matching-name" class="block mb-2 font-semibold">Please enter your name:</label>
                            <input type="text" id="matching-name" class="w-full max-w-xs px-4 py-2 mb-4 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Your Name" required>
                            <button type="submit" class="py-2 px-6 bg-blue-600 text-white rounded-lg font-semibold hover:bg-blue-700 transition-colors">Start Game</button>
                        </form>
                    </div>
                    <div id="matching-game" class="hidden">
                        <div class="flex justify-between items-center mb-4">
                            <p class="font-semibold text-gray-600">Time: <span id="timer" class="text-blue-600">0s</span></p>
                        </div>
                        <div id="matching-grid" class="grid grid-cols-4 gap-4 justify-items-center"></div>
                    </div>
                    <div id="matching-result" class="hidden text-center">
                        <p id="matching-message" class="text-xl font-bold text-gray-800 mb-4"></p>
                        <p id="matching-time" class="text-lg text-gray-700"></p>
                    </div>
                </div>
                
                <!-- Fill-in-the-Blanks Game Section -->
                <div class="bg-white p-6 sm:p-8 rounded-xl shadow-lg border border-gray-200 col-span-1 md:col-span-2">
                    <h3 class="text-xl font-bold text-blue-600 mb-4 text-center">Fill-in-the-Blanks</h3>
                    <div id="blanks-game-area" class="space-y-6">
                        <div class="bg-gray-50 p-6 rounded-xl border border-gray-200">
                            <p id="blanks-question-text" class="text-lg md:text-xl font-semibold text-gray-700 mb-4 text-center"></p>
                            <div class="flex flex-col md:flex-row items-center justify-center gap-4">
                                <input type="text" id="blanks-user-answer" placeholder="Type your answer here..."
                                    class="w-full md:flex-1 p-3 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-sky-500 text-center text-lg">
                                <button id="blanks-submit-button"
                                    class="w-full md:w-auto px-6 py-3 bg-sky-600 text-white font-bold rounded-lg shadow-md hover:bg-sky-700 transition-colors transform hover:scale-105">
                                    Submit
                                </button>
                            </div>
                        </div>

                        <div id="blanks-feedback-area" class="min-h-[100px]">
                            <!-- Feedback will be displayed here -->
                        </div>
                        
                        <div class="flex justify-center">
                            <button id="blanks-next-button"
                                class="hidden px-6 py-3 bg-gray-300 text-gray-700 font-bold rounded-lg shadow-md hover:bg-gray-400 transition-colors transform hover:scale-105">
                                Next Question
                            </button>
                        </div>
                    </div>
                </div>

            </div>
        </section>

    </main>
    
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const airData = {
                labels: ['Nitrogen (78%)', 'Oxygen (21%)', 'Argon & Other (0.96%)', 'Carbon Dioxide (0.04%)'],
                datasets: [{
                    label: 'Composition of Air',
                    data: [78, 21, 0.96, 0.04],
                    backgroundColor: [
                        '#818cf8',
                        '#60a5fa',
                        '#fbbf24',
                        '#a78bfa'
                    ],
                    borderColor: '#fdfaf6',
                    borderWidth: 4,
                    hoverOffset: 10
                }]
            };

            const config = {
                type: 'doughnut',
                data: airData,
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: {
                                padding: 20,
                                font: {
                                    size: 14,
                                    family: "'Inter', sans-serif"
                                }
                            }
                        }
                    },
                    cutout: '60%'
                }
            };
            
            const airChartCanvas = document.getElementById('airCompositionChart');
            if (airChartCanvas) {
                new Chart(airChartCanvas, config);
            }

            const gasTabs = document.querySelectorAll('.gas-tab');
            const gasInfos = document.querySelectorAll('.gas-info');
            gasTabs.forEach(tab => {
                tab.addEventListener('click', () => {
                    const targetGas = tab.dataset.gas;

                    gasTabs.forEach(t => {
                        t.classList.remove('tab-active');
                        t.classList.add('tab-inactive');
                    });
                    tab.classList.add('tab-active');
                    tab.classList.remove('tab-inactive');

                    gasInfos.forEach(info => {
                        if (info.id === `${targetGas}-info`) {
                            info.classList.remove('hidden');
                        } else {
                            info.classList.add('hidden');
                        }
                    });
                });
            });

            const flashcardData = [
                { term: 'Atmosphere', chinese: '大氣' },
                { term: 'Mixture', chinese: '混合物' },
                { term: 'Nitrogen', chinese: '氮' },
                { term: 'Oxygen', chinese: '氧' },
                { term: 'Carbon dioxide', chinese: '二氧化碳' },
                { term: 'Noble gas', chinese: '貴氣體' },
                { term: 'Argon', chinese: '氬' },
                { term: 'Neon', chinese: '氖' },
                { term: 'Helium', chinese: '氦' },
                { term: 'Identify', chinese: '辨認' },
                { term: 'Burning splint', chinese: '燃燒中的木條' },
                { term: 'Glowing splint', chinese: '有餘燼的木條' },
                { term: 'Hydrogencarbonate indicator', chinese: '碳酸氫鹽指示劑' },
                { term: 'Lime water', chinese: '石灰水' },
                { term: 'Cobalt chloride paper', chinese: '氯化鈷試紙' },
                { term: 'Alcohol', chinese: '酒精' },
                { term: 'Desiccator', chinese: '乾燥器' },
                { term: 'Collaboration', chinese: '合作' },
                { term: 'Odourless', chinese: '無氣味的' },
                { term: 'Welding torch', chinese: '焊槍' },
                { term: 'Propel', chinese: '推動' },
                { term: 'Fire extinguisher', chinese: '滅火筒' },
                { term: 'Displace', chinese: '取代' },
                { term: 'Cord blood', chinese: '臍帶血' },
            ];
            
            let currentCardIndex = 0;
            const flashcard = document.getElementById('flashcard');
            const frontText = document.querySelector('#flashcard-front p');
            const backText = document.querySelector('#flashcard-back p');
            const prevBtn = document.getElementById('prev-btn');
            const nextBtn = document.getElementById('next-btn');
            const shuffleBtn = document.getElementById('shuffle-btn');
            const audioBtn = document.getElementById('audio-btn');
            const cardCounter = document.getElementById('card-counter');
            
            // Helper function to convert base64 to ArrayBuffer
            function base64ToArrayBuffer(base64) {
                const binary_string = window.atob(base64);
                const len = binary_string.length;
                const bytes = new Uint8Array(len);
                for (let i = 0; i < len; i++) {
                    bytes[i] = binary_string.charCodeAt(i);
                }
                return bytes.buffer;
            }

            // Helper function to convert PCM audio to WAV format
            function pcmToWav(pcmData, sampleRate) {
                const dataLength = pcmData.length * 2;
                const buffer = new ArrayBuffer(44 + dataLength);
                const view = new DataView(buffer);

                let offset = 0;
                function writeString(str) {
                    for (let i = 0; i < str.length; i++) {
                        view.setUint8(offset + i, str.charCodeAt(i));
                    }
                    offset += str.length;
                }

                function writeUint32(val) {
                    view.setUint32(offset, val, true);
                    offset += 4;
                }

                function writeUint16(val) {
                    view.setUint16(offset, val, true);
                    offset += 2;
                }

                // RIFF chunk descriptor
                writeString('RIFF');
                writeUint32(36 + dataLength);
                writeString('WAVE');

                // fmt sub-chunk
                writeString('fmt ');
                writeUint32(16);
                writeUint16(1); // Audio format (1 = PCM)
                writeUint16(1); // Number of channels (1)
                writeUint32(sampleRate);
                writeUint32(sampleRate * 2); // Byte rate
                writeUint16(2); // Block align
                writeUint16(16); // Bits per sample

                // data sub-chunk
                writeString('data');
                writeUint32(dataLength);

                // Write PCM data
                for (let i = 0; i < pcmData.length; i++) {
                    view.setInt16(offset, pcmData[i], true);
                    offset += 2;
                }

                return new Blob([view], { type: 'audio/wav' });
            }

            async function speakFlashcardTerm(term) {
                audioBtn.disabled = true;
                audioBtn.innerHTML = `<svg class="animate-spin h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path></svg><span>Loading...</span>`;
                
                const payload = {
                    contents: [{
                        parts: [{ text: term }]
                    }],
                    generationConfig: {
                        responseModalities: ["AUDIO"],
                        speechConfig: {
                            voiceConfig: {
                                prebuiltVoiceConfig: { voiceName: "Zephyr" }
                            }
                        }
                    },
                    model: "gemini-2.5-flash-preview-tts"
                };
                const apiKey = ""; 
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-tts:generateContent?key=${apiKey}`;

                try {
                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });
                    
                    const result = await response.json();
                    const part = result?.candidates?.[0]?.content?.parts?.[0];
                    const audioData = part?.inlineData?.data;
                    const mimeType = part?.inlineData?.mimeType;

                    if (audioData && mimeType && mimeType.startsWith("audio/")) {
                        const sampleRate = parseInt(mimeType.match(/rate=(\d+)/)[1], 10);
                        const pcmData = base64ToArrayBuffer(audioData);
                        const pcm16 = new Int16Array(pcmData);
                        const wavBlob = pcmToWav(pcm16, sampleRate);
                        const audioUrl = URL.createObjectURL(wavBlob);
                        const audio = new Audio(audioUrl);
                        audio.play();
                    }
                } catch (error) {
                    console.error("Error speaking word:", error);
                } finally {
                    audioBtn.disabled = false;
                    audioBtn.innerHTML = `<svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.293-4.293A1 1 0 0112 5v14a1 1 0 01-1.707.707L5.586 15z"></path></svg><span>Listen</span>`;
                }
            }

            function showCard() {
                const card = flashcardData[currentCardIndex];
                frontText.textContent = card.term;
                backText.textContent = card.chinese;
                flashcard.classList.remove('flipped');
                cardCounter.textContent = `${currentCardIndex + 1} / ${flashcardData.length}`;
            }

            function flipCard() {
                flashcard.classList.toggle('flipped');
            }

            function nextCard() {
                currentCardIndex = (currentCardIndex + 1) % flashcardData.length;
                showCard();
            }

            function prevCard() {
                currentCardIndex = (currentCardIndex - 1 + flashcardData.length) % flashcardData.length;
                showCard();
            }
            
            function shuffleCards() {
                for (let i = flashcardData.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [flashcardData[i], flashcardData[j]] = [flashcardData[j], flashcardData[i]];
                }
                currentCardIndex = 0;
                showCard();
            }
            
            if(flashcard){
                flashcard.addEventListener('click', flipCard);
                prevBtn.addEventListener('click', prevCard);
                nextBtn.addEventListener('click', nextCard);
                shuffleBtn.addEventListener('click', shuffleCards);
                audioBtn.addEventListener('click', () => {
                    const currentTerm = flashcardData[currentCardIndex].term;
                    speakFlashcardTerm(currentTerm);
                });
                showCard();
            }

            const quizNameForm = document.getElementById('quiz-name-form');
            const quizIntro = document.getElementById('quiz-intro');
            const quizGame = document.getElementById('quiz-game');
            const quizQuestionEl = document.getElementById('quiz-question');
            const quizOptionsEl = document.getElementById('quiz-options');
            const quizNextBtn = document.getElementById('quiz-next-btn');
            const quizResult = document.getElementById('quiz-result');
            const quizMessageEl = document.getElementById('quiz-message');
            const quizScoreEl = document.getElementById('quiz-score');
            const quizFeedbackEl = document.getElementById('quiz-feedback');

            let quizQuestions = [
                {
                    question: "What gas makes up approximately 78% of the air?",
                    options: ["Oxygen", "Carbon dioxide", "Nitrogen", "Argon"],
                    answer: "Nitrogen",
                    explanation: "Nitrogen is the most abundant gas in the atmosphere, making up about 78% of the air."
                },
                {
                    question: "Which gas is used in a fire extinguisher because it does not support burning?",
                    options: ["Oxygen", "Nitrogen", "Noble gas", "Carbon dioxide"],
                    answer: "Carbon dioxide",
                    explanation: "Carbon dioxide is denser than air and does not support burning, allowing it to cut off oxygen from a fire."
                },
                {
                    question: "What happens when you place a glowing splint in a test tube of oxygen?",
                    options: ["It goes out", "It relights", "It pops", "It turns pink"],
                    answer: "It relights",
                    explanation: "The presence of oxygen will cause a glowing splint to relight, which is a key test for this gas."
                },
                {
                    question: "Lime water turns milky/cloudy in the presence of which gas?",
                    options: ["Oxygen", "Carbon dioxide", "Nitrogen", "Helium"],
                    answer: "Carbon dioxide",
                    explanation: "Bubbling carbon dioxide through lime water causes it to turn milky, a classic chemical test."
                },
                {
                    question: "Which property makes Nitrogen useful for preserving food?",
                    options: ["It's heavy", "It's unreactive", "It's colourful", "It supports burning"],
                    answer: "It's unreactive",
                    explanation: "Nitrogen is unreactive and prevents food from decaying quickly by stopping chemical reactions with oxygen."
                },
            ];

            let quizScore = 0;
            let currentQuizQuestion = 0;
            let userName = '';

            quizNameForm.addEventListener('submit', (e) => {
                e.preventDefault();
                userName = document.getElementById('quiz-name').value;
                quizIntro.classList.add('hidden');
                quizGame.classList.remove('hidden');
                showQuizQuestion();
            });

            function showQuizQuestion() {
                const q = quizQuestions[currentQuizQuestion];
                quizQuestionEl.textContent = q.question;
                quizOptionsEl.innerHTML = '';
                quizFeedbackEl.innerHTML = '';
                quizNextBtn.disabled = true;

                q.options.forEach(option => {
                    const button = document.createElement('button');
                    button.textContent = option;
                    button.className = 'w-full py-2 px-4 rounded-lg text-left font-medium bg-gray-100 hover:bg-gray-200 transition-colors';
                    button.dataset.option = option;
                    button.addEventListener('click', selectQuizOption);
                    quizOptionsEl.appendChild(button);
                });
            }

            function selectQuizOption(e) {
                const selectedOption = e.target.dataset.option;
                const correctOption = quizQuestions[currentQuizQuestion].answer;
                const explanation = quizQuestions[currentQuizQuestion].explanation;

                document.querySelectorAll('#quiz-options button').forEach(button => {
                    button.disabled = true;
                    if (button.dataset.option === correctOption) {
                        button.classList.add('bg-green-500', 'text-white');
                    } else if (button.dataset.option === selectedOption) {
                        button.classList.add('bg-red-500', 'text-white');
                    }
                });

                if (selectedOption === correctOption) {
                    quizScore++;
                    quizFeedbackEl.innerHTML = `<p class="text-green-600">Correct! Well done. ${explanation}</p>`;
                } else {
                    quizFeedbackEl.innerHTML = `<p class="text-red-600">Incorrect. The correct answer is: <strong>${correctOption}</strong>. ${explanation}</p>`;
                }

                quizNextBtn.disabled = false;
            }

            quizNextBtn.addEventListener('click', () => {
                currentQuizQuestion++;
                if (currentQuizQuestion < quizQuestions.length) {
                    showQuizQuestion();
                } else {
                    quizGame.classList.add('hidden');
                    quizResult.classList.remove('hidden');
                    quizMessageEl.textContent = `Hello, ${userName}! You've finished the quiz.`;
                    quizScoreEl.textContent = `Your score is ${quizScore} out of ${quizQuestions.length}. Great effort!`;
                }
            });

            const matchingNameForm = document.getElementById('matching-name-form');
            const matchingIntro = document.getElementById('matching-intro');
            const matchingGame = document.getElementById('matching-game');
            const matchingGrid = document.getElementById('matching-grid');
            const matchingResult = document.getElementById('matching-result');
            const matchingMessageEl = document.getElementById('matching-message');
            const matchingTimeEl = document.getElementById('matching-time');
            const timerEl = document.getElementById('timer');

            let matchingCards = [];
            let flippedCards = [];
            let matchedPairs = 0;
            let startTime = 0;
            let timerInterval;

            matchingNameForm.addEventListener('submit', (e) => {
                e.preventDefault();
                userName = document.getElementById('matching-name').value;
                matchingIntro.classList.add('hidden');
                matchingGame.classList.remove('hidden');
                initMatchingGame();
            });

            function shuffle(array) {
                for (let i = array.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [array[i], array[j]] = [array[j], array[i]];
                }
                return array;
            }

            function initMatchingGame() {
                matchingCards = [];
                const termsToUse = flashcardData.slice(0, 10);
                const pairs = [];
                termsToUse.forEach((item, index) => {
                    pairs.push({ text: item.term, pairId: index });
                    pairs.push({ text: item.chinese, pairId: index });
                });

                shuffle(pairs);
                
                matchingGrid.innerHTML = '';
                pairs.forEach((pair, index) => {
                    const cardElement = document.createElement('div');
                    cardElement.className = 'matching-card';
                    cardElement.dataset.pairId = pair.pairId;
                    cardElement.textContent = pair.text;
                    cardElement.addEventListener('click', flipMatchingCard);
                    matchingCards.push(cardElement);
                    matchingGrid.appendChild(cardElement);
                });

                matchedPairs = 0;
                flippedCards = [];
                timerEl.textContent = '0s';
                startTime = 0;
                clearInterval(timerInterval);
            }

            function flipMatchingCard(e) {
                const clickedCard = e.currentTarget;
                if (flippedCards.length < 2 && !clickedCard.classList.contains('is-revealed')) {
                    if (flippedCards.length === 0 && startTime === 0) {
                        startTime = Date.now();
                        timerInterval = setInterval(() => {
                            const elapsed = Math.floor((Date.now() - startTime) / 1000);
                            timerEl.textContent = `${elapsed}s`;
                        }, 1000);
                    }
                    clickedCard.classList.add('is-revealed');
                    flippedCards.push(clickedCard);

                    if (flippedCards.length === 2) {
                        const card1 = flippedCards[0];
                        const card2 = flippedCards[1];
                        
                        const pairId1 = card1.dataset.pairId;
                        const pairId2 = card2.dataset.pairId;
                        
                        if (pairId1 === pairId2 && card1 !== card2) {
                            setTimeout(() => {
                                card1.classList.add('matched');
                                card2.classList.add('matched');
                                matchedPairs++;
                                flippedCards = [];
                                if (matchedPairs === matchingCards.length / 2) {
                                    clearInterval(timerInterval);
                                    matchingGame.classList.add('hidden');
                                    matchingResult.classList.remove('hidden');
                                    const finalTime = Math.floor((Date.now() - startTime) / 1000);
                                    matchingMessageEl.textContent = `Congratulations, ${userName}!`;
                                    matchingTimeEl.textContent = `You finished the game in ${finalTime} seconds. What a great job!`;
                                }
                            }, 1000);
                        } else {
                            setTimeout(() => {
                                card1.classList.remove('is-revealed');
                                card2.classList.remove('is-revealed');
                                flippedCards = [];
                            }, 1000);
                        }
                    }
                }
            }
            
            // Fill-in-the-Blanks game logic
            const blanksQuestions = [
                {
                    sentence: "The Earth is surrounded by a layer of air called the __BLANK__.",
                    answer: "atmosphere",
                    explanation: "The atmosphere is the layer of gases that surrounds the planet Earth. It's essential for life."
                },
                {
                    sentence: "Air is a __BLANK__ of gases, mainly nitrogen and oxygen.",
                    answer: "mixture",
                    explanation: "A mixture is formed when two or more substances are combined without a new substance being created."
                },
                {
                    sentence: "Air contains about 78% of __BLANK__, which is the most abundant gas.",
                    answer: "nitrogen",
                    explanation: "Nitrogen ($N_2$) is a colorless and odorless gas that makes up the majority of the Earth's atmosphere."
                },
                {
                    sentence: "__BLANK__ is a gas that is essential for the survival of humans.",
                    answer: "oxygen",
                    explanation: "Oxygen ($O_2$) is a highly reactive gas that is crucial for respiration in most living organisms."
                },
                {
                    sentence: "Oxygen and nitrogen are both colourless and __BLANK__ gases.",
                    answer: "odourless",
                    explanation: "Odourless means having no smell."
                },
                {
                    sentence: "A __BLANK__ torch is used to cut and weld metals by mixing oxygen with a fuel gas.",
                    answer: "welding",
                    explanation: "Welding is the process of joining materials, usually metals or thermoplastics, by causing fusion."
                },
                {
                    sentence: "Oxygen is essential for the __BLANK__ of humans and other living things.",
                    answer: "survival",
                    explanation: "Survival refers to the state or fact of continuing to live or exist, especially in spite of difficult conditions."
                },
                {
                    sentence: "Carbon dioxide sinks and __BLANK__ the air surrounding a fire, cutting off the oxygen supply.",
                    answer: "displaces",
                    explanation: "Displaces means to take over the place, position, or role of someone or something."
                }
            ];

            let currentBlanksQuestionIndex = 0;

            const blanksQuestionText = document.getElementById('blanks-question-text');
            const blanksUserAnswerInput = document.getElementById('blanks-user-answer');
            const blanksSubmitButton = document.getElementById('blanks-submit-button');
            const blanksNextButton = document.getElementById('blanks-next-button');
            const blanksFeedbackArea = document.getElementById('blanks-feedback-area');

            function loadBlanksQuestion() {
                if (currentBlanksQuestionIndex < blanksQuestions.length) {
                    const currentQuestion = blanksQuestions[currentBlanksQuestionIndex];
                    blanksQuestionText.textContent = currentQuestion.sentence.replace('__BLANK__', '______');
                    blanksUserAnswerInput.value = '';
                    blanksFeedbackArea.innerHTML = '';
                    blanksSubmitButton.style.display = 'block';
                    blanksNextButton.style.display = 'none';
                    blanksUserAnswerInput.focus();
                } else {
                    blanksQuestionText.textContent = "You've completed the game! Great job!";
                    blanksUserAnswerInput.style.display = 'none';
                    blanksSubmitButton.style.display = 'none';
                    blanksNextButton.style.display = 'none';
                }
            }

            function checkBlanksAnswer() {
                const currentQuestion = blanksQuestions[currentBlanksQuestionIndex];
                const userAnswer = blanksUserAnswerInput.value.trim().toLowerCase();
                const correctAnswer = currentQuestion.answer.toLowerCase();

                if (userAnswer === correctAnswer) {
                    showBlanksFeedback(true, `Correct! The answer is ${currentQuestion.answer}.`);
                    blanksSubmitButton.style.display = 'none';
                    blanksNextButton.style.display = 'block';
                } else {
                    showBlanksFeedback(false, `Oops! That's not quite right. The correct answer is <span class="text-sky-600 font-bold">${correctAnswer}</span>.`);
                    showBlanksExplanation(currentQuestion.explanation);
                    blanksSubmitButton.style.display = 'none';
                    blanksNextButton.style.display = 'block';
                }
            }

            function showBlanksFeedback(isCorrect, message) {
                const feedbackClass = isCorrect ? 'text-green-600' : 'text-red-600';
                const feedbackDiv = document.createElement('div');
                feedbackDiv.className = `p-4 rounded-xl shadow-inner mb-4 transition-all duration-300 ${isCorrect ? 'bg-green-100' : 'bg-red-100'} text-center`;
                feedbackDiv.innerHTML = `<p class="font-semibold text-lg ${feedbackClass}">${message}</p>`;
                blanksFeedbackArea.appendChild(feedbackDiv);
            }

            function showBlanksExplanation(explanation) {
                const explanationDiv = document.createElement('div');
                explanationDiv.className = 'p-4 rounded-xl bg-blue-100 border border-blue-200 mt-2';
                explanationDiv.innerHTML = `<p class="font-medium text-gray-700">${explanation}</p>`;
                blanksFeedbackArea.appendChild(explanationDiv);
            }

            blanksSubmitButton.addEventListener('click', checkBlanksAnswer);

            blanksNextButton.addEventListener('click', () => {
                currentBlanksQuestionIndex++;
                loadBlanksQuestion();
            });

            blanksUserAnswerInput.addEventListener('keypress', (event) => {
                if (event.key === 'Enter') {
                    if (blanksSubmitButton.style.display !== 'none') {
                        checkBlanksAnswer();
                    } else if (blanksNextButton.style.display !== 'none') {
                        blanksNextButton.click();
                    }
                }
            });

            // Initial load for the blanks game
            loadBlanksQuestion();
        });
    </script>
</body>
</html>
