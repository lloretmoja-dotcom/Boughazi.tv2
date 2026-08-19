<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Boughazi.tv - Your World of Entertainment</title>
    <!-- Tailwind CSS optimizado -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- HLS.js para reproducción ultrarrápida -->
    <script src="https://cdn.jsdelivr.net/npm/hls.js@latest"></script>
    <style>
        body { background-color: #060911; color: #f8fafc; font-family: system-ui, -apple-system, sans-serif; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
        #splash-screen {
            position: fixed; inset: 0; background: #060911; z-index: 9999;
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            transition: opacity 0.3s ease;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col">

    <!-- Pantalla de Carga Inicial Ultrarrápida -->
    <div id="splash-screen">
        <div class="flex flex-col items-center gap-3">
            <div class="bg-sky-500/20 border border-sky-500/40 p-4 rounded-3xl text-sky-400 shadow-2xl">
                <svg class="w-12 h-12" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                    <path d="M21 12a9 9 0 01-9 9m9-9a9 9 0 00-9-9m9 9H3m9 9a9 9 0 01-9-9m9 9c1.657 0 3-4.03 3-9s-1.343-9-3-9m0 18c-1.657 0-3-4.03-3-9s1.343-9 3-9m-9 9a9 9 0 019-9"></path>
                </svg>
            </div>
            <h1 class="text-2xl font-extrabold tracking-wider text-white">Boughazi<span class="text-sky-400 font-light">.tv</span></h1>
        </div>
    </div>

    <!-- Cabecera Oficial -->
    <header class="bg-slate-950 border-b border-slate-800 p-3 sticky top-0 z-50 shadow-xl">
        <div class="max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center gap-3">
            <div class="flex items-center gap-3">
                <div class="bg-sky-500/20 border border-sky-500/40 p-2 rounded-xl text-sky-400">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                        <path d="M21 12a9 9 0 01-9 9m9-9a9 9 0 00-9-9m9 9H3m9 9a9 9 0 01-9-9m9 9c1.657 0 3-4.03 3-9s-1.343-9-3-9m0 18c-1.657 0-3-4.03-3-9s1.343-9 3-9m-9 9a9 9 0 019-9"></path>
                    </svg>
                </div>
                <div>
                    <h1 class="text-lg font-extrabold text-white">Boughazi<span class="text-sky-400">.tv</span></h1>
                    <span class="text-[9px] text-slate-400 uppercase tracking-wider block">Entertainment & News</span>
                </div>
            </div>

            <div class="flex flex-wrap gap-2 w-full md:w-auto items-center">
                <input type="text" id="buscador" placeholder="Buscar canal..." 
                    class="bg-slate-900 border border-slate-800 rounded-xl px-3 py-1.5 text-xs w-full sm:w-48 text-white focus:outline-none focus:border-sky-500">
                
                <div id="authContainer">
                    <button onclick="abrirModalRegistro()" class="bg-white text-slate-900 px-3 py-1.5 rounded-xl text-xs font-semibold hover:bg-slate-200 transition">
                        Registrarse Gmail
                    </button>
                </div>
            </div>
        </div>
    </header>

    <!-- Contenido Principal -->
    <main class="max-w-7xl mx-auto flex-1 w-full p-3 grid grid-cols-1 lg:grid-cols-3 gap-4">
        
        <!-- Reproductor y Espacio de Publicidad Superior -->
        <div class="lg:col-span-2 flex flex-col gap-3">
            <div class="bg-black rounded-2xl overflow-hidden shadow-2xl aspect-video relative flex items-center justify-center border border-slate-800">
                <video id="videoPlayer" controls autoplay playsinline class="w-full h-full object-contain"></video>
            </div>
            
            <div class="bg-slate-900/80 p-3 rounded-2xl border border-slate-800 flex justify-between items-center">
                <div>
                    <h2 id="currentChannelTitle" class="text-sm font-bold text-white">Cargando canal...</h2>
                    <p id="currentChannelDesc" class="text-xs text-slate-400">Transmisión en directo optimizada.</p>
                </div>
                <span class="bg-red-500/20 text-red-400 text-[10px] px-2 py-0.5 rounded-full font-semibold animate-pulse">● LIVE</span>
            </div>

            <!-- ESPACIO DE PUBLICIDAD (Google AdMob Banner / Monetización) -->
            <div class="bg-slate-900/50 border border-dashed border-slate-800 rounded-2xl p-3 text-center">
                <span class="text-[10px] text-slate-500 uppercase tracking-widest block mb-1">Anuncio Patrocinado</span>
                <div class="bg-slate-950 rounded-xl py-2 px-4 text-xs text-slate-400 flex justify-between items-center">
                    <span>💡 Espacio publicitario optimizado para Google AdMob</span>
                    <span class="bg-sky-500/20 text-sky-400 text-[9px] px-2 py-0.5 rounded">Ads Free para Premium</span>
                </div>
            </div>
        </div>

        <!-- Cassette de Canales Rápido -->
        <div class="bg-slate-900/80 rounded-2xl border border-slate-800 p-3 flex flex-col h-[400px] lg:h-[calc(100vh-160px)]">
            <h3 class="text-[10px] font-bold text-slate-400 uppercase tracking-widest mb-2">Cassette de Canales</h3>
            <div id="channelList" class="overflow-y-auto flex-1 flex flex-col gap-1.5 pr-1"></div>
        </div>
    </main>

    <!-- Modal de Registro -->
    <div id="registroModal" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-[100] flex items-center justify-center hidden p-4">
        <div class="bg-slate-900 border border-slate-800 rounded-2xl p-5 w-full max-w-sm shadow-2xl">
            <h3 class="text-base font-bold text-white mb-1">Registro de Usuario</h3>
            <p class="text-xs text-slate-400 mb-3">Guarda tus preferencias en Boughazi.tv</p>
            <div class="flex flex-col gap-2.5">
                <input type="text" id="inputNombre" placeholder="Tu Nombre" class="bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-xs text-white">
                <input type="email" id="inputGmail" placeholder="tuusuario@gmail.com" class="bg-slate-950 border border-slate-800 rounded-xl px-3 py-2 text-xs text-white">
            </div>
            <div class="flex justify-end gap-2 mt-4">
                <button onclick="cerrarModalRegistro()" class="px-3 py-1.5 rounded-xl bg-slate-800 text-slate-300 text-xs">Cancelar</button>
                <button onclick="guardarRegistro()" class="px-4 py-1.5 rounded-xl bg-sky-600 text-white text-xs font-semibold">Guardar</button>
            </div>
        </div>
    </div>

    <script>
        window.addEventListener('load', () => {
            setTimeout(() => document.getElementById('splash-screen').remove(), 400);
        });

        // Canales por defecto rápidos y sincronizados
        let channels = JSON.parse(localStorage.getItem('boughazi_channels')) || [
            { name: "Al Jazeera English", country: "🇶🇦 Catar", url: "https://live-hls-web-aje.getaj.net/AJE/index.m3u8", desc: "Noticias globales." },
            { name: "Al Aoula Inter", country: "🇲🇦 Marruecos", url: "https://snrt-live.gcdn.co/la1/index.m3u8", desc: "Canal público." }
        ];

        let hls = null;
        const videoPlayer = document.getElementById('videoPlayer');
        const channelListContainer = document.getElementById('channelList');

        function abrirModalRegistro() { document.getElementById('registroModal').classList.remove('hidden'); }
        function cerrarModalRegistro() { document.getElementById('registroModal').classList.add('hidden'); }

        function guardarRegistro() {
            const nombre = document.getElementById('inputNombre').value.trim();
            const gmail = document.getElementById('inputGmail').value.trim();
            if (!nombre || !gmail.includes("@gmail.com")) { alert("Introduce datos válidos."); return; }
            
            let usuarios = JSON.parse(localStorage.getItem('boughazi_usuarios')) || [];
            usuarios.push({ nombre, gmail, fecha: new Date().toLocaleString() });
            localStorage.setItem('boughazi_usuarios', JSON.stringify(usuarios));

            cerrarModalRegistro();
            alert(`¡Registro completado con éxito, ${nombre}!`);
        }

        function renderChannels() {
            channelListContainer.innerHTML = "";
            channels.forEach(channel => {
                const item = document.createElement('div');
                item.className = "flex items-center justify-between p-2.5 rounded-xl bg-slate-950 hover:bg-slate-800 cursor-pointer transition border border-slate-800/60";
                item.innerHTML = `<div><h4 class='font-semibold text-xs text-white'>${channel.name}</h4><span class='text-[10px] text-sky-400'>${channel.country}</span></div>`;
                item.onclick = () => playChannel(channel);
                channelListContainer.appendChild(item);
            });
        }

        function playChannel(channel) {
            document.getElementById('currentChannelTitle').textContent = channel.name;
            document.getElementById('currentChannelDesc').textContent = channel.desc;
            if (Hls.isSupported()) {
                if (hls) hls.destroy();
                hls = new Hls({ maxBufferLength: 30, liveSyncDurationCount: 3 });
                hls.loadSource(channel.url);
                hls.attachMedia(videoPlayer);
                hls.on(Hls.Events.MANIFEST_PARSED, () => videoPlayer.play().catch(()=>{}));
            }
        }

        renderChannels();
        if(channels.length > 0) playChannel(channels[0]);
    </script>
</body>
</html>
