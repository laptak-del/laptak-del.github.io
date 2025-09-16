<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>French Vocabulary & Grammar Game</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            color: #333;
            transition: all 0.3s ease;
        }

        body.mobile-mode {
            font-size: 14px;
        }

        .game-container {
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            padding: 2rem;
            max-width: 600px;
            width: 90%;
            text-align: center;
            position: relative;
            min-height: 500px;
            transition: all 0.3s ease;
        }

        body.mobile-mode .game-container {
            padding: 1rem;
            max-width: 95%;
            min-height: 400px;
        }

        h1 {
            color: #4a5568;
            margin-bottom: 2rem;
            font-size: 2rem;
            transition: font-size 0.3s ease;
        }

        body.mobile-mode h1 {
            font-size: 1.5rem;
            margin-bottom: 1rem;
        }

        .top-controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1rem;
            flex-wrap: wrap;
            gap: 1rem;
        }

        .mobile-toggle {
            background: #805ad5;
            color: white;
            border: none;
            padding: 0.5rem 1rem;
            border-radius: 8px;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .mobile-toggle:hover {
            background: #6b46c1;
            transform: translateY(-1px);
        }

        body.mobile-mode .mobile-toggle {
            padding: 0.4rem 0.8rem;
            font-size: 0.8rem;
        }

        .mode-selector {
            display: flex;
            gap: 1rem;
            margin-bottom: 2rem;
            flex-wrap: wrap;
            justify-content: center;
            transition: all 0.3s ease;
        }

        body.mobile-mode .mode-selector {
            gap: 0.5rem;
            margin-bottom: 1rem;
        }

        .mode-btn {
            background: #667eea;
            color: white;
            border: none;
            padding: 0.8rem 1.5rem;
            border-radius: 10px;
            cursor: pointer;
            font-size: 1rem;
            transition: all 0.3s;
        }

        body.mobile-mode .mode-btn {
            padding: 0.6rem 1rem;
            font-size: 0.8rem;
            border-radius: 6px;
        }

        .mode-btn:hover {
            background: #5a67d8;
            transform: translateY(-2px);
        }

        .mode-btn.active {
            background: #4c51bf;
        }

        .controls {
            margin-bottom: 2rem;
            display: flex;
            gap: 1rem;
            align-items: center;
            justify-content: center;
            flex-wrap: wrap;
            transition: all 0.3s ease;
        }

        body.mobile-mode .controls {
            gap: 0.5rem;
            margin-bottom: 1rem;
        }

        .language-toggle {
            background: #48bb78;
            color: white;
            border: none;
            padding: 0.6rem 1.2rem;
            border-radius: 8px;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.3s;
        }

        body.mobile-mode .language-toggle {
            padding: 0.5rem 1rem;
            font-size: 0.8rem;
        }

        .quiz-length {
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .quiz-length button {
            background: #ed8936;
            color: white;
            border: none;
            padding: 0.4rem 0.8rem;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.3s;
        }

        body.mobile-mode .quiz-length button {
            padding: 0.3rem 0.6rem;
            font-size: 0.8rem;
        }

        /* Flashcard flip container */
        .flashcard-container {
            perspective: 1000px;
            margin: 2rem 0;
            height: 200px;
            position: relative;
        }

        body.mobile-mode .flashcard-container {
            height: 150px;
            margin: 1rem 0;
        }

        .flashcard {
            background: linear-gradient(145deg, #f7fafc, #edf2f7);
            border: 2px solid #e2e8f0;
            border-radius: 15px;
            padding: 3rem 2rem;
            cursor: pointer;
            transition: transform 0.6s;
            transform-style: preserve-3d;
            position: absolute;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            font-weight: bold;
            color: #2d3748;
        }

        body.mobile-mode .flashcard {
            padding: 2rem 1rem;
            font-size: 1.2rem;
        }

        .flashcard.flipped {
            transform: rotateY(180deg);
        }

        .flashcard-front, .flashcard-back {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            display: flex;
	    flex-direction: column;
            align-items: center;
            justify-content: center;
            border-radius: 15px;
            padding: 2rem;
            text-align: center;
            word-wrap: break-word;
        }

        .flashcard-front {
            background: linear-gradient(145deg, #f7fafc, #edf2f7);
            border: 2px solid #e2e8f0;
        }

        .flashcard-back {
            background: linear-gradient(145deg, #e6fffa, #b2f5ea);
            border: 2px solid #4fd1c7;
            transform: rotateY(180deg);
        }

        .flashcard-container.slide-out {
            animation: slideOut 0.5s ease-in forwards;
        }

        .flashcard-container.slide-in {
            animation: slideIn 0.5s ease-out forwards;
        }

        @keyframes slideOut {
            0% {
                transform: translateX(0) scale(1);
                opacity: 1;
            }
            100% {
                transform: translateX(100%) scale(0.8);
                opacity: 0;
            }
        }

        @keyframes slideIn {
            0% {
                transform: translateX(-100%) scale(0.8);
                opacity: 0;
            }
            100% {
                transform: translateX(0) scale(1);
                opacity: 1;
            }
        }

        .quiz-container {
            text-align: center;
        }

        .question-container {
            perspective: 1000px;
            margin-bottom: 2rem;
            height: auto;
            min-height: 120px;
        }

        body.mobile-mode .question-container {
            min-height: 100px;
        }

        .question {
            background: #f7fafc;
            padding: 2rem;
            border-radius: 10px;
            font-size: 1.5rem;
            font-weight: bold;
            color: #2d3748;
            transition: transform 0.6s;
            transform-style: preserve-3d;
            position: relative;
            width: 100%;
            min-height: 120px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .question.flipped {
            transform: rotateY(180deg);
        }

        .question-front, .question-back {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            display: flex;
	    flex-direction: column;
            align-items: center;
            justify-content: center;
            border-radius: 10px;
            padding: 2rem;
            text-align: center;
            word-wrap: break-word;
            font-size: 1.5rem;
            font-weight: bold;
        }

        .question-front {
            background: #f7fafc;
            color: #2d3748;
        }

        .question-back {
            background: linear-gradient(145deg, #e6fffa, #b2f5ea);
            color: #2d3748;
            transform: rotateY(180deg);
        }

        body.mobile-mode .question-front, 
        body.mobile-mode .question-back {
            padding: 1.5rem;
            font-size: 1.2rem;
        }

        .options {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 1rem;
            margin-bottom: 2rem;
            transition: all 0.3s ease;
        }

        body.mobile-mode .options {
            grid-template-columns: 1fr;
            gap: 0.5rem;
            margin-bottom: 1rem;
        }

        .option {
            perspective: 1000px;
            height: 60px;
            position: relative;
        }

        body.mobile-mode .option {
            height: 50px;
        }

        .option-card {
            background: #e2e8f0;
            border: 2px solid transparent;
            border-radius: 10px;
            cursor: pointer;
            transition: transform 0.6s;
            transform-style: preserve-3d;
            position: absolute;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1rem;
            font-weight: 500;
        }

        body.mobile-mode .option-card {
            font-size: 0.9rem;
        }

        .option-card:hover {
            background: #cbd5e0;
            transform: translateY(-2px);
        }

        .option-card.flipped {
            transform: rotateY(180deg);
        }

        .option-front, .option-back {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 10px;
            padding: 1rem;
            text-align: center;
            word-wrap: break-word;
        }

        .option-front {
            background: #e2e8f0;
            border: 2px solid transparent;
            color: #2d3748;
        }

        .option-back {
            transform: rotateY(180deg);
            font-weight: bold;
            font-size: 1.1rem;
        }

        .option-back.correct {
            background: linear-gradient(145deg, #c6f6d5, #9ae6b4);
            border: 2px solid #48bb78;
            color: #22543d;
        }

        .option-back.incorrect {
            background: linear-gradient(145deg, #fed7d7, #feb2b2);
            border: 2px solid #f56565;
            color: #742a2a;
        }

        .option.correct-highlight .option-front {
            background: linear-gradient(145deg, #c6f6d5, #9ae6b4);
            border: 2px solid #48bb78;
            color: #22543d;
            animation: pulse 0.5s ease-in-out;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .final-results {
            background: linear-gradient(145deg, #e6fffa, #b2f5ea);
            border: 2px solid #4fd1c7;
            padding: 2rem;
            border-radius: 15px;
            text-align: center;
            font-size: 1.5rem;
            font-weight: bold;
            color: #2d3748;
            cursor: pointer;
            transition: all 0.3s;
            margin: 2rem 0;
        }

        .final-results:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
        }

        body.mobile-mode .final-results {
            padding: 1.5rem;
            font-size: 1.2rem;
        }

        .score {
            background: #4a5568;
            color: white;
            padding: 0.5rem 1rem;
            border-radius: 8px;
            margin-bottom: 1rem;
            display: inline-block;
            transition: all 0.3s ease;
        }

        body.mobile-mode .score {
            padding: 0.4rem 0.8rem;
            font-size: 0.9rem;
        }

        .tense-selector {
            display: flex;
            gap: 0.5rem;
            margin-bottom: 1rem;
            flex-wrap: wrap;
            justify-content: center;
        }

        body.mobile-mode .tense-selector {
            gap: 0.3rem;
        }

        .tense-btn {
            background: #4299e1;
            color: white;
            border: none;
            padding: 0.5rem 1rem;
            border-radius: 6px;
            cursor: pointer;
            font-size: 0.8rem;
            transition: all 0.3s;
        }

        body.mobile-mode .tense-btn {
            padding: 0.4rem 0.8rem;
            font-size: 0.7rem;
        }

        .tense-btn:hover {
            background: #3182ce;
        }

        .tense-btn.active {
            background: #2b6cb0;
        }

        .hidden {
            display: none;
        }

        /* New style for tense indicator below verb phrase */
        .tense-indicator {
            margin-top: 2rem;
            font-style: italic;
            color: #4a5568;
            font-size: 1rem;
        }
        
        body.mobile-mode .tense-indicator {
            font-size: 0.9rem;
            margin-top: 0.8rem;
        }

        @media (max-width: 600px) {
            .game-container {
                padding: 1rem;
            }
            
            .options {
                grid-template-columns: 1fr;
            }
            
            .mode-selector {
                flex-direction: column;
            }

            .top-controls {
                justify-content: center;
            }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <div class="top-controls">
            <h1>FR204 Module 2: Le jeu</h1>
            <button class="mobile-toggle" onclick="toggleMobileMode()">
                📱 <span id="mobile-mode-text">Mode Mobile</span>
            </button>
        </div>
        
        <div class="mode-selector">
            <button class="mode-btn active" onclick="setMode('flashcard')">Flashcards: Vocabulaire</button>
            <button class="mode-btn" onclick="setMode('flashcard-verb')">Flashcards: Conjugaisons</button>
            <button class="mode-btn" onclick="setMode('quiz')">Quiz: Vocabulaire</button>
            <button class="mode-btn" onclick="setMode('verb')">Quiz: Conjugaisons</button>
        </div>

        <div class="controls">
            <button class="language-toggle" onclick="toggleLanguage()">
                <span id="lang-display">Français → Anglais</span>
            </button>
            
            <div class="quiz-length" id="quiz-controls">
                <span>Longueur:</span>
                <button onclick="adjustQuizLength(-5)">-</button>
                <span id="quiz-length-display">10</span>
                <button onclick="adjustQuizLength(5)">+</button>
            </div>
        </div>

        <div id="score-display" class="score hidden">Score: <span id="score">0</span>/<span id="total">0</span></div>

        <!-- Flashcard Mode -->
        <div id="flashcard-mode">
            <div class="flashcard-container" id="flashcard-container">
                <div class="flashcard" onclick="flipCard()" id="flashcard">
                    <div class="flashcard-front">
                        <div id="flashcard-content-front">Cliquez pour commencer</div>
                    </div>
                    <div class="flashcard-back">
                        <div id="flashcard-content-back"></div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Flashcard Verb Mode -->
        <div id="flashcard-verb-mode" class="hidden">
            <div class="tense-selector">
                <button class="tense-btn active" onclick="setFlashcardTense('present')">Présent</button>
                <button class="tense-btn" onclick="setFlashcardTense('future')">Futur</button>
                <button class="tense-btn" onclick="setFlashcardTense('passe')">Passé composé</button>
                <button class="tense-btn" onclick="setFlashcardTense('imperfect')">Imparfait</button>
                <button class="tense-btn" onclick="setFlashcardTense('conditional')">Conditionnel</button>
                <button class="tense-btn" onclick="setFlashcardTense('subjunctive')">Subjonctif</button>
                <button class="tense-btn" onclick="setFlashcardTense('plus-que-parfait')">Plus-que-parfait</button>
                <button class="tense-btn" onclick="setFlashcardTense('passe-simple')">Passé simple</button>
                <button class="tense-btn" onclick="setFlashcardTense('futur-anterieur')">Futur antérieur</button>
                <button class="tense-btn" onclick="setFlashcardTense('random')">Aléatoire</button>
            </div>
            <div class="flashcard-container" id="flashcard-verb-container">
                <div class="flashcard" onclick="flipVerbCard()" id="flashcard-verb">
                    <div class="flashcard-front">
                        <div id="flashcard-verb-content-front">Cliquez pour commencer</div>
                        <div class="tense-indicator" id="flashcard-tense-indicator"></div>
                    </div>
                    <div class="flashcard-back">
                        <div id="flashcard-verb-content-back"></div>
                        <div class="tense-indicator" id="flashcard-tense-indicator-back"></div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Quiz Mode -->
        <div id="quiz-mode" class="hidden">
            <div class="quiz-container">
                <div class="question-container">
                    <div class="question" id="quiz-question">
                        <div class="question-front">
                            <div id="quiz-question-text">Question apparaîtra ici</div>
                        </div>
                        <div class="question-back">
                            <div id="quiz-feedback-text"></div>
                        </div>
                    </div>
                </div>
                <div class="options" id="quiz-options"></div>
            </div>
        </div>

        <!-- Verb Mode -->
        <div id="verb-mode" class="hidden">
            <div class="tense-selector">
                <button class="tense-btn active" onclick="setTense('present')">Présent</button>
                <button class="tense-btn" onclick="setTense('future')">Futur</button>
                <button class="tense-btn" onclick="setTense('passe')">Passé composé</button>
                <button class="tense-btn" onclick="setTense('imperfect')">Imparfait</button>
                <button class="tense-btn" onclick="setTense('conditional')">Conditionnel</button>
                <button class="tense-btn" onclick="setTense('subjunctive')">Subjonctif</button>
                <button class="tense-btn" onclick="setTense('plus-que-parfait')">Plus-que-parfait</button>
                <button class="tense-btn" onclick="setTense('passe-simple')">Passé simple</button>
                <button class="tense-btn" onclick="setTense('futur-anterieur')">Futur antérieur</button>
                <button class="tense-btn" onclick="setTense('random')">Aléatoire</button>
            </div>
            <div class="quiz-container">
                <div class="question-container">
                    <div class="question" id="verb-question">
                        <div class="question-front">
                            <div id="verb-question-text">Question apparaîtra ici</div>
                            <div class="tense-indicator" id="verb-tense-indicator"></div>
                        </div>
                        <div class="question-back">
                            <div id="verb-feedback-text"></div>
                            <div class="tense-indicator" id="verb-tense-indicator-back"></div>
                        </div>
                    </div>
                </div>
                <div class="options" id="verb-options"></div>
            </div>
        </div>
    </div>

    <script>
        // Vocabulary data
        const vocab = {
            nouns: [
                {fr: "Bénéfice", ang: "benefits"},
                {fr: "Bien-être", ang: "well-being"},
                {fr: "Chômage", ang: "unemployment"},
                {fr: "Commerce", ang: "trade"},
                {fr: "Concurrence", ang: "competition"},
                {fr: "Croissance", ang: "growth"},
                {fr: "Déchets", ang: "waste, garbage"},
                {fr: "Dégâts", ang: "damage"},
                {fr: "Développement durable", ang: "sustainable development"},
                {fr: "Droits", ang: "rights"},
                {fr: "Écart", ang: "gap"},
                {fr: "Économie", ang: "economy"},
                {fr: "Économies", ang: "savings"},
                {fr: "Effet de serre", ang: "greenhouse effect"},
                {fr: "Exportateur(-trice)", ang: "exporter"},
                {fr: "Importateur(-trice)", ang: "importer"},
                {fr: "Inégalité", ang: "inequality"},
                {fr: "Libre-échange", ang: "free trade"},
                {fr: "Lutte", ang: "struggle"},
                {fr: "Main d'œuvre", ang: "labor, workforce"},
                {fr: "Marché", ang: "market"},
                {fr: "Matières premières", ang: "raw materials"},
                {fr: "Mondialisation", ang: "globalization"},
                {fr: "Niveau de vie", ang: "standard of living"},
                {fr: "Ouvrier(-ière)", ang: "worker"},
                {fr: "Pauvreté", ang: "poverty"},
                {fr: "Perte", ang: "loss"},
                {fr: "Prix", ang: "price"},
                {fr: "Produit", ang: "product"},
                {fr: "Réchauffement climatique", ang: "global warming"},
                {fr: "Revenu", ang: "income, earnings"},
                {fr: "Terre", ang: "Earth"}
            ],
            verbs: [
                {fr: "Agir", ang: "to act"},
                {fr: "Avoir l'habitude de", ang: "to usually do"},
                {fr: "Avoir les moyens de", ang: "to have the means to"},
                {fr: "Baisser", ang: "to lower"},
                {fr: "Critiquer", ang: "to criticize"},
                {fr: "Se déplacer", ang: "to get around, to move"},
                {fr: "Diminuer", ang: "to reduce"},
                {fr: "Élargir", ang: "to broaden"},
                {fr: "Emprunter", ang: "to borrow"},
                {fr: "Éviter", ang: "to avoid"},
                {fr: "Exporter", ang: "to export"},
                {fr: "Fabriquer", ang: "to manufacture"},
                {fr: "Faire concurrence à", ang: "to compete with"},
                {fr: "Importer", ang: "to import"},
                {fr: "Lutter", ang: "to fight"},
                {fr: "Menacer", ang: "to threaten"},
                {fr: "S'opposer à", ang: "to be opposed (to)"},
                {fr: "Prêter", ang: "to lend"},
                {fr: "Produire", ang: "to produce"},
                {fr: "Ralentir", ang: "to slow down"}
            ],
            adjectives: [
                {fr: "Agricole", ang: "agricultural"},
                {fr: "Avantageux(-euse)", ang: "advantageous"},
                {fr: "Concurrentiel(le)", ang: "competitive"},
                {fr: "Disponible", ang: "available"},
                {fr: "Économique", ang: "economic/economical"},
                {fr: "Écologique", ang: "environmental, ecological"},
                {fr: "Mondial(e)", ang: "global/worldwide"},
                {fr: "Qualifié(e)", ang: "skilled"}
            ],
            expressions: [
                {fr: "À l'échelle mondiale", ang: "worldwide, on a global scale"},
                {fr: "À l'étranger", ang: "abroad, overseas"},
                {fr: "Bon marché", ang: "cheap, inexpensive"}
            ]
        };

        // Verb conjugations with added tenses
        const verbConjugations = {
            agir: {
                present: {je: "agis", tu: "agis", il: "agit", elle: "agit", nous: "agissons", vous: "agissez", ils: "agissent", elles: "agissent"},
                future: {je: "agirai", tu: "agiras", il: "agira", elle: "agira", nous: "agirons", vous: "agirez", ils: "agiront", elles: "agiront"},
                passe: {je: "ai agi", tu: "as agi", il: "a agi", elle: "a agi", nous: "avons agi", vous: "avez agi", ils: "ont agi", elles: "ont agi"},
                imperfect: {je: "agissais", tu: "agissais", il: "agissait", elle: "agissait", nous: "agissions", vous: "agissiez", ils: "agissaient", elles: "agissaient"},
                conditional: {je: "agirais", tu: "agirais", il: "agirait", elle: "agirait", nous: "agirions", vous: "agiriez", ils: "agiraient", elles: "agiraient"},
                subjunctive: {je: "agisse", tu: "agisses", il: "agisse", elle: "agisse", nous: "agissions", vous: "agissiez", ils: "agissent", elles: "agissent"},
                "plus-que-parfait": {je: "avais agi", tu: "avais agi", il: "avait agi", elle: "avait agi", nous: "avions agi", vous: "aviez agi", ils: "avaient agi", elles: "avaient agi"},
                "passe-simple": {je: "agis", tu: "agis", il: "agit", elle: "agit", nous: "agîmes", vous: "agîtes", ils: "agirent", elles: "agirent"},
                "futur-anterieur": {je: "aurai agi", tu: "auras agi", il: "aura agi", elle: "aura agi", nous: "aurons agi", vous: "aurez agi", ils: "auront agi", elles: "auront agi"}
            },
            baisser: {
                present: {je: "baisse", tu: "baisses", il: "baisse", elle: "baisse", nous: "baissons", vous: "baissez", ils: "baissent", elles: "baissent"},
                future: {je: "baisserai", tu: "baisseras", il: "baissera", elle: "baissera", nous: "baisserons", vous: "baisserez", ils: "baisseront", elles: "baisseront"},
                passe: {je: "ai baissé", tu: "as baissé", il: "a baissé", elle: "a baissé", nous: "avons baissé", vous: "avez baissé", ils: "ont baissé", elles: "ont baissé"},
                imperfect: {je: "baissais", tu: "baissais", il: "baissait", elle: "baissait", nous: "baissions", vous: "baissiez", ils: "baissaient", elles: "baissaient"},
                conditional: {je: "baisserais", tu: "baisserais", il: "baisserait", elle: "baisserait", nous: "baisserions", vous: "baisseriez", ils: "baisseraient", elles: "baisseraient"},
                subjunctive: {je: "baisse", tu: "baisses", il: "baisse", elle: "baisse", nous: "baissions", vous: "baissiez", ils: "baissent", elles: "baissent"},
                "plus-que-parfait": {je: "avais baissé", tu: "avais baissé", il: "avait baissé", elle: "avait baissé", nous: "avions baissé", vous: "aviez baissé", ils: "avaient baissé", elles: "avaient baissé"},
                "passe-simple": {je: "baissai", tu: "baissas", il: "baissa", elle: "baissa", nous: "baissâmes", vous: "baissâtes", ils: "baissèrent", elles: "baissèrent"},
                "futur-anterieur": {je: "aurai baissé", tu: "auras baissé", il: "aura baissé", elle: "aura baissé", nous: "aurons baissé", vous: "aurez baissé", ils: "auront baissé", elles: "auront baissé"}
            },
            critiquer: {
                present: {je: "critique", tu: "critiques", il: "critique", elle: "critique", nous: "critiquons", vous: "critiquez", ils: "critiquent", elles: "critiquent"},
                future: {je: "critiquerai", tu: "critiqueras", il: "critiquera", elle: "critiquera", nous: "critiquerons", vous: "critiquerez", ils: "critiqueront", elles: "critiqueront"},
                passe: {je: "ai critiqué", tu: "as critiqué", il: "a critiqué", elle: "a critiqué", nous: "avons critiqué", vous: "avez critiqué", ils: "ont critiqué", elles: "ont critiqué"},
                imperfect: {je: "critiquais", tu: "critiquais", il: "critiquait", elle: "critiquait", nous: "critiquions", vous: "critiquiez", ils: "critiquaient", elles: "critiquaient"},
                conditional: {je: "critiquerais", tu: "critiquerais", il: "critiquerait", elle: "critiquerait", nous: "critiquerions", vous: "critiqueriez", ils: "critiqueraient", elles: "critiqueraient"},
                subjunctive: {je: "critique", tu: "critiques", il: "critique", elle: "critique", nous: "critiquions", vous: "critiquiez", ils: "critiquent", elles: "critiquent"},
                "plus-que-parfait": {je: "avais critiqué", tu: "avais critiqué", il: "avait critiqué", elle: "avait critiqué", nous: "avions critiqué", vous: "aviez critiqué", ils: "avaient critiqué", elles: "avaient critiqué"},
                "passe-simple": {je: "critiquai", tu: "critiquas", il: "critiqua", elle: "critiqua", nous: "critiquâmes", vous: "critiquâtes", ils: "critiquèrent", elles: "critiquèrent"},
                "futur-anterieur": {je: "aurai critiqué", tu: "auras critiqué", il: "aura critiqué", elle: "aura critiqué", nous: "aurons critiqué", vous: "aurez critiqué", ils: "auront critiqué", elles: "auront critiqué"}
            },
            diminuer: {
                present: {je: "diminue", tu: "diminues", il: "diminue", elle: "diminue", nous: "diminuons", vous: "diminuez", ils: "diminuent", elles: "diminuent"},
                future: {je: "diminuerai", tu: "diminueras", il: "diminuera", elle: "diminuera", nous: "diminuerons", vous: "diminuerez", ils: "diminueront", elles: "diminueront"},
                passe: {je: "ai diminué", tu: "as diminué", il: "a diminué", elle: "a diminué", nous: "avons diminué", vous: "avez diminué", ils: "ont diminué", elles: "ont diminué"},
                imperfect: {je: "diminuais", tu: "diminuais", il: "diminuait", elle: "diminuait", nous: "diminuions", vous: "diminuiez", ils: "diminuaient", elles: "diminuaient"},
                conditional: {je: "diminuerais", tu: "diminuerais", il: "diminuerait", elle: "diminuerait", nous: "diminuerions", vous: "diminueriez", ils: "diminueraient", elles: "diminueraient"},
                subjunctive: {je: "diminue", tu: "diminues", il: "diminue", elle: "diminue", nous: "diminuions", vous: "diminuiez", ils: "diminuent", elles: "diminuent"},
                "plus-que-parfait": {je: "avais diminué", tu: "avais diminué", il: "avait diminué", elle: "avait diminué", nous: "avions diminué", vous: "aviez diminué", ils: "avaient diminué", elles: "avaient diminué"},
                "passe-simple": {je: "diminuai", tu: "diminuas", il: "diminua", elle: "diminua", nous: "diminuâmes", vous: "diminuâtes", ils: "diminuèrent", elles: "diminuèrent"},
                "futur-anterieur": {je: "aurai diminué", tu: "auras diminué", il: "aura diminué", elle: "aura diminué", nous: "aurons diminué", vous: "aurez diminué", ils: "auront diminué", elles: "auront diminué"}
            },
            éviter: {
                present: {je: "évite", tu: "évites", il: "évite", elle: "évite", nous: "évitons", vous: "évitez", ils: "évitent", elles: "évitent"},
                future: {je: "éviterai", tu: "éviteras", il: "évitera", elle: "évitera", nous: "éviterons", vous: "éviterez", ils: "éviteront", elles: "éviteront"},
                passe: {je: "ai évité", tu: "as évité", il: "a évité", elle: "a évité", nous: "avons évité", vous: "avez évité", ils: "ont évité", elles: "ont évité"},
                imperfect: {je: "évitais", tu: "évitais", il: "évitait", elle: "évitait", nous: "évitions", vous: "évitiez", ils: "évitaient", elles: "évitaient"},
                conditional: {je: "éviterais", tu: "éviterais", il: "éviterait", elle: "éviterait", nous: "éviterions", vous: "éviteriez", ils: "éviteraient", elles: "éviteraient"},
                subjunctive: {je: "évite", tu: "évites", il: "évite", elle: "évite", nous: "évitions", vous: "évitiez", ils: "évitent", elles: "évitent"},
                "plus-que-parfait": {je: "avais évité", tu: "avais évité", il: "avait évité", elle: "avait évité", nous: "avions évité", vous: "aviez évité", ils: "avaient évité", elles: "avaient évité"},
                "passe-simple": {je: "évitai", tu: "évitas", il: "évita", elle: "évita", nous: "évitâmes", vous: "évitâtes", ils: "évitèrent", elles: "évitèrent"},
                "futur-anterieur": {je: "aurai évité", tu: "auras évité", il: "aura évité", elle: "aura évité", nous: "aurons évité", vous: "aurez évité", ils: "auront évité", elles: "auront évité"}
            },
            lutter: {
                present: {je: "lutte", tu: "luttes", il: "lutte", elle: "lutte", nous: "luttons", vous: "luttez", ils: "luttent", elles: "luttent"},
                future: {je: "lutterai", tu: "lutteras", il: "luttera", elle: "luttera", nous: "lutterons", vous: "lutterez", ils: "lutteront", elles: "lutteront"},
                passe: {je: "ai lutté", tu: "as lutté", il: "a lutté", elle: "a lutté", nous: "avons lutté", vous: "avez lutté", ils: "ont lutté", elles: "ont lutté"},
                imperfect: {je: "luttais", tu: "luttais", il: "luttait", elle: "luttait", nous: "luttions", vous: "luttiez", ils: "luttaient", elles: "luttaient"},
                conditional: {je: "lutterais", tu: "lutterais", il: "lutterait", elle: "lutterait", nous: "lutterions", vous: "lutteriez", ils: "lutteraient", elles: "lutteraient"},
                subjunctive: {je: "lutte", tu: "luttes", il: "lutte", elle: "lutte", nous: "luttions", vous: "luttiez", ils: "luttent", elles: "luttent"},
                "plus-que-parfait": {je: "avais lutté", tu: "avais lutté", il: "avait lutté", elle: "avait lutté", nous: "avions lutté", vous: "aviez lutté", ils: "avaient lutté", elles: "avaient lutté"},
                "passe-simple": {je: "luttai", tu: "luttas", il: "lutta", elle: "lutta", nous: "luttâmes", vous: "luttâtes", ils: "luttèrent", elles: "luttèrent"},
                "futur-anterieur": {je: "aurai lutté", tu: "auras lutté", il: "aura lutté", elle: "aura lutté", nous: "aurons lutté", vous: "aurez lutté", ils: "auront lutté", elles: "auront lutté"}
            }
        };

        // Game state
        let currentMode = 'flashcard';
        let currentLanguage = 'fr-ang'; // fr-ang or ang-fr
        let allWords = [];
        let currentCardIndex = 0;
        let isFlipped = false;
        let quizLength = 10;
        let currentQuizIndex = 0;
        let score = 0;
        let currentQuestion = null;
        let isAnswered = false;
        let isMobileMode = false;

        // Updated tenses array with new tenses
        const tenses = ['present', 'future', 'passe', 'imperfect', 'conditional', 'subjunctive', 'plus-que-parfait', 'passe-simple', 'futur-anterieur'];
        const pronouns = ['je', 'tu', 'il', 'elle', 'nous', 'vous', 'ils', 'elles'];
        let currentTense = 'present';
        let selectedTenseMode = 'present'; // Can be a specific tense or 'random'
        let selectedFlashcardTense = 'present';
        let currentPronoun = '';
        let currentVerb = '';

        // Verb flashcard variables
        let verbFlashcardCombos = [];
        let currentVerbComboIndex = 0;
        let isVerbFlipped = false;

        // Initialize game
        function init() {
            // Combine all vocabulary
            allWords = [...vocab.nouns, ...vocab.verbs, ...vocab.adjectives, ...vocab.expressions];
            shuffleArray(allWords);
            
            // Initialize variables
            selectedFlashcardTense = 'present';
            selectedTenseMode = 'present';
            currentTense = 'present';
            
            startFlashcards();
        }

        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }

        // Mobile mode toggle
        function toggleMobileMode() {
            isMobileMode = !isMobileMode;
            document.body.classList.toggle('mobile-mode', isMobileMode);
            document.getElementById('mobile-mode-text').textContent = 
                isMobileMode ? 'Mode Desktop' : 'Mode Mobile';
        }

        function setMode(mode) {
            currentMode = mode;
            
            // Update button states
            document.querySelectorAll('.mode-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
            
            // Show/hide appropriate elements
            document.getElementById('flashcard-mode').classList.toggle('hidden', mode !== 'flashcard');
            document.getElementById('flashcard-verb-mode').classList.toggle('hidden', mode !== 'flashcard-verb');
            document.getElementById('quiz-mode').classList.toggle('hidden', mode !== 'quiz');
            document.getElementById('verb-mode').classList.toggle('hidden', mode !== 'verb');
            document.getElementById('quiz-controls').classList.toggle('hidden', mode === 'flashcard' || mode === 'flashcard-verb');
            document.getElementById('score-display').classList.toggle('hidden', mode === 'flashcard' || mode === 'flashcard-verb');
            
            // Start appropriate mode
            if (mode === 'flashcard') {
                startFlashcards();
            } else if (mode === 'flashcard-verb') {
                startVerbFlashcards();
            } else if (mode === 'quiz') {
                startQuiz();
            } else if (mode === 'verb') {
                startVerbQuiz();
            }
        }

        function toggleLanguage() {
            currentLanguage = currentLanguage === 'fr-ang' ? 'ang-fr' : 'fr-ang';
            document.getElementById('lang-display').textContent = 
                currentLanguage === 'fr-ang' ? 'Français → Anglais' : 'Anglais → Français';
            
            if (currentMode === 'flashcard') {
                isFlipped = false;
                document.getElementById('flashcard').classList.remove('flipped');
                showCard();
            } else if (currentMode === 'flashcard-verb') {
                // Language toggle doesn't affect verb flashcards
            } else if (currentMode === 'quiz') {
                startQuiz();
            }
        }

        function adjustQuizLength(change) {
            quizLength = Math.max(5, Math.min(50, quizLength + change));
            document.getElementById('quiz-length-display').textContent = quizLength;
        }

        // Flashcard Mode
        function startFlashcards() {
            currentCardIndex = 0;
            isFlipped = false;
            document.getElementById('flashcard').classList.remove('flipped');
            document.getElementById('flashcard-container').classList.remove('slide-out', 'slide-in');
            showCard();
        }

        function showCard() {
            if (allWords.length === 0) return;
            
            const word = allWords[currentCardIndex];
            const frontContent = currentLanguage === 'fr-ang' ? word.fr : word.ang;
            const backContent = currentLanguage === 'fr-ang' ? word.ang : word.fr;
            
            document.getElementById('flashcard-content-front').textContent = frontContent;
            document.getElementById('flashcard-content-back').textContent = backContent;
        }

        function flipCard() {
            const flashcard = document.getElementById('flashcard');
            
            if (!isFlipped) {
                // Flip to show answer
                flashcard.classList.add('flipped');
                isFlipped = true;
            } else {
                // Move to next card with slide animation
                const container = document.getElementById('flashcard-container');
                container.classList.add('slide-out');
                
                setTimeout(() => {
                    // Move to next card
                    currentCardIndex = (currentCardIndex + 1) % allWords.length;
                    isFlipped = false;
                    flashcard.classList.remove('flipped');
                    showCard();
                    
                    // Slide in new card
                    container.classList.remove('slide-out');
                    container.classList.add('slide-in');
                    
                    setTimeout(() => {
                        container.classList.remove('slide-in');
                    }, 500);
                }, 500);
            }
        }

        // Verb Flashcard Mode
        function startVerbFlashcards() {
            generateVerbFlashcardCombos();
            currentVerbComboIndex = 0;
            isVerbFlipped = false;
            document.getElementById('flashcard-verb').classList.remove('flipped');
            document.getElementById('flashcard-verb-container').classList.remove('slide-out', 'slide-in');
            showVerbCard();
        }

        function generateVerbFlashcardCombos() {
            verbFlashcardCombos = [];
            const verbKeys = Object.keys(verbConjugations);
            
            // Generate all combinations
            verbKeys.forEach(verb => {
                pronouns.forEach(pronoun => {
                    if (selectedFlashcardTense === 'random') {
                        // For random, create combinations with all tenses
                        tenses.forEach(tense => {
                            verbFlashcardCombos.push({ verb, pronoun, tense });
                        });
                    } else {
                        // For specific tense, only that tense
                        verbFlashcardCombos.push({ verb, pronoun, tense: selectedFlashcardTense });
                    }
                });
            });
            
            // Randomize the order
            shuffleArray(verbFlashcardCombos);
        }

        function setFlashcardTense(tense) {
            selectedFlashcardTense = tense;
            
            // Update button states - find the clicked button
            const clickedButton = Array.from(document.querySelectorAll('#flashcard-verb-mode .tense-btn'))
                .find(btn => btn.textContent === event.target.textContent);
            
            document.querySelectorAll('#flashcard-verb-mode .tense-btn').forEach(btn => btn.classList.remove('active'));
            if (clickedButton) {
                clickedButton.classList.add('active');
            } else {
                event.target.classList.add('active');
            }
            
            // Restart flashcards with new tense
            startVerbFlashcards();
        }

        function showVerbCard() {
            if (verbFlashcardCombos.length === 0) return;
            
            const combo = verbFlashcardCombos[currentVerbComboIndex];
            const { verb, pronoun, tense } = combo;
            
            const conjugation = verbConjugations[verb][tense][pronoun];
            const formattedPronoun = formatPronounWithVerb(pronoun, conjugation, tense);
            
            const frontContent = `${formattedPronoun} _____ (${verb})`;
            const backContent = `${formattedPronoun} ${conjugation}`;
            
            document.getElementById('flashcard-verb-content-front').textContent = frontContent;
            document.getElementById('flashcard-verb-content-back').textContent = backContent;
            
            // Display tense indicator below the phrase
            document.getElementById('flashcard-tense-indicator').textContent = getTenseName(tense);
            document.getElementById('flashcard-tense-indicator-back').textContent = getTenseName(tense);
        }

        function flipVerbCard() {
            const flashcard = document.getElementById('flashcard-verb');
            
            if (!isVerbFlipped) {
                // Flip to show answer
                flashcard.classList.add('flipped');
                isVerbFlipped = true;
            } else {
                // Move to next combination with slide animation
                const container = document.getElementById('flashcard-verb-container');
                container.classList.add('slide-out');
                
                setTimeout(() => {
                    // Move to next combination
                    currentVerbComboIndex = (currentVerbComboIndex + 1) % verbFlashcardCombos.length;
                    isVerbFlipped = false;
                    flashcard.classList.remove('flipped');
                    showVerbCard();
                    
                    // Slide in new card
                    container.classList.remove('slide-out');
                    container.classList.add('slide-in');
                    
                    setTimeout(() => {
                        container.classList.remove('slide-in');
                    }, 500);
                }, 500);
            }
        }

        // Quiz Mode
        function startQuiz() {
            currentQuizIndex = 0;
            score = 0;
            updateScoreDisplay();
            shuffleArray(allWords);
            nextQuestion();
        }

        function nextQuestion() {
            if (currentQuizIndex >= quizLength) {
                showFinalScore();
                return;
            }
            
            isAnswered = false;
            const word = allWords[currentQuizIndex % allWords.length];
            currentQuestion = word;
            
            const question = currentLanguage === 'fr-ang' ? word.fr : word.ang;
            const correctAnswer = currentLanguage === 'fr-ang' ? word.ang : word.fr;
            
            document.getElementById('quiz-question-text').textContent = question;
            
            // Generate wrong answers
            const wrongAnswers = getRandomWrongAnswers(word, 3);
            const options = [correctAnswer, ...wrongAnswers];
            shuffleArray(options);
            
            const optionsContainer = document.getElementById('quiz-options');
            optionsContainer.innerHTML = '';
            
            options.forEach(option => {
                const optionDiv = document.createElement('div');
                optionDiv.className = 'option';
                
                const optionCard = document.createElement('div');
                optionCard.className = 'option-card';
                optionCard.onclick = () => selectAnswer(optionCard, option, correctAnswer);
                
                const front = document.createElement('div');
                front.className = 'option-front';
                front.textContent = option;
                
                const back = document.createElement('div');
                back.className = 'option-back';
                
                optionCard.appendChild(front);
                optionCard.appendChild(back);
                optionDiv.appendChild(optionCard);
                optionsContainer.appendChild(optionDiv);
            });
        }

        // Helper function to check if an object exists in an array
function arrayIncludes(array, object) {
    return array.some(item => 
        item.fr === object.fr && item.ang === object.ang
    );
}

function getRandomWrongAnswers(correctWord, count) {
    const wrongAnswers = [];
    
    // Determine the part of speech of the correct word
    let correctPartOfSpeech = '';
    if (arrayIncludes(vocab.nouns, correctWord)) {
        correctPartOfSpeech = 'nouns';
    } else if (arrayIncludes(vocab.verbs, correctWord)) {
        correctPartOfSpeech = 'verbs';
    } else if (arrayIncludes(vocab.adjectives, correctWord)) {
        correctPartOfSpeech = 'adjectives';
    } else if (arrayIncludes(vocab.expressions, correctWord)) {
        correctPartOfSpeech = 'expressions';
    }
    
    // Get words from the same part of speech
    let sameCategoryWords = [];
    if (correctPartOfSpeech) {
        sameCategoryWords = vocab[correctPartOfSpeech].filter(w => !(w.fr === correctWord.fr && w.ang === correctWord.ang));
    } else {
        // Fallback if we can't determine the category
        sameCategoryWords = allWords.filter(w => !(w.fr === correctWord.fr && w.ang === correctWord.ang));
    }
    
    // If we don't have enough words in the same category, use all words
    const availableWords = sameCategoryWords.length >= count ? sameCategoryWords : allWords.filter(w => !(w.fr === correctWord.fr && w.ang === correctWord.ang));
    
    while (wrongAnswers.length < count && availableWords.length > 0) {
        const randomIndex = Math.floor(Math.random() * availableWords.length);
        const randomWord = availableWords[randomIndex];
        const answer = currentLanguage === 'fr-ang' ? randomWord.ang : randomWord.fr;
        
        if (!wrongAnswers.includes(answer)) {
            wrongAnswers.push(answer);
        }
        availableWords.splice(randomIndex, 1);
    }
    
    return wrongAnswers;
}

        function selectAnswer(optionCard, selected, correct) {
            if (isAnswered) return;
            isAnswered = true;
            
            const isCorrect = selected === correct;
            const allOptions = document.querySelectorAll('#quiz-options .option-card');
            
            // Disable all clicks
            allOptions.forEach(card => {
                card.style.pointerEvents = 'none';
            });
            
            if (isCorrect) {
                // Flip selected card to show correct feedback
                const back = optionCard.querySelector('.option-back');
                back.className = 'option-back correct';
                back.innerHTML = getPositiveFeedback();
                optionCard.classList.add('flipped');
                
                score++;
                setTimeout(() => {
                    currentQuizIndex++;
                    nextQuestion();
                }, 1200);
            } else {
                // Flip selected card to show incorrect feedback
                const back = optionCard.querySelector('.option-back');
                back.className = 'option-back incorrect';
                back.innerHTML = getNegativeFeedback();
                optionCard.classList.add('flipped');
                
                // Highlight correct answer
                allOptions.forEach(card => {
                    const front = card.querySelector('.option-front');
                    if (front.textContent === correct) {
                        card.parentElement.classList.add('correct-highlight');
                    }
                });
                
                setTimeout(() => {
                    currentQuizIndex++;
                    nextQuestion();
                }, 2500);
            }
            
            updateScoreDisplay();
        }

        function getPositiveFeedback() {
            const positive = ['Très bien! 👍', 'Excellent! 😊', 'Bravo! 🎉', 'Parfait! ⭐'];
            return positive[Math.floor(Math.random() * positive.length)];
        }

        function getNegativeFeedback() {
            const negative = ['Pas tout à fait... 😔', 'Essayez encore! 💪', 'Presque! 🤔', 'Oops! 😅'];
            return negative[Math.floor(Math.random() * negative.length)];
        }

        // Verb Conjugation Mode
        function startVerbQuiz() {
            currentQuizIndex = 0;
            score = 0;
            updateScoreDisplay();
            nextVerbQuestion();
        }

        function setTense(tense) {
            selectedTenseMode = tense;
            
            // Update button states - find the clicked button
            const clickedButton = Array.from(document.querySelectorAll('#verb-mode .tense-btn'))
                .find(btn => btn.textContent === event.target.textContent);
            
            document.querySelectorAll('#verb-mode .tense-btn').forEach(btn => btn.classList.remove('active'));
            if (clickedButton) {
                clickedButton.classList.add('active');
            } else {
                event.target.classList.add('active');
            }
            
            // If currently in a verb quiz, restart it
            if (currentMode === 'verb') {
                startVerbQuiz();
            }
        }

        function nextVerbQuestion() {
            if (currentQuizIndex >= quizLength) {
                showFinalScore();
                return;
            }
            
            isAnswered = false;
            
            // Select random verb and pronoun
            const verbKeys = Object.keys(verbConjugations);
            currentVerb = verbKeys[Math.floor(Math.random() * verbKeys.length)];
            currentPronoun = pronouns[Math.floor(Math.random() * pronouns.length)];
            
            // Select tense based on mode
            if (selectedTenseMode === 'random') {
                currentTense = tenses[Math.floor(Math.random() * tenses.length)];
            } else {
                currentTense = selectedTenseMode;
            }
            
            const correctAnswer = verbConjugations[currentVerb][currentTense][currentPronoun];
            
            const questionPronoun = formatQuestionPronoun(currentPronoun, currentTense);
            document.getElementById('verb-question-text').textContent = `${questionPronoun} _____ (${currentVerb})`;
            
            // Display tense indicator below the question
            document.getElementById('verb-tense-indicator').textContent = getTenseName(currentTense);
            document.getElementById('verb-tense-indicator-back').textContent = getTenseName(currentTense);
            
            // Generate wrong answers from other conjugations
            const wrongAnswers = getRandomVerbWrongAnswers(currentVerb, currentTense, currentPronoun, correctAnswer, 3);
            
            // Format all answers to include pronouns with proper contractions
            const answerPronoun = formatAnswerPronoun(currentPronoun, correctAnswer, currentTense);
            const formattedCorrectAnswer = `${answerPronoun} ${correctAnswer}`;
            const formattedWrongAnswers = wrongAnswers.map(answer => {
                const wrongAnswerPronoun = formatAnswerPronoun(currentPronoun, answer, currentTense);
                return `${wrongAnswerPronoun} ${answer}`;
            });
            
            const options = [formattedCorrectAnswer, ...formattedWrongAnswers];
            shuffleArray(options);
            
            const optionsContainer = document.getElementById('verb-options');
            optionsContainer.innerHTML = '';
            
            options.forEach(option => {
                const optionDiv = document.createElement('div');
                optionDiv.className = 'option';
                
                const optionCard = document.createElement('div');
                optionCard.className = 'option-card';
                optionCard.onclick = () => selectVerbAnswer(optionCard, option, formattedCorrectAnswer);
                
                const front = document.createElement('div');
                front.className = 'option-front';
                front.textContent = option;
                
                const back = document.createElement('div');
                back.className = 'option-back';
                
                optionCard.appendChild(front);
                optionCard.appendChild(back);
                optionDiv.appendChild(optionCard);
                optionsContainer.appendChild(optionDiv);
            });
        }

        function getTenseName(tense) {
            const names = {
                present: 'Présent',
                future: 'Futur',
                passe: 'Passé composé',
                imperfect: 'Imparfait',
                conditional: 'Conditionnel',
                subjunctive: 'Subjonctif',
                "plus-que-parfait": "Plus-que-parfait",
                "passe-simple": "Passé simple",
                "futur-anterieur": "Futur antérieur"
            };
            return names[tense];
        }

        function formatPronounWithVerb(pronoun, conjugation, tense) {
            // Handle "que" prefix for subjunctive with proper contraction
            let basePronouns = pronoun;
            let quePrefix = '';
            
            if (tense === 'subjunctive') {
                // Handle que + vowel-starting pronouns
                if (['il', 'elle', 'ils', 'elles'].includes(pronoun)) {
                    quePrefix = "qu'";
                } else {
                    quePrefix = 'que ';
                }
            }
            
            // Handle je contraction before vowels and unaspirated h
            if (basePronouns === 'je' && startsWithVowelOrH(conjugation)) {
                basePronouns = "j'";
            }
            
            return `${quePrefix}${basePronouns}`;
        }
        
        function formatQuestionPronoun(pronoun, tense) {
            if (tense === 'subjunctive') {
                // Handle que + vowel-starting pronouns
                if (['il', 'elle', 'ils', 'elles'].includes(pronoun)) {
                    return `qu'${pronoun}`;
                } else {
                    return `que ${pronoun}`;
                }
            }
            return pronoun;
        }
        
        function formatAnswerPronoun(pronoun, conjugation, tense) {
            let formattedPronoun = pronoun;
            
            // Handle je contraction before vowels and unaspirated h
            if (pronoun === 'je' && startsWithVowelOrH(conjugation)) {
                formattedPronoun = "j'";
            }
            
            if (tense === 'subjunctive') {
                // Handle que + vowel-starting pronouns
                if (['il', 'elle', 'ils', 'elles'].includes(pronoun)) {
                    return `qu'${pronoun}`;
                } else {
                    return `que ${formattedPronoun}`;
                }
            }
            return formattedPronoun;
        }
        
        function startsWithVowelOrH(word) {
            if (!word) return false;
            const firstChar = word.toLowerCase().charAt(0);
            
            // Check for vowels including those with diacritical marks
            const vowels = 'aeiouàáâãäåæèéêëìíîïòóôõöøùúûüýÿ';
            if (vowels.includes(firstChar)) {
                return true;
            }
            
            // Check for unaspirated h (most h's in French are unaspirated)
            // Common words that start with unaspirated h
            const unaspiratedHWords = [
                'habite', 'habitent', 'habitons', 'habitez', 'habites',
                'agit', 'agis', 'agissent', 'agissons', 'agissez', 'agissait',
                'agirai', 'agiras', 'agira', 'agirons', 'agirez', 'agiront',
                'agirais', 'agirait', 'agirions', 'agiriez', 'agiraient',
                'agisse', 'agisses', 'agissions', 'agissiez'
            ];
            
            if (firstChar === 'h') {
                // For our limited vocabulary, treat most h's as unaspirated
                // In a real app, you'd want a comprehensive list
                return unaspiratedHWords.some(w => word.toLowerCase().startsWith(w)) || 
                       word.toLowerCase().startsWith('a'); // "ai", "as", "a", "avons", etc.
            }
            
            return false;
        }

        function getRandomVerbWrongAnswers(verb, tense, pronoun, correct, count) {
            const wrongAnswers = [];
            const verbConjugation = verbConjugations[verb];
            
            // For passé composé, create specific wrong answers
            if (tense === 'passe') {
                // Wrong auxiliary + correct past participle
                const correctParts = correct.split(' ');
                if (correctParts.length === 2) {
                    const [correctAux, pastParticiple] = correctParts;
                    const wrongAux = correctAux === 'ai' ? 'suis' : 
                                   correctAux === 'as' ? 'es' :
                                   correctAux === 'a' ? 'est' :
                                   correctAux === 'avons' ? 'sommes' :
                                   correctAux === 'avez' ? 'êtes' :
                                   correctAux === 'ont' ? 'sont' : 'ai';
                    wrongAnswers.push(`${wrongAux} ${pastParticiple}`);
                }
                
                // Wrong ending variations
                const baseVerb = verb;
                let wrongEndings = [];
                if (baseVerb.endsWith('er')) {
                    wrongEndings = [`${pronoun === 'je' ? 'ai' : pronoun === 'tu' ? 'as' : pronoun === 'il' ? 'a' : pronoun === 'nous' ? 'avons' : pronoun === 'vous' ? 'avez' : 'ont'} ${baseVerb.slice(0, -2)}i`];
                } else if (baseVerb.endsWith('ir')) {
                    wrongEndings = [`${pronoun === 'je' ? 'ai' : pronoun === 'tu' ? 'as' : pronoun === 'il' ? 'a' : pronoun === 'nous' ? 'avons' : pronoun === 'vous' ? 'avez' : 'ont'} ${baseVerb.slice(0, -2)}é`];
                }
                wrongAnswers.push(...wrongEndings);
            }
            
            // Add conjugations from other tenses of the same verb (just the conjugation, not the pronoun)
            Object.keys(verbConjugation).forEach(t => {
                if (t !== tense && verbConjugation[t][pronoun]) {
                    const conj = verbConjugation[t][pronoun];
                    if (conj !== correct && !wrongAnswers.includes(conj)) {
                        wrongAnswers.push(conj);
                    }
                }
            });
            
            // Add same tense but different pronouns (just the conjugation, not the pronoun)
            Object.keys(verbConjugation[tense]).forEach(p => {
                if (p !== pronoun) {
                    const conj = verbConjugation[tense][p];
                    if (conj !== correct && !wrongAnswers.includes(conj)) {
                        wrongAnswers.push(conj);
                    }
                }
            });
            
            // If we need more, add from other verbs of the same tense (just the conjugation, not the pronoun)
            if (wrongAnswers.length < count) {
                Object.keys(verbConjugations).forEach(v => {
                    if (v !== verb && verbConjugations[v][tense] && verbConjugations[v][tense][pronoun]) {
                        const conj = verbConjugations[v][tense][pronoun];
                        if (conj !== correct && !wrongAnswers.includes(conj)) {
                            wrongAnswers.push(conj);
                        }
                    }
                });
            }
            
            shuffleArray(wrongAnswers);
            return wrongAnswers.slice(0, count);
        }

        function selectVerbAnswer(optionCard, selected, correct) {
            if (isAnswered) return;
            isAnswered = true;
            
            const isCorrect = selected === correct;
            const allOptions = document.querySelectorAll('#verb-options .option-card');
            
            // Disable all clicks
            allOptions.forEach(card => {
                card.style.pointerEvents = 'none';
            });
            
            if (isCorrect) {
                // Flip selected card to show correct feedback
                const back = optionCard.querySelector('.option-back');
                back.className = 'option-back correct';
                back.innerHTML = getPositiveFeedback();
                optionCard.classList.add('flipped');
                
                score++;
                setTimeout(() => {
                    currentQuizIndex++;
                    nextVerbQuestion();
                }, 1200);
            } else {
                // Flip selected card to show incorrect feedback
                const back = optionCard.querySelector('.option-back');
                back.className = 'option-back incorrect';
                back.innerHTML = getNegativeFeedback();
                optionCard.classList.add('flipped');
                
                // Highlight correct answer
                allOptions.forEach(card => {
                    const front = card.querySelector('.option-front');
                    if (front.textContent === correct) {
                        card.parentElement.classList.add('correct-highlight');
                    }
                });
                
                setTimeout(() => {
                    currentQuizIndex++;
                    nextVerbQuestion();
                }, 2500);
            }
            
            updateScoreDisplay();
        }

        function showFinalScore() {
            const percentage = Math.round((score / quizLength) * 100);
            let message = `Quiz terminé!<br><br>Score: ${score}/${quizLength} (${percentage}%)`;
            
            if (percentage >= 90) {
                message += '<br><br>Excellent! 🌟';
            } else if (percentage >= 70) {
                message += '<br><br>Très bien! 👍';
            } else if (percentage >= 50) {
                message += '<br><br>Bon travail! 💪';
            } else {
                message += '<br><br>Continuez à pratiquer! 📚';
            }
            
            message += '<br><br><em>Cliquez ou tapez pour recommencer</em>';
            
            // Show final results in the quiz area
            const quizContainer = document.querySelector(`#${currentMode}-mode .quiz-container`);
            const finalResults = document.createElement('div');
            finalResults.className = 'final-results';
            finalResults.innerHTML = message;
            finalResults.onclick = () => {
                finalResults.remove();
                // Reset visibility
                const questionContainer = quizContainer.querySelector('.question-container');
                const options = quizContainer.querySelector('.options');
                questionContainer.style.display = 'block';
                options.style.display = 'grid';
                
                if (currentMode === 'quiz') {
                    startQuiz();
                } else if (currentMode === 'verb') {
                    startVerbQuiz();
                }
            };
            
            // Hide current quiz elements
            const questionContainer = quizContainer.querySelector('.question-container');
            const options = quizContainer.querySelector('.options');
            
            questionContainer.style.display = 'none';
            options.style.display = 'none';
            
            // Show final results
            quizContainer.appendChild(finalResults);
        }

        function updateScoreDisplay() {
            document.getElementById('score').textContent = score;
            document.getElementById('total').textContent = currentQuizIndex + 1;
        }

        // Initialize the game
        init();
    </script>
</body>
</html>
