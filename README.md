# index.-htlm <!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerencia General | Red Superior</title>
    <link rel="stylesheet" href="style.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
</head>
<body>
    <div id="lock-screen">
        <div class="lock-card">
            <i class="fa-solid fa-vault"></i>
            <h2>GERENCIA GENERAL</h2>
            <p>Acceso Restringido - Ingrese Clave</p>
            <input type="password" id="pass-input" placeholder="****">
            <button onclick="Gerente.validarAcceso()">AUTORIZAR</button>
        </div>
    </div>

    <div id="main-app" style="display:none;">
        <header class="top-bar">
            <div class="brand">🚀 SOCIAL ENCRYPT <span>v2.0</span></div>
            <div id="status-mic">Voz: Lista 🎤</div>
        </header>

        <main id="chat-feed">
            <div class="msg-sys">🛡️ Conexión segura establecida con el Gerente General.</div>
        </main>

        <footer class="tool-box">
            <button class="btn-tool" onclick="Voz.iniciar()"><i class="fa-solid fa-microphone"></i></button>
            <input type="text" id="input-text" placeholder="Escriba una orden o mensaje...">
            <button class="btn-tool" onclick="Gerente.enviar()"><i class="fa-solid fa-paper-plane"></i></button>
        </footer>
    </div>
    <script src="script.js"></script>
</body>
</html> const Gerente = {
    claveMaestra: "2026",

    validarAcceso: function() {
        const input = document.getElementById('pass-input').value;
        if(input === this.claveMaestra) {
            document.getElementById('lock-screen').style.display = 'none';
            document.getElementById('main-app').style.display = 'block';
        } else {
            alert("CLAVE ERRÓNEA. ACCESO DENEGADO.");
        }
    },

    enviar: function(textoVoz) {
        const msg = textoVoz || document.getElementById('input-text').value;
        if(!msg) return;

        // --- CAJA DE HERRAMIENTAS POR VOZ ---
        if(msg.includes("limpiar todo")) {
            document.getElementById('chat-feed').innerHTML = "";
            return;
        }
        if(msg.includes("bloquear")) {
            location.reload(); // Cierra sesión inmediatamente
            return;
        }

        const feed = document.getElementById('chat-feed');
        const d = document.createElement('div');
        d.className = 'bubble';
        d.innerHTML = `<strong>🔒 CIFRADO:</strong><br>${msg}`;
        feed.appendChild(d);
        document.getElementById('input-text').value = "";
        feed.scrollTop = feed.scrollHeight;
    }
};

const Voz = {
    iniciar: function() {
        const rec = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
        rec.lang = 'es-ES';
        rec.onstart = () => { document.getElementById('status-mic').style.color = "#00d1b2"; };
        rec.onresult = (e) => {
            const orden = e.results[0][0].transcript.toLowerCase();
            document.getElementById('status-mic').style.color = "white";
            Gerente.enviar(orden);
        };
        rec.start();
    }
}; body { background: #0b0e11; color: white; font-family: 'Segoe UI', sans-serif; margin: 0; overflow: hidden; }
#lock-screen { display: flex; justify-content: center; align-items: center; height: 100vh; background: #000; }
.lock-card { background: #1a1d21; padding: 40px; border-radius: 20px; text-align: center; border: 1px solid #333; }
.lock-card i { font-size: 50px; color: #00d1b2; margin-bottom: 20px; }
input { background: #25292e; border: none; padding: 12px; color: white; border-radius: 5px; margin: 10px 0; outline: none; }
button { background: #00d1b2; color: black; border: none; padding: 12px 25px; border-radius: 5px; cursor: pointer; font-weight: bold; }

.top-bar { background: #1a1d21; padding: 15px; display: flex; justify-content: space-between; border-bottom: 2px solid #00d1b2; }
#chat-feed { height: 70vh; overflow-y: auto; padding: 20px; display: flex; flex-direction: column; gap: 15px; }
.bubble { background: #232d36; padding: 12px; border-radius: 10px; border-left: 4px solid #00d1b2; width: fit-content; max-width: 80%; }
.tool-box { position: fixed; bottom: 0; width: 100%; display: flex; padding: 15px; background: #1a1d21; box-sizing: border-box; }
.btn-tool { background: none; color: #00d1b2; font-size: 24px; }


