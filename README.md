[index.html](https://github.com/user-attachments/files/33963039/index.html)
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Messagerie Privée & Interactive</title>
    <style>
        :root {
            --bg-color: #f4f6f9;
            --chat-bg: #ffffff;
            --text-color: #333333;
            --primary-color: #4a90e2;
            --secondary-color: #f1f3f5;
            --border-color: #d1d5db;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            height: 100vh;
        }

        header {
            background-color: var(--chat-bg);
            border-bottom: 1px solid var(--border-color);
            padding: 10px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 10px;
        }

        .header-title {
            font-size: 1.1rem;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .tools-bar {
            display: flex;
            gap: 8px;
            align-items: center;
            flex-wrap: wrap;
        }

        .tool-btn {
            background-color: var(--secondary-color);
            border: 1px solid var(--border-color);
            padding: 6px 10px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 0.85rem;
            display: flex;
            align-items: center;
            gap: 5px;
        }

        .tool-btn:hover {
            background-color: #e2e8f0;
        }

        .danger-btn {
            background-color: #fee2e2;
            color: #b91c1c;
            border-color: #fca5a5;
        }
        .danger-btn:hover {
            background-color: #fecaca;
        }

        #chat-container {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 15px;
            max-width: 800px;
            width: 100%;
            margin: 0 auto;
            box-sizing: border-box;
        }

        .message {
            max-width: 75%;
            padding: 12px 16px;
            border-radius: 12px;
            line-height: 1.4;
            word-wrap: break-word;
            font-size: 0.95rem;
        }

        .message.homme {
            background-color: #e3f2fd;
            align-self: flex-end;
            border-bottom-right-radius: 2px;
        }

        .message.kh {
            background-color: var(--chat-bg);
            border: 1px solid var(--border-color);
            align-self: flex-start;
            border-bottom-left-radius: 2px;
        }

        .message.system {
            background-color: #fef3c7;
            border: 1px solid #fde68a;
            align-self: center;
            text-align: center;
            font-size: 0.85rem;
            max-width: 90%;
            color: #92400e;
        }

        .sender-name {
            font-size: 0.75rem;
            font-weight: bold;
            margin-bottom: 4px;
            color: #666;
        }

        #input-container {
            background-color: var(--chat-bg);
            border-top: 1px solid var(--border-color);
            padding: 15px;
            display: flex;
            justify-content: center;
        }

        .input-wrapper {
            max-width: 800px;
            width: 100%;
            display: flex;
            gap: 10px;
        }

        textarea {
            flex: 1;
            padding: 10px 12px;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            resize: none;
            height: 24px;
            max-height: 120px;
            font-family: inherit;
            font-size: 0.95rem;
        }

        textarea:focus {
            outline: none;
            border-color: var(--primary-color);
        }

        #send-btn {
            background-color: var(--primary-color);
            color: white;
            border: none;
            padding: 0 20px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
        }

        #send-btn:hover {
            background-color: #357abd;
        }

        /* Modal Général */
        .modal {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background-color: rgba(0,0,0,0.5);
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        .modal-content {
            background: white;
            padding: 25px;
            border-radius: 12px;
            width: 90%;
            max-width: 500px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.15);
            display: flex;
            flex-direction: column;
            gap: 15px;
            max-height: 90vh;
            overflow-y: auto;
        }
        .modal-content h3 { margin-top: 0; }
        .modal-content textarea { width: 100%; height: 150px; }
        .modal-actions { display: flex; justify-content: flex-end; gap: 10px; }

        /* Roue de la fortune style */
        .wheel-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 15px;
        }
        #canvas-wheel {
            border-radius: 50%;
            box-shadow: 0 4px 10px rgba(0,0,0,0.2);
            cursor: pointer;
        }
        .wheel-options-list {
            display: flex;
            flex-direction: column;
            gap: 5px;
            width: 100%;
            max-height: 150px;
            overflow-y: auto;
        }
        .wheel-option-row {
            display: flex;
            gap: 5px;
        }
        .wheel-option-row input { flex: 1; padding: 5px; border: 1px solid var(--border-color); border-radius: 4px; }
    </style>
