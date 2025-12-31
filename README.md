<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gak Romantis - Deep Space Journey</title>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Poppins:wght@300;600;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-color: #000000;
            --accent: #00d2ff;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            background-color: var(--bg-color);
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Poppins', sans-serif;
            overflow: hidden;
        }

        canvas {
            position: fixed;
            top: 0; left: 0;
            width: 100%; height: 100%;
            z-index: 1;
            opacity: 0;
            transition: opacity 2s ease;
        }

        .envelope-container {
            position: relative;
            z-index: 10;
            width: 90vw; max-width: 500px; height: 320px;
            transition: all 1.2s ease;
        }

        .paper {
            position: absolute; left: 5%; bottom: 15px; width: 90%; height: 300px;
            background: #ffffff; padding: 35px; border-radius: 12px;
            z-index: 2; transition: transform 1s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            text-align: center; display: flex; flex-direction: column; justify-content: center;
        }

        .envelope { position: absolute; width: 100%; height: 100%; background: #1e293b; z-index: 1; border-radius: 0 0 20px 20px; box-shadow: 0 30px 80px rgba(0,0,0,1); }
        
        .flap { 
            position: absolute; top: 0; width: 100%; height: 100%; 
            border-left: 45vw solid transparent; 
            border-right: 45vw solid transparent; 
            border-top: 170px solid #334155; 
            transform-origin: top; transition: 0.8s; z-index: 4; 
        }
        @media (min-width: 500px) {
            .flap { border-left: 250px solid transparent; border-right: 250px solid transparent; }
        }

        .pocket { 
            position: absolute; top: 0; width: 100%; height: 100%; 
            border-left: 45vw solid transparent; 
            border-right: 45vw solid transparent; 
            border-bottom: 170px solid #1e293b; 
            z-index: 3; border-radius: 0 0 20px 20px; 
        }
        @media (min-width: 500px) {
            .pocket { border-left: 250px solid transparent; border-right: 250px solid transparent; }
        }

        .btn-start { position: absolute; z-index: 100; background: linear-gradient(45deg, #00d2ff, #3a7bd5); color: white; padding: 18px 45px; border: none; border-radius: 40px; font-weight: bold; cursor: pointer; box-shadow: 0 10px 20px rgba(0, 210, 255, 0.4); }

        .open .flap { transform: rotateX(180deg); z-index: 0; }
        
        /* Posisi kertas tetap rendah agar teks tidak terpotong */
        .open .paper { transform: translateY(-20px); height: 420px; z-index: 5; }

        #nextBtn { padding: 12px 30px; cursor: pointer; background: #1e293b; color: white; border: none; border-radius: 30px; margin-top: 20px; font-weight: bold; }
        .hidden { display: none !important; }
        .fade-out { opacity: 0; transform: scale(0.1) translateY(-800px); pointer-events: none; }
    </style>
</head>
<body>

    <canvas id="galaxyCanvas"></canvas>

    <audio id="myAudio">
        <source src="ga romantis.mp3" type="audio/mpeg">
    </audio>

    <button class="btn-start" id="btnStart" onclick="startApp()">Buka Surat</button>

    <div class="envelope-container" id="mainEnv">
        <div class="flap"></div>
        <div class="pocket"></div>
        <div class="paper" id="paper">
            <h2 style="font-family: 'Dancing Script'; font-size: 2.5rem;">Hello!</h2>
            <p style="margin-top: 15px; color: #475569;">Ada pesan singkat untukmu...</p>
            <div id="tag" class="hidden" style="color:#00d2ff; font-weight:bold; margin-top: 20px;">@HiLibra</div>
            <button id="nextBtn" class="hidden" onclick="changeMsg()">Lanjut →</button>
        </div>
        <div class="envelope"></div>
    </div>

    <script>
        const lyricsData = [
            { time: 0.0, text: "Bila kau marah..." },
            { time: 3.0, text: "Janganlah engkau pergi" },
            { time: 6.6, text: "Tetaplah di sini..." },
            { time: 9.6, text: "Dengarkan 'ku bernyanyi" },
            { time: 13.9, text: "Aku ga mau jadi mataharimu" },
            { time: 18.6, text: "Karena itu akan membuatku jauh" },
            { time: 23.0, text: "Aku ga mau jadi bintang-bintangmu" },
            { time: 27.5, text: "Walau indah itu juga 'kan jauh" },
            { time: 32.1, text: "Yang aku mau menjadi udaramu" },
            { time: 36.7, text: "Selalu setia tiap hela nafasmu" },
            { time: 40.9, text: "Aku ga romantis..." },
            { time: 43.7, text: "Maaf, aku ga romantis... 😊" },
            { time: 45.9, text: "Maaf, aku ga romantis..." },
            { time: 48.2, text: "Aku ga romantis... ✨" }
        ];

        let step = 1;
        const audio = document.getElementById("myAudio");

        function startApp() {
            audio.play();
            document.getElementById('btnStart').classList.add('hidden');
            document.getElementById('mainEnv').classList.add('open');
            setTimeout(() => document.getElementById('nextBtn').classList.remove('hidden'), 1000);
        }

        function changeMsg() {
            const paper = document.getElementById('paper');
            step++;
            if(step === 2) {
                // Kalimat penyemangat yang lebih santai dan tidak berlebihan
                paper.innerHTML = `<h2 style="font-family: 'Dancing Script'; font-size: 2.5rem;">For you...</h2><p style="margin-top: 15px; color: #475569;">Semangat Ya Kerjanya...!</p><button id="nextBtn" onclick="changeMsg()">Lanjut →</button>`;
            } else if(step === 3) {
                // Kalimat penyemangat tambahan yang santai
                paper.innerHTML = `<h2 style="font-family: 'Dancing Script'; font-size: 2.5rem;">Semangat!</h2><p style="margin-top: 15px; color: #475569;">Kalau belum sukses, berarti bangunannya masih tahap bekisting.</p><div style="color:#00d2ff; font-weight:bold; margin-top: 20px;">@HiLibra</div><button id="nextBtn" onclick="changeMsg()">Masuk Galaksi ✨</button>`;
            } else {
                showGalaxy();
            }
        }

        function showGalaxy() {
            document.getElementById('mainEnv').classList.add('fade-out');
            const canvas = document.getElementById('galaxyCanvas');
            canvas.style.opacity = '1';
            initGalaxy();
        }

        function initGalaxy() {
            const canvas = document.getElementById('galaxyCanvas');
            const ctx = canvas.getContext('2d');
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;

            let stars = [];
            let currentLyric = { text: "", z: 0, opacity: 0, active: false };
            let lastLyricIndex = -1;

            for(let i=0; i<700; i++) {
                let hue = 185 + Math.random() * 35; 
                stars.push({
                    x: Math.random() * canvas.width - canvas.width/2,
                    y: Math.random() * canvas.height - canvas.height/2,
                    z: Math.random() * canvas.width,
                    color: `hsl(${hue}, 100%, 75%)` 
                });
            }

            function draw() {
                ctx.fillStyle = 'rgba(0, 0, 0, 0.2)';
                ctx.fillRect(0, 0, canvas.width, canvas.height);

                stars.forEach(s => {
                    s.z -= 8; 
                    if(s.z <= 0) s.z = canvas.width;
                    let sx = (s.x / s.z) * canvas.width + canvas.width/2;
                    let sy = (s.y / s.z) * canvas.height + canvas.height/2;
                    let r = (1 - s.z/canvas.width) * 5;
                    
                    ctx.shadowBlur = 10;
                    ctx.shadowColor = s.color;
                    ctx.fillStyle = s.color;
                    ctx.beginPath(); ctx.arc(sx, sy, r, 0, Math.PI*2); ctx.fill();
                    ctx.shadowBlur = 0; 
                });

                const currentTime = audio.currentTime;
                let index = -1;
                for (let i = 0; i < lyricsData.length; i++) {
                    if (currentTime >= lyricsData[i].time) index = i;
                }

                if (index !== lastLyricIndex && index !== -1) {
                    lastLyricIndex = index;
                    currentLyric = {
                        text: lyricsData[index].text,
                        z: canvas.width,
                        opacity: 0,
                        active: true
                    };
                }

                if (currentLyric.active) {
                    currentLyric.z -= 7; 
                    
                    if (currentLyric.z > canvas.width * 0.8) currentLyric.opacity += 0.06;
                    if (currentLyric.z < canvas.width * 0.2) currentLyric.opacity -= 0.06;

                    if (currentLyric.z <= 0) {
                        currentLyric.active = false;
                    } else {
                        ctx.save();
                        let size = (1 - currentLyric.z/canvas.width) * 70 + 20;
                        ctx.font = `800 ${size}px 'Poppins'`; 
                        ctx.textAlign = "center";
                        ctx.fillStyle = `rgba(255, 255, 255, ${Math.max(0, currentLyric.opacity)})`;
                        ctx.shadowBlur = 20;
                        ctx.shadowColor = "rgba(0, 210, 255, 0.9)"; 
                        ctx.fillText(currentLyric.text, canvas.width / 2, canvas.height / 2);
                        ctx.restore();
                    }
                }

                requestAnimationFrame(draw);
            }
            draw();
        }

        window.addEventListener('resize', () => {
            const canvas = document.getElementById('galaxyCanvas');
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        });
    </script>
</body>
</html>
