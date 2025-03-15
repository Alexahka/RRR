<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Иннесса - Генератор комплиментов</title>
    <link href="https://unpkg.com/tailwindcss@^2/dist/tailwind.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .heart {
            animation: float 7s infinite linear;
            opacity: 0.5;
        }
        
        @keyframes float {
            0%, 100% { transform: translateY(-40px) rotate(0deg); }
            50% { transform: translateY(40px) rotate(180deg); }
        }

        .name-letter {
            animation: glow 1s infinite alternate;
        }

        @keyframes glow {
            from { opacity: 0.6; }
            to { opacity: 1; }
        }

        .compliment-window {
            animation: slideIn 0.5s ease-out, fadeOut 0.5s ease-in 4s;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 500px;
            height: 300px;
            background: linear-gradient(135deg, rgba(75, 0, 130, 0.95), rgba(147, 112, 219, 0.95));
            backdrop-filter: blur(10px);
            border-radius: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            border: 2px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 0 30px rgba(147, 112, 219, 0.7);
            overflow: hidden;
        }

        @keyframes slideIn {
            from { transform: translate(-50%, -50%) scale(0); opacity: 0; }
            to { transform: translate(-50%, -50%) scale(1); opacity: 1; }
        }

        @keyframes fadeOut {
            from { opacity: 1; }
            to { opacity: 0; }
        }

        .emoji {
            animation: floatEmoji 3s infinite ease-in-out;
        }

        @keyframes floatEmoji {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }

        .heart-particle {
            position: absolute;
            color: rgba(255, 255, 255, 0.7);
            animation: floatParticle 5s infinite ease-in-out;
        }

        @keyframes floatParticle {
            0%, 100% { transform: translateY(0) rotate(0deg); }
            50% { transform: translateY(-20px) rotate(180deg); }
        }

        .compliment-text {
            font-family: 'Dancing Script', cursive;
            font-size: 2rem;
            color: white;
            text-align: center;
            animation: textAppear 1s ease-in-out;
            text-transform: uppercase;
        }

        @keyframes textAppear {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&display=swap" rel="stylesheet">
</head>
<body class="min-h-screen relative overflow-hidden bg-pink-400">
    <div id="hearts-container" class="absolute inset-0 pointer-events-none"></div>
    
    <div class="container mx-auto px-4 py-16 text-center">
        <div class="relative mb-12">
            <h1 class="text-6xl font-bold text-white mb-8">
                <span class="name-letter">И</span>
                <span class="name-letter">Н</span>
                <span class="name-letter">Н</span>
                <span class="name-letter">Е</span>
                <span class="name-letter">С</span>
                <span class="name-letter">С</span>
                <span class="name-letter">А</span>
                <i class="fas fa-crown absolute -top-8 left-1/2 transform -translate-x-1/2 text-yellow-300 text-4xl"></i>
            </h1>
        </div>

        <div class="flex flex-col items-center space-y-4">
            <button onclick="generateCompliment()" 
                    class="bg-gradient-to-br from-pink-500 to-rose-600 text-white py-6 px-8 rounded-2xl text-lg font-bold shadow-lg hover:scale-105 transition-transform w-64">
                ✨ Получить Энергию
            </button>
            
            <button onclick="changeBackground()" 
                    class="bg-gradient-to-br from-cyan-400 to-blue-600 text-white py-6 px-8 rounded-2xl text-lg font-bold shadow-lg hover:scale-105 transition-transform w-64">
                🌈 Сменить Ауру
            </button>
            
            <button onclick="clearCompliments()" 
                    class="bg-gradient-to-br from-emerald-400 to-green-600 text-white py-6 px-8 rounded-2xl text-lg font-bold shadow-lg hover:scale-105 transition-transform w-64">
                🍃 Очистить Пространство
            </button>
        </div>
    </div>

    <script>
        const compliments = [
            "Ты можешь все, что захочешь!", "Ты создаешь магию вокруг себя!", "Ты воплощение вдохновения!",
            "Ты яркая, как вспышка света!", "Ты двигаешься вперед с уверенностью!", "Ты воплощение силы и нежности!",
            "Ты можешь свернуть горы!", "Ты превращаешь мечты в реальность!", "Ты наполняешь жизнь смыслом!",
            "Ты сияешь даже в темноте!", "Ты достойна всего самого лучшего!", "Ты настоящая героиня своей жизни!",
            "Ты несешь в себе радость и позитив!", "Ты творишь чудеса вокруг!", "Ты вдохновляешь людей становиться лучше!",
            "Ты делаешь мир красивее просто тем, что существуешь!", "Ты создаешь атмосферу уюта и счастья!",
            "Ты сильнее, чем думаешь!", "Ты искренне прекрасна!", "Ты всегда находишь выход!",
            "Ты символ добра и любви!", "Ты уверенно идешь к своим целям!", "Ты обладаешь неотразимой харизмой!",
            "Ты лучший друг и советчик!", "Ты потрясающе умна!", "Ты излучаешь свет и тепло!",
            "Ты на шаг впереди всех!", "Ты вдохновляешь окружающих на великие дела!", "Ты делаешь этот мир лучше!",
            "Ты как магия, меняешь все к лучшему!", "Ты всегда готова помочь!", "Ты сильная и решительная!",
            "Ты приносишь радость людям!", "Ты невероятно талантлива!", "Ты заслуживаешь самых больших побед!",
            "Ты делаешь невозможное возможным!", "Ты настоящая муза!", "Ты совмещаешь в себе все лучшие качества!",
            "Ты бесконечно обаятельна!", "Ты создаешь вокруг себя счастье!", "Ты уникальна и неповторима!",
            "Ты настоящий источник вдохновения!", "Ты пробуждаешь лучшие чувства в людях!", "Ты движешься к успеху!",
            "Ты вызываешь восхищение!", "Ты озаряешь этот мир!", "Ты достойна самой красивой жизни!",
            "Ты можешь все, потому что ты — Инна!"
        ];

        const emojis = ["😊", "🌟", "💖", "✨", "🌸", "🎀", "💫", "💎", "🌺", "🦋", "🌈", "💐"];

        const colors = [
            "bg-red-400", "bg-blue-400", "bg-green-400", 
            "bg-yellow-400", "bg-purple-400", "bg-pink-400", "bg-orange-400"
        ];

        let currentColorIndex = 0;

        function createHearts() {
            const container = document.getElementById('hearts-container');
            for (let i = 0; i < 30; i++) {
                const heart = document.createElement('i');
                heart.className = 'fas fa-heart heart text-red-400 absolute';
                heart.style.left = `${Math.random() * 100}%`;
                heart.style.top = `${Math.random() * 100}%`;
                heart.style.animationDelay = `${i * 0.4}s`;
                container.appendChild(heart);
            }
        }

        function generateCompliment() {
            const compliment = compliments[Math.floor(Math.random() * compliments.length)];
            const emoji = emojis[Math.floor(Math.random() * emojis.length)];

            const complimentWindow = document.createElement('div');
            complimentWindow.className = 'compliment-window';
            complimentWindow.innerHTML = `
                <span class="emoji text-4xl">${emoji}</span>
                <p class="compliment-text">${compliment}</p>
            `;

            // Add heart particles
            for (let i = 0; i < 10; i++) {
                const heartParticle = document.createElement('i');
                heartParticle.className = 'fas fa-heart heart-particle';
                heartParticle.style.left = `${Math.random() * 100}%`;
                heartParticle.style.top = `${Math.random() * 100}%`;
                heartParticle.style.fontSize = `${Math.random() * 20 + 10}px`;
                heartParticle.style.animationDelay = `${Math.random() * 2}s`;
                heartParticle.style.color = `rgba(255, ${Math.random() * 105 + 150}, ${Math.random() * 105 + 150}, 0.7)`;
                complimentWindow.appendChild(heartParticle);
            }

            document.body.appendChild(complimentWindow);

            setTimeout(() => {
                complimentWindow.remove();
            }, 4000);
        }

        function changeBackground() {
            currentColorIndex = (currentColorIndex + 1) % colors.length;
            document.body.className = `min-h-screen relative overflow-hidden ${colors[currentColorIndex]}`;
        }

        function clearCompliments() {
            const windows = document.querySelectorAll('.compliment-window');
            windows.forEach(window => window.remove());
        }

        // Initialize
        document.body.className = `min-h-screen relative overflow-hidden ${colors[0]}`;
        createHearts();
    </script>
</body>
</html>