</head>
<body>

    <header>
        <div class="header-title">
            <span id="current-time-display" style="font-size: 0.85rem; font-weight: normal; color: #666;">--:--</span>
            <span>💬 Messagerie Privée</span>
        </div>
        <div class="tools-bar">
            <button class="tool-btn" onclick="openCageModal()">⏳ Chrono Cage : <span id="cage-timer-label">00:00:00</span></button>
            <button class="tool-btn" onclick="openWheelModal()">🎡 Roue</button>
            <button class="tool-btn danger-btn" onclick="clearChat()">🗑️ Effacer</button>
            <button class="tool-btn" id="admin-btn" onclick="openAdminModal()" style="display:none;">⚙️ Config KH</button>
        </div>
    </header>

    <div id="chat-container"></div>

    <div id="input-container">
        <div class="input-wrapper">
            <textarea id="user-input" placeholder="Écrivez votre message..." rows="1"></textarea>
            <button id="send-btn" onclick="sendMessage()">Envoyer</button>
        </div>
    </div>

    <!-- Modal Admin KH -->
    <div id="admin-modal" class="modal">
        <div class="modal-content">
            <h3>Configuration de l'IA (Réservé à KH)</h3>
            <label>Instructions système (Prompt) :</label>
            <textarea id="admin-prompt-input"></textarea>
            <div class="modal-actions">
                <button class="tool-btn" onclick="closeAdminModal()">Annuler</button>
                <button class="tool-btn" style="background-color: var(--primary-color); color:white;" onclick="saveAdminPrompt()">Enregistrer</button>
            </div>
        </div>
    </div>

    <!-- Modal Chrono Cage -->
    <div id="cage-modal" class="modal">
        <div class="modal-content" style="text-align: center;">
            <h3>Chronomètre de Cage</h3>
            <div id="cage-display" style="font-size: 2.5rem; font-weight: bold; margin: 15px 0; color: var(--primary-color);">00:00:00</div>
            <div style="display: flex; justify-content: center; gap: 10px;">
                <button class="tool-btn" onclick="startCageTimer()" style="background-color: #d1fae5; color: #065f46;">Démarrer</button>
                <button class="tool-btn" onclick="pauseCageTimer()" style="background-color: #fef3c7; color: #92400e;">Pause</button>
                <button class="tool-btn danger-btn" onclick="resetCageTimer()">Réinitialiser</button>
            </div>
            <div class="modal-actions" style="margin-top: 20px;">
                <button class="tool-btn" onclick="closeCageModal()">Fermer</button>
            </div>
        </div>
    </div>

    <!-- Modal Roue de la Fortune -->
    <div id="wheel-modal" class="modal">
        <div class="modal-content wheel-container">
            <h3>Roue de la Chance</h3>
            <div style="position: relative;">
                <canvas id="canvas-wheel" width="280" height="280"></canvas>
                <div style="position: absolute; top: 0; left: 50%; transform: translateX(-50%); width: 0; height: 0; border-left: 10px solid transparent; border-right: 10px solid transparent; border-top: 15px solid #e74c3c;"></div>
            </div>
            <button class="tool-btn" onclick="spinWheel()" style="background-color: var(--primary-color); color: white; padding: 10px 20px; font-weight: bold;">Lancer la roue !</button>
            <div id="wheel-result" style="font-weight: bold; color: #27ae60; min-height: 24px;"></div>
            
            <details style="width: 100%;">
                <summary style="cursor: pointer; font-size: 0.85rem; color: #666;">Modifier les options</summary>
                <div class="wheel-options-list" id="wheel-options-editor" style="margin-top: 10px;"></div>
                <div style="display: flex; gap: 5px; margin-top: 8px;">
                    <input type="text" id="new-wheel-item" placeholder="Nouvelle option..." style="flex:1; padding:5px; border:1px solid var(--border-color); border-radius:4px;">
                    <button class="tool-btn" onclick="addWheelOption()">Ajouter</button>
                </div>
            </details>

            <div class="modal-actions" style="width: 100%; justify-content: space-between;">
                <button class="tool-btn" onclick="sendWheelResultToChat()" id="send-wheel-btn" disabled style="opacity: 0.5;">Insérer dans le chat</button>
                <button class="tool-btn" onclick="closeWheelModal()">Fermer</button>
            </div>
        </div>
    </div>

    <script>
        // Configuration & État
        const STORAGE_KEY = "messagerie_app_state_v3";
        let state = JSON.parse(localStorage.getItem(STORAGE_KEY)) || {
            messages: [],
            systemPrompt: "Tu es KH, un assistant personnel et confidentiel.",
            wheelOptions: ["Option 1", "Option 2", "Option 3", "Option 4", "Option 5"]
        };

        let cageState = JSON.parse(localStorage.getItem("cage_timer_v1")) || {
            seconds: 0,
            isRunning: false,
            lastTimestamp: Date.now()
        };

        // Horloge globale
        setInterval(() => {
            const now = new Date();
            document.getElementById("current-time-display").innerText = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
        }, 1000);

        // Chrono Cage en arrière-plan
        setInterval(() => {
            if (cageState.isRunning) {
                const now = Date.now();
                const diff = Math.floor((now - cageState.lastTimestamp) / 1000);
                cageState.seconds += diff;
                cageState.lastTimestamp = now;
                saveCageState();
            } else {
                cageState.lastTimestamp = Date.now();
            }
            updateCageDisplay();
        }, 1000);

        function saveState() { localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); }
        function saveCageState() { localStorage.setItem("cage_timer_v1", JSON.stringify(cageState)); updateCageDisplay(); }

        function updateCageDisplay() {
            const h = String(Math.floor(cageState.seconds / 3600)).padStart(2, '0');
            const m = String(Math.floor((cageState.seconds % 3600) / 60)).padStart(2, '0');
            const s = String(cageState.seconds % 60).padStart(2, '0');
            const timeStr = `${h}:${m}:${s}`;
            const label = document.getElementById("cage-timer-label");
            const display = document.getElementById("cage-display");
            if (label) label.innerText = timeStr;
            if (display) display.innerText = timeStr;
        }

        function startCageTimer() { cageState.isRunning = true; cageState.lastTimestamp = Date.now(); saveCageState(); }
        function pauseCageTimer() { cageState.isRunning = false; saveCageState(); }
        function resetCageTimer() { cageState.isRunning = false; cageState.seconds = 0; saveCageState(); }

        // Initialisation UI
        window.onload = function() {
            renderMessages();
            // Activer bouton admin si double clic ou mode secret (ici visible pour simplifier)
            document.getElementById("admin-btn").style.display = "block";
            document.getElementById("admin-prompt-input").value = state.systemPrompt;
            updateCageDisplay();
            setupWheel();
        };

        function renderMessages() {
            const container = document.getElementById("chat-container");
            container.innerHTML = "";
            state.messages.forEach(msg => {
                const div = document.createElement("div");
                div.className = `message ${msg.role}`;
                if(msg.role !== 'system') {
                    const nameDiv = document.createElement("div");
                    nameDiv.className = "sender-name";
                    nameDiv.innerText = msg.role === 'homme' ? 'Homme' : 'KH';
                    div.appendChild(nameDiv);
                }
                const textDiv = document.createElement("div");
                textDiv.innerText = msg.text;
                div.appendChild(textDiv);
                container.appendChild(div);
            });
            container.scrollTop = container.scrollHeight;
        }

        function clearChat() {
            if(confirm("Voulez-vous vraiment effacer toute la conversation ?")) {
                state.messages = [];
                saveState();
                renderMessages();
            }
        }

        // Envoi message API Gemini
        async function sendMessage() {
            const textarea = document.getElementById("user-input");
            const text = textarea.value.trim();
            if(!text) return;

            textarea.value = "";
            state.messages.push({ role: 'homme', text: text });
            saveState();
            renderMessages();

            // Indicateur "En train de répondre"
            const loadingId = 'loading_' + Date.now();
            state.messages.push({ role: 'kh', text: "IA est en train de répondre...", id: loadingId });
            renderMessages();

            try {
                const now = new Date();
                const timeString = `Date actuelle : ${now.toLocaleDateString()} | Heure actuelle : ${now.toLocaleTimeString()}`;
                const fullPrompt = `${state.systemPrompt}\n[Contexte temporel : ${timeString}]\n\nHistorique récent et message de l'utilisateur : ${text}`;

                // Appel API Gemini (Flash 3.8 / 2.5 compatible)
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent?key=${API_KEY_PLACEHOLDER}`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        contents: [{ parts: [{ text: fullPrompt }] }]
                    })
                });

                // Simulation clé si non configurée ou erreur
                let aiResponseText = "";
                if (response.ok) {
                    const data = await response.json();
                    aiResponseText = data.candidates[0].content.parts[0].text;
                } else if (response.status === 429) {
                    aiResponseText = "⚠️ Limite quotidienne de requêtes atteinte (Quota 429). Veuillez patienter jusqu'au renouvellement.";
                } else if (response.status === 503) {
                    aiResponseText = "⚠️ Les serveurs de Google sont temporairement surchargés (Erreur 503). Veuillez patienter quelques instants avant de réessayer.";
                } else {
                    aiResponseText = `⚠️ Erreur technique (${response.status}).`;
                }

                // Remplacer le message de chargement
                state.messages = state.messages.filter(m => m.id !== loadingId);
                state.messages.push({ role: 'kh', text: aiResponseText });
                saveState();
                renderMessages();

            } catch (error) {
                state.messages = state.messages.filter(m => m.id !== loadingId);
                state.messages.push({ role: 'kh', text: "⚠️ Erreur de connexion réseau." });
                saveState();
                renderMessages();
            }
        }

        // Modals Admin & Outils
        function openAdminModal() { document.getElementById("admin-modal").style.display = "flex"; }
        function closeAdminModal() { document.getElementById("admin-modal").style.display = "none"; }
        function saveAdminPrompt() {
            state.systemPrompt = document.getElementById("admin-prompt-input").value;
            saveState();
            closeAdminModal();
        }

        function openCageModal() { document.getElementById("cage-modal").style.display = "flex"; }
        function closeCageModal() { document.getElementById("cage-modal").style.display = "none"; }

        function openWheelModal() { document.getElementById("wheel-modal").style.display = "flex"; drawWheel(); renderWheelOptionsEditor(); }
        function closeWheelModal() { document.getElementById("wheel-modal").style.display = "none"; }

        // Roue de la fortune logique
        let wheelAngle = 0;
        let isSpinning =ફ let lastSelectedOption = "";

        function setupWheel() { renderWheelOptionsEditor(); }

        function renderWheelOptionsEditor() {
            const container = document.getElementById("wheel-options-editor");
            container.innerHTML = "";
            state.wheelOptions.forEach((opt, index) => {
                const row = document.createElement("div");
                row.className = "wheel-option-row";
                row.innerHTML = `
                    <input type="text" value="${opt}" onchange="updateWheelOption(${index}, this.value)">
                    <button class="tool-btn danger-btn" onclick="removeWheelOption(${index})">X</button>
                `;
                container.appendChild(row);
            });
            drawWheel();
        }

        function updateWheelOption(index, val) { state.wheelOptions[index] = val; saveState(); drawWheel(); }
        function removeWheelOption(index) { if(state.wheelOptions.length > 2) { state.wheelOptions.splice(index, 1); saveState(); renderWheelOptionsEditor(); } else { alert("Minimum 2 options requises."); } }
        function addWheelOption() {
            const input = document.getElementById("new-wheel-item");
            if(input.value.trim()) {
                state.wheelOptions.push(input.value.trim());
                input.value = "";
                saveState();
                renderWheelOptionsEditor();
            }
        }

        function drawWheel() {
            const canvas = document.getElementById("canvas-wheel");
            if(!canvas) return;
            const ctx = canvas.getContext("2d");
            const numSegments = state.wheelOptions.length;
            const arc = (2 * Math.PI) / numSegments;
            const colors = ["#4a90e2", "#e74c3c", "#f39c12", "#2ecc71", "#9b59b6", "#1abc9c", "#e67e22"];

            ctx.clearRect(0, 0, canvas.width, canvas.height);
            ctx.save();
            ctx.translate(140, 140);
            ctx.rotate(wheelAngle);

            for(let i = 0; i < numSegments; i++) {
                const angle = i * arc;
                ctx.beginPath();
                ctx.fillStyle = colors[i % colors.length];
                ctx.moveTo(0, 0);
                ctx.arc(0, 0, 130, angle, angle + arc, false);
                ctx.lineTo(0, 0);
                ctx.fill();
                ctx.save();
                
                ctx.rotate(angle + arc / 2);
                ctx.fillStyle = "#ffffff";
                ctx.font = "bold 13px sans-serif";
                ctx.fillText(state.wheelOptions[i].substring(0, 12), 50, 5);
                ctx.restore();
            }
            ctx.restore();
        }

        function spinWheel() {
            if(isSpinning) return;
            isSpinning = document.getElementById("send-wheel-btn").disabled = true;
            document.getElementById("wheel-result").innerText = "";

            const numSegments = state.wheelOptions.length;
            const randomSpin = Math.floor(Math.random() * 3000) + 3000; // Aléatoire
            const targetAngle = wheelAngle + randomSpin;
            
            let startTime = null;
            const duration = 3000;

            function animateSpin(currentTime) {
                if(!startTime) startTime = currentTime;
                const elapsed = currentTime - startTime;
                const progress = Math.min(elapsed / duration, 1);
                
                // Effet de décélération (easeOut)
                const ease = 1 - Math.pow(1 - progress, 3);
                wheelAngle = wheelAngle + (targetAngle - wheelAngle) * ease;
                drawWheel();

                if(progress < 1) {
                    requestAnimationFrame(animateSpin);
                } else {
                    isSpinning = false;
                    // Calculer le gagnant
                    const arc = (2 * Math.PI) / numSegments;
                    const normalizedAngle = (2 * Math.PI - (wheelAngle % (2 * Math.PI))) % (2 * Math.PI);
                    const winningIndex = Math.floor(normalizedAngle / arc) % numSegments;
                    lastSelectedOption = state.wheelOptions[winningIndex];
                    
                    document.getElementById("wheel-result").innerText = `Résultat : ${lastSelectedOption}`;
                    document.getElementById("send-wheel-btn").disabled = false;
                }
            }
            requestAnimationFrame(animateSpin);
        }

        function sendWheelResultToChat() {
            if(!lastSelectedOption) return;
            state.messages.push({ role: 'system', text: `🎡 Résultat de la roue de la chance : ${lastSelectedOption}` });
            saveState();
            renderMessages();
            closeWheelModal();
        }

        // Support touche Entrée
        document.getElementById("user-input").addEventListener("keydown", function(e) {
            if(e.key === "Enter" && !e.shiftKey) {
                e.preventDefault();
                sendMessage();
            }
        });
    </script>
</body>
</html>
