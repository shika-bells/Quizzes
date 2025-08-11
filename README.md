<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Biology Quiz 🧬</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @keyframes float {
            0%, 100% { transform: translateY(0px) rotate(0deg); }
            50% { transform: translateY(-20px) rotate(180deg); }
        }
        
        @keyframes drift {
            0% { transform: translateX(-100px); }
            100% { transform: translateX(calc(100vw + 100px)); }
        }
        
        @keyframes celebrate {
            0% { transform: translateY(0) rotate(0deg); opacity: 1; }
            100% { transform: translateY(-200px) rotate(360deg); opacity: 0; }
        }
        
        .floating-cell {
            animation: float 4s ease-in-out infinite;
        }
        
        .drifting-cell {
            animation: drift 15s linear infinite;
        }
        
        .celebrating-leaf {
            animation: celebrate 3s ease-out forwards;
        }
        
        .progress-plant {
            font-size: 3rem;
            transition: all 0.5s ease;
        }
        
        .badge-glow {
            box-shadow: 0 0 30px rgba(34, 197, 94, 0.5);
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.7; }
        }
    </style>
</head>
<body class="bg-gradient-to-br from-green-50 to-blue-50 min-h-screen relative overflow-hidden">
    <!-- Floating Background Elements -->
    <div id="background-cells" class="fixed inset-0 pointer-events-none z-0">
        <!-- Cells will be dynamically added here -->
    </div>
    
    <div class="relative z-10 container mx-auto px-4 py-8">
        <!-- Header -->
        <div class="text-center mb-8">
            <h1 class="text-4xl font-bold text-green-800 mb-2">🧬 Biology Quiz Adventure 🌱</h1>
            <p class="text-green-600 text-lg">Test your knowledge and grow your biology skills!</p>
        </div>
        
        <!-- Progress Section -->
        <div class="bg-white/80 backdrop-blur-sm rounded-2xl p-6 mb-8 shadow-lg border border-green-200">
            <div class="flex items-center justify-between mb-4">
                <div class="text-center">
                    <div class="progress-plant" id="progress-plant">🌱</div>
                    <p class="text-sm text-green-600 font-medium">Your Progress</p>
                </div>
                <div class="text-right">
                    <div class="text-2xl font-bold text-green-800" id="score">0/0</div>
                    <p class="text-sm text-green-600">Correct Answers</p>
                </div>
            </div>
            <div class="w-full bg-green-100 rounded-full h-3">
                <div class="bg-gradient-to-r from-green-400 to-green-600 h-3 rounded-full transition-all duration-500" id="progress-bar" style="width: 0%"></div>
            </div>
        </div>
        
        <!-- Difficulty Selection -->
        <div id="difficulty-screen" class="bg-white/80 backdrop-blur-sm rounded-2xl p-8 shadow-lg border border-green-200">
            <h2 class="text-2xl font-bold text-green-800 mb-6 text-center">Choose Your Challenge Level 🎯</h2>
            <div class="grid md:grid-cols-3 gap-6">
                <button onclick="startQuiz('easy')" class="bg-gradient-to-br from-green-300 to-green-400 hover:from-green-400 hover:to-green-500 p-6 rounded-xl text-white font-bold text-lg transition-all duration-300 transform hover:scale-105">
                    <div class="text-3xl mb-2">🌱</div>
                    <div>Seedling</div>
                    <div class="text-sm opacity-90">Basic Concepts</div>
                </button>
                <button onclick="startQuiz('medium')" class="bg-gradient-to-br from-green-500 to-green-600 hover:from-green-600 hover:to-green-700 p-6 rounded-xl text-white font-bold text-lg transition-all duration-300 transform hover:scale-105">
                    <div class="text-3xl mb-2">🌿</div>
                    <div>Growing Plant</div>
                    <div class="text-sm opacity-90">Intermediate</div>
                </button>
                <button onclick="startQuiz('hard')" class="bg-gradient-to-br from-green-700 to-green-800 hover:from-green-800 hover:to-green-900 p-6 rounded-xl text-white font-bold text-lg transition-all duration-300 transform hover:scale-105">
                    <div class="text-3xl mb-2">🌳</div>
                    <div>Mighty Tree</div>
                    <div class="text-sm opacity-90">Advanced</div>
                </button>
            </div>
        </div>
        
        <!-- Quiz Screen -->
        <div id="quiz-screen" class="hidden">
            <div class="bg-white/80 backdrop-blur-sm rounded-2xl p-8 shadow-lg border border-green-200">
                <div class="mb-6">
                    <div class="flex justify-between items-center mb-4">
                        <span class="text-green-600 font-medium" id="question-counter">Question 1 of 10</span>
                        <span class="text-2xl" id="question-emoji">🦠</span>
                    </div>
                    <h3 class="text-xl font-bold text-green-800 mb-6" id="question-text"></h3>
                </div>
                
                <div class="space-y-3" id="answers-container">
                    <!-- Answer buttons will be dynamically added here -->
                </div>
                
                <div class="mt-6 text-center">
                    <button id="next-button" onclick="nextQuestion()" class="hidden bg-gradient-to-r from-blue-500 to-blue-600 hover:from-blue-600 hover:to-blue-700 text-white px-8 py-3 rounded-xl font-medium transition-all duration-300">
                        Next Question →
                    </button>
                </div>
            </div>
        </div>
        
        <!-- Results Screen -->
        <div id="results-screen" class="hidden">
            <div class="bg-white/80 backdrop-blur-sm rounded-2xl p-8 shadow-lg border border-green-200 text-center">
                <div class="badge-glow bg-gradient-to-br from-green-400 to-blue-500 text-white p-8 rounded-2xl mb-6 inline-block">
                    <div class="text-4xl mb-2">🏆</div>
                    <div class="text-2xl font-bold mb-2">Junior Biologist</div>
                    <div class="text-lg">🔬🧪🧬</div>
                </div>
                
                <h2 class="text-3xl font-bold text-green-800 mb-4">Congratulations! 🎉</h2>
                <div class="text-xl text-green-600 mb-6" id="final-score"></div>
                <div class="text-lg text-green-700 mb-8" id="performance-message"></div>
                
                <button onclick="restartQuiz()" class="bg-gradient-to-r from-green-500 to-green-600 hover:from-green-600 hover:to-green-700 text-white px-8 py-3 rounded-xl font-medium transition-all duration-300">
                    Take Quiz Again 🔄
                </button>
            </div>
        </div>
    </div>

    <script>
        let currentQuiz = [];
        let currentQuestion = 0;
        let score = 0;
        let difficulty = '';
        
        const quizData = {
            easy: [
                {
                    question: "What part of the plant cell makes food using sunlight? 🌱",
                    emoji: "🌱",
                    answers: ["Chloroplast", "Nucleus", "Cell wall", "Vacuole"],
                    correct: 0
                },
                {
                    question: "Which gas do plants take in during photosynthesis? 🍃",
                    emoji: "🍃",
                    answers: ["Oxygen", "Carbon dioxide", "Nitrogen", "Hydrogen"],
                    correct: 1
                },
                {
                    question: "What do we call animals that eat only plants? 🐰",
                    emoji: "🐰",
                    answers: ["Carnivores", "Omnivores", "Herbivores", "Predators"],
                    correct: 2
                },
                {
                    question: "Which part of the cell controls what goes in and out? 🦠",
                    emoji: "🦠",
                    answers: ["Nucleus", "Cell membrane", "Cytoplasm", "Mitochondria"],
                    correct: 1
                },
                {
                    question: "What do plants release as a waste product during photosynthesis? 🌿",
                    emoji: "🌿",
                    answers: ["Carbon dioxide", "Water", "Oxygen", "Sugar"],
                    correct: 2
                },
                {
                    question: "Which animals are warm-blooded? 🐦",
                    emoji: "🐦",
                    answers: ["Fish and reptiles", "Birds and mammals", "Insects and spiders", "Amphibians and fish"],
                    correct: 1
                },
                {
                    question: "What is the basic unit of life? 🧫",
                    emoji: "🧫",
                    answers: ["Tissue", "Organ", "Cell", "Organism"],
                    correct: 2
                },
                {
                    question: "Which part of the plant absorbs water from the soil? 🌱",
                    emoji: "🌱",
                    answers: ["Leaves", "Stem", "Flowers", "Roots"],
                    correct: 3
                },
                {
                    question: "What do we call animals that eat both plants and meat? 🐻",
                    emoji: "🐻",
                    answers: ["Herbivores", "Carnivores", "Omnivores", "Decomposers"],
                    correct: 2
                },
                {
                    question: "Which organelle is known as the 'powerhouse of the cell'? ⚡",
                    emoji: "⚡",
                    answers: ["Nucleus", "Chloroplast", "Vacuole", "Mitochondria"],
                    correct: 3
                }
            ],
            medium: [
                {
                    question: "During photosynthesis, what two things combine to make glucose? 🧬",
                    emoji: "🧬",
                    answers: ["Water and oxygen", "Carbon dioxide and water", "Oxygen and nitrogen", "Sugar and water"],
                    correct: 1
                },
                {
                    question: "Which kingdom includes organisms like mushrooms and yeast? 🍄",
                    emoji: "🍄",
                    answers: ["Plant kingdom", "Animal kingdom", "Fungi kingdom", "Bacteria kingdom"],
                    correct: 2
                },
                {
                    question: "What is the process called when cells divide to create new cells? 🔬",
                    emoji: "🔬",
                    answers: ["Photosynthesis", "Respiration", "Mitosis", "Digestion"],
                    correct: 2
                },
                {
                    question: "Which type of blood vessel carries blood away from the heart? ❤️",
                    emoji: "❤️",
                    answers: ["Veins", "Arteries", "Capillaries", "Ventricles"],
                    correct: 1
                },
                {
                    question: "What do we call the green pigment in plants that captures light? 🌿",
                    emoji: "🌿",
                    answers: ["Carotene", "Chlorophyll", "Melanin", "Hemoglobin"],
                    correct: 1
                },
                {
                    question: "Which organ system is responsible for transporting nutrients? 🩸",
                    emoji: "🩸",
                    answers: ["Nervous system", "Digestive system", "Circulatory system", "Respiratory system"],
                    correct: 2
                },
                {
                    question: "What type of organism can make its own food? 🌱",
                    emoji: "🌱",
                    answers: ["Heterotroph", "Autotroph", "Decomposer", "Parasite"],
                    correct: 1
                },
                {
                    question: "Which part of the flower contains the male reproductive organs? 🌸",
                    emoji: "🌸",
                    answers: ["Pistil", "Petals", "Stamen", "Sepals"],
                    correct: 2
                },
                {
                    question: "What is the scientific name for the windpipe? 🫁",
                    emoji: "🫁",
                    answers: ["Bronchus", "Trachea", "Larynx", "Pharynx"],
                    correct: 1
                },
                {
                    question: "Which process do plants use to transport water upward? 💧",
                    emoji: "💧",
                    answers: ["Osmosis", "Diffusion", "Transpiration", "Absorption"],
                    correct: 2
                }
            ],
            hard: [
                {
                    question: "What is the role of ribosomes in protein synthesis? 🧬",
                    emoji: "🧬",
                    answers: ["Transcribe DNA", "Translate mRNA", "Replicate DNA", "Package proteins"],
                    correct: 1
                },
                {
                    question: "Which phase of mitosis involves chromosome alignment? 🔬",
                    emoji: "🔬",
                    answers: ["Prophase", "Metaphase", "Anaphase", "Telophase"],
                    correct: 1
                },
                {
                    question: "What is the primary function of the Calvin cycle? 🌿",
                    emoji: "🌿",
                    answers: ["Produce ATP", "Fix carbon dioxide", "Split water", "Generate oxygen"],
                    correct: 1
                },
                {
                    question: "Which enzyme breaks down hydrogen peroxide in cells? 🧪",
                    emoji: "🧪",
                    answers: ["Amylase", "Catalase", "Pepsin", "Lipase"],
                    correct: 1
                },
                {
                    question: "What type of symmetry do echinoderms exhibit? ⭐",
                    emoji: "⭐",
                    answers: ["Bilateral", "Radial", "Asymmetrical", "Spherical"],
                    correct: 1
                },
                {
                    question: "Which structure regulates water balance in freshwater protists? 💧",
                    emoji: "💧",
                    answers: ["Food vacuole", "Nucleus", "Contractile vacuole", "Cytoplasm"],
                    correct: 2
                },
                {
                    question: "What is the function of the loop of Henle in the kidney? 🫘",
                    emoji: "🫘",
                    answers: ["Filter blood", "Concentrate urine", "Produce hormones", "Store urine"],
                    correct: 1
                },
                {
                    question: "Which type of RNA carries amino acids to ribosomes? 🧬",
                    emoji: "🧬",
                    answers: ["mRNA", "tRNA", "rRNA", "siRNA"],
                    correct: 1
                },
                {
                    question: "What is the primary photosynthetic pigment in red algae? 🔴",
                    emoji: "🔴",
                    answers: ["Chlorophyll a", "Chlorophyll b", "Phycoerythrin", "Carotenoids"],
                    correct: 2
                },
                {
                    question: "Which process describes the movement of water through a plant? 🌳",
                    emoji: "🌳",
                    answers: ["Translocation", "Transpiration stream", "Guttation", "Root pressure"],
                    correct: 1
                }
            ]
        };
        
        function createBackgroundCells() {
            const container = document.getElementById('background-cells');
            const cells = ['🦠', '🔬', '🧫', '🧬'];
            
            setInterval(() => {
                const cell = document.createElement('div');
                cell.className = 'fixed text-2xl opacity-20 drifting-cell';
                cell.style.top = Math.random() * window.innerHeight + 'px';
                cell.style.left = '-100px';
                cell.textContent = cells[Math.floor(Math.random() * cells.length)];
                cell.style.animationDelay = Math.random() * 2 + 's';
                
                container.appendChild(cell);
                
                setTimeout(() => {
                    if (cell.parentNode) {
                        cell.parentNode.removeChild(cell);
                    }
                }, 15000);
            }, 3000);
        }
        
        function startQuiz(selectedDifficulty) {
            difficulty = selectedDifficulty;
            currentQuiz = [...quizData[difficulty]];
            currentQuestion = 0;
            score = 0;
            
            document.getElementById('difficulty-screen').classList.add('hidden');
            document.getElementById('quiz-screen').classList.remove('hidden');
            
            updateProgress();
            showQuestion();
        }
        
        function showQuestion() {
            const question = currentQuiz[currentQuestion];
            document.getElementById('question-counter').textContent = `Question ${currentQuestion + 1} of ${currentQuiz.length}`;
            document.getElementById('question-emoji').textContent = question.emoji;
            document.getElementById('question-text').textContent = question.question;
            
            const container = document.getElementById('answers-container');
            container.innerHTML = '';
            
            question.answers.forEach((answer, index) => {
                const button = document.createElement('button');
                button.className = 'w-full p-4 text-left bg-white hover:bg-green-50 border-2 border-green-200 hover:border-green-400 rounded-xl transition-all duration-300 transform hover:scale-102';
                button.textContent = answer;
                button.onclick = () => selectAnswer(index);
                container.appendChild(button);
            });
            
            document.getElementById('next-button').classList.add('hidden');
        }
        
        function selectAnswer(selectedIndex) {
            const question = currentQuiz[currentQuestion];
            const buttons = document.querySelectorAll('#answers-container button');
            
            buttons.forEach((button, index) => {
                button.disabled = true;
                if (index === question.correct) {
                    button.classList.add('bg-green-500', 'text-white', 'border-green-500');
                } else if (index === selectedIndex && index !== question.correct) {
                    button.classList.add('bg-red-500', 'text-white', 'border-red-500');
                } else {
                    button.classList.add('opacity-50');
                }
            });
            
            if (selectedIndex === question.correct) {
                score++;
                updateProgress();
            }
            
            document.getElementById('next-button').classList.remove('hidden');
        }
        
        function nextQuestion() {
            currentQuestion++;
            if (currentQuestion < currentQuiz.length) {
                showQuestion();
            } else {
                showResults();
            }
        }
        
        function updateProgress() {
            const progressPercent = (score / currentQuiz.length) * 100;
            document.getElementById('progress-bar').style.width = progressPercent + '%';
            document.getElementById('score').textContent = `${score}/${currentQuiz.length}`;
            
            // Update plant growth
            const plant = document.getElementById('progress-plant');
            if (score <= currentQuiz.length * 0.3) {
                plant.textContent = '🌱';
            } else if (score <= currentQuiz.length * 0.7) {
                plant.textContent = '🌿';
            } else {
                plant.textContent = '🌳';
            }
        }
        
        function showResults() {
            document.getElementById('quiz-screen').classList.add('hidden');
            document.getElementById('results-screen').classList.remove('hidden');
            
            const percentage = Math.round((score / currentQuiz.length) * 100);
            document.getElementById('final-score').textContent = `You scored ${score} out of ${currentQuiz.length} (${percentage}%)`;
            
            let message = '';
            if (percentage >= 90) {
                message = "Outstanding! You're a true biology expert! 🌟";
            } else if (percentage >= 70) {
                message = "Great job! You have a solid understanding of biology! 🎉";
            } else if (percentage >= 50) {
                message = "Good effort! Keep studying to improve your biology knowledge! 📚";
            } else {
                message = "Keep learning! Biology is fascinating - try again! 💪";
            }
            
            document.getElementById('performance-message').textContent = message;
            
            // Celebrate with floating leaves
            setTimeout(() => {
                for (let i = 0; i < 20; i++) {
                    setTimeout(() => {
                        const leaf = document.createElement('div');
                        leaf.className = 'fixed text-3xl celebrating-leaf pointer-events-none z-50';
                        leaf.style.left = Math.random() * window.innerWidth + 'px';
                        leaf.style.top = window.innerHeight + 'px';
                        leaf.textContent = '🍃';
                        document.body.appendChild(leaf);
                        
                        setTimeout(() => {
                            if (leaf.parentNode) {
                                leaf.parentNode.removeChild(leaf);
                            }
                        }, 3000);
                    }, i * 200);
                }
            }, 500);
        }
        
        function restartQuiz() {
            document.getElementById('results-screen').classList.add('hidden');
            document.getElementById('difficulty-screen').classList.remove('hidden');
            
            // Reset progress
            document.getElementById('progress-plant').textContent = '🌱';
            document.getElementById('progress-bar').style.width = '0%';
            document.getElementById('score').textContent = '0/0';
        }
        
        // Initialize background animation
        createBackgroundCells();
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'96d67ec450b90a84',t:'MTc1NDkwMjMyOS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
