<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Happy Birthday Gaayeee!</title>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <style>
        :root {
            --pink: #ff4d6d;
            --dark-pink: #c9184a;
            --bg-pink: #fff0f3;
        }

        body {
            margin: 0;
            padding: 0;
            overflow: hidden;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: var(--bg-pink);
            height: 100vh;
            position: fixed; 
            width: 100%;
            transition: background 1s ease-in-out;
        }

        /* Background class triggered after unlock */
        .birthday-bg {
            /* REPLACE 'YOUR_IMAGE_URL_HERE' with the link to the photo you uploaded */
            background-image: url('YOUR_IMAGE_URL_HERE');
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
        }

        /* --- Lock Screen --- */
        #lock-screen {
            position: fixed;
            inset: 0;
            background: white;
            z-index: 500;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

        input { padding: 10px; font-size: 18px; border: 2px solid var(--pink); border-radius: 8px; outline: none; margin-bottom: 10px; text-align: center; }
        button { padding: 10px 20px; background: var(--pink); color: white; border: none; border-radius: 8px; cursor: pointer; font-size: 16px; }

        /* --- Main Content --- */
        #main-container { display: none; height: 100%; width: 100%; position: relative; z-index: 10; }

        #ui-layer {
            position: absolute;
            top: 10%;
            width: 100%;
            text-align: center;
            z-index: 20;
            pointer-events: none;
        }

        h1 { 
            color: white; 
            font-size: 2rem; 
            margin: 0; 
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5); /* Added for visibility against bg */
        }
        
        #timer {
            font-size: 1.2rem;
            font-weight: bold;
            color: white;
            background: rgba(201, 24, 74, 0.6); /* Semi-transparent dark pink */
            backdrop-filter: blur(5px);
            padding: 10px 20px;
            border-radius: 15px;
            display: inline-block;
            margin-top: 10px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
        }

        .emoji { position: absolute; font-size: 2.5rem; user-select: none; pointer-events: none; will-change: transform; z-index: 1; }

        /* --- Floating Boxes --- */
        .floating-box {
            position: absolute;
            cursor: pointer;
            z-index: 50;
            text-align: center;
            width: 90px;
            height: 90px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            touch-action: none;
        }
        .icon-circle { 
            font-size: 3.5rem; 
            filter: drop-shadow(0 5px 10px rgba(0,0,0,0.3)); 
        }
        .box-label { 
            color: white; 
            font-size: 0.6rem; 
            font-weight: bold; 
            background: var(--dark-pink); 
            border-radius: 4px; 
            padding: 2px 8px; 
            margin-top: -5px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }

        /* --- Modals --- */
        .overlay {
            display: none;
            position: fixed;
            inset: 0;
            background: rgba(0,0,0,0.85);
            z-index: 400;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .modal-box {
            background: #fff;
            width: 100%;
            max-width: 450px;
            border-radius: 15px;
            position: relative;
            border: 4px solid var(--pink);
            overflow: hidden;
        }

        .close-btn { position: absolute; top: 10px; right: 15px; font-size: 2rem; color: var(--dark-pink); cursor: pointer; z-index: 10; }

        /* Confession Specific */
        #confession-box { padding: 25px; max-height: 70vh; overflow-y: auto; }
        .confession-text { white-space: pre-wrap; font-family: 'Georgia', serif; line-height: 1.6; color: #333; }

        /* Photo Slider Specific */
        .slider-container { width: 100%; position: relative; }
        .slide { display: none; text-align: center; }
        .slide img { width: 100%; height: 350px; object-fit: cover; }
        .slide-desc { padding: 15px; color: #333; font-style: italic; background: white; min-height: 50px; }
        .nav-btn {
            position: absolute; top: 45%; transform: translateY(-50%);
            background: var(--pink); color: white; border: none; padding: 12px; border-radius: 50%; cursor: pointer;
            box-shadow: 0 2px 10px rgba(0,0,0,0.3);
        }
        #prevBtn { left: 10px; }
        #nextBtn { right: 10px; }
    </style>
</head>
<body>

    <div id="lock-screen">
        <h2 style="color: var(--dark-pink)">Enter Password ❤️</h2>
        <input type="password" id="pass" placeholder="👀👀👀">
        <button onclick="unlock()">Unlock!</button>
    </div>

    <div id="main-container">
        <div id="ui-layer">
            <h1>Happy Birthday, Gaayee! 🎂</h1>
            <div id="timer">Loading timer...</div>
        </div>

        <div id="letter-box" class="floating-box" onclick="openModal('confession-overlay')">
            <div class="icon-circle">💌</div>
            <div class="box-label">LETTER</div>
        </div>

        <div id="photo-box" class="floating-box" onclick="openModal('photo-overlay')">
            <div class="icon-circle">📸</div>
            <div class="box-label">MEMORIES</div>
        </div>
    </div>

    <div id="confession-overlay" class="overlay">
        <div class="modal-box" id="confession-box">
            <span class="close-btn" onclick="closeModal('confession-overlay')">&times;</span>
            <div class="confession-text">
Gaayeee.....
I still remember the day I saw u, October 6th 2024. naa life lo jarigina the biggest moment.
Naaku adi still oka dream la undi, charan gadu velli evaranna ammayi id kanukkundam ani anadam enti, vadu ninnu choodadam enti ninnu adagadam enti.
Ah roju ninnu ah dress lo chala bavunnav. Ninna jariginatte undi edi antha but adi ayyi 1 year 4 months avtundi.
Vadu entha try chesina nuvvu kadalaledu annappude nee meda chala respect perigindi. Adi oka reason neeku txt cheyyadaniki.
charan gadu nannu challenge chesadu edi inko reason. & ah challenge tesukoni manchi pani chesa.
Ah challenge valla naa life lo the lowest phase lo naaku oka thodu doorikindi. naa future neetho unte bavundu ani anipinchindi.
...
(Full text here)
...
ILY sooo sooo soooo much!!!!!
Mwah!!!
Happy Birthday My Love!
~ Nee Teeeeeeeennnuuuuuuuuu
            </div>
        </div>
    </div>

    <div id="photo-overlay" class="overlay">
        <div class="modal-box">
            <span class="close-btn" onclick="closeModal('photo-overlay')">&times;</span>
            <div class="slider-container" id="slider">
                <button id="prevBtn" class="nav-btn" onclick="changeSlide(-1)">❮</button>
                <button id="nextBtn" class="nav-btn" onclick="changeSlide(1)">❯</button>
            </div>
        </div>
    </div>

    <script>
        // ADD YOUR PHOTOS HERE
        const photoData = [
            { url: "WhatsApp Image 2026-02-13 at 3.11.21 PM.jpeg", desc: "ILYSM❤️" },
            { url: "WhatsApp Image 2026-02-13 at 3.42.32 PM.jpeg", desc: "Mana 1st Spot lo...😂❤️" },
            { url: "WhatsApp Image 2026-02-13 at 3.20.04 PM.jpeg", desc: "Naaku nuvvu ala ready avtunnappudu choostaa undadam istam👀" },
            { url: "WhatsApp Image 2026-02-13 at 3.13.53 PM.jpeg", desc: "Neetho chala memories unnayi but we didn't bother capturing, eppati nunchi anna capture cheddam👀" },
            { url: "WhatsApp Image 2026-02-13 at 3.21.03 PM.jpeg", desc: "Happy Birthday Love❤️" },
        ];

        const targetDate = new Date("February 15, 2026 00:00:00").getTime();
        let emojis = [], currentSlide = 0;
        let motionPermission = false;
        let gravityX = 0, gravityY = 0.5;

        let boxes = [
            { id: 'letter-box', x: 50, y: 350, vx: 2, vy: 2, el: null },
            { id: 'photo-box', x: 200, y: 450, vx: -2, vy: 1.5, el: null }
        ];

        function unlock() {
            if (document.getElementById('pass').value.toUpperCase() === "ILY") {
                document.getElementById('lock-screen').style.display = 'none';
                document.getElementById('main-container').style.display = 'block';
                
                // Set the background image to the body
                document.body.classList.add('birthday-bg');
                
                boxes.forEach(b => b.el = document.getElementById(b.id));
                initSlider();
                startCelebration();
                requestMotion();
            } else { alert("Wrong password!"); }
        }

        function requestMotion() {
            if (typeof DeviceOrientationEvent.requestPermission === 'function') {
                DeviceOrientationEvent.requestPermission().then(s => {
                    if (s === 'granted') { motionPermission = true; window.addEventListener('deviceorientation', e => { gravityX = e.gamma/15; gravityY = e.beta/15; }); }
                });
            } else { motionPermission = true; window.addEventListener('deviceorientation', e => { gravityX = e.gamma/15; gravityY = e.beta/15; }); }
        }

        function initSlider() {
            const slider = document.getElementById('slider');
            photoData.forEach((item) => {
                let div = document.createElement('div');
                div.className = 'slide';
                div.innerHTML = `<img src="${item.url}"><div class="slide-desc">${item.desc}</div>`;
                slider.appendChild(div);
            });
            showSlide(0);
        }

        function showSlide(n) {
            let slides = document.getElementsByClassName('slide');
            if (n >= slides.length) currentSlide = 0;
            if (n < 0) currentSlide = slides.length - 1;
            for (let i = 0; i < slides.length; i++) slides[i].style.display = "none";
            slides[currentSlide].style.display = "block";
        }

        function changeSlide(n) { showSlide(currentSlide += n); }
        function openModal(id) { document.getElementById(id).style.display = 'flex'; }
        function closeModal(id) { document.getElementById(id).style.display = 'none'; }

        function startCelebration() {
            confetti({ particleCount: 150, spread: 70, origin: { y: 0.6 } });
            createEmojis();
            setInterval(updateTimer, 1000);
            animate();
        }

        function updateTimer() {
            const diff = targetDate - new Date().getTime();
            if (diff <= 0) { document.getElementById('timer').innerHTML = "HAPPY BIRTHDAY GAAYEE! ❤️"; return; }
            const d = Math.floor(diff / 86400000), h = Math.floor((diff % 86400000) / 3600000), m = Math.floor((diff % 3600000) / 60000), s = Math.floor((diff % 60000) / 1000);
            document.getElementById('timer').innerHTML = `${d}d ${h}h ${m}m ${s}s`;
        }

        function createEmojis() {
            const types = ['😘', '❤️', '🌹', '💖', '🥰'];
            for (let i = 0; i < 500; i++) {
                let el = document.createElement('div'); el.className = 'emoji'; el.innerHTML = types[i % 5];
                document.getElementById('main-container').appendChild(el);
                emojis.push({ el, x: Math.random()*window.innerWidth, y: Math.random()*window.innerHeight, vx: (Math.random()-0.5)*3, vy: (Math.random()-0.5)*3 });
            }
        }

        function animate() {
            if (!motionPermission) { gravityX = Math.sin(Date.now()/1000)*0.2; gravityY = 0.5; }

            boxes.forEach(b => {
                b.vx += gravityX * 0.05; b.vy += gravityY * 0.05;
                b.x += b.vx; b.y += b.vy; b.vx *= 0.99; b.vy *= 0.99;
                if (b.x < 0 || b.x > window.innerWidth - 90) b.vx *= -0.8;
                if (b.y < 0 || b.y > window.innerHeight - 90) b.vy *= -0.8;
                b.el.style.transform = `translate(${b.x}px, ${b.y}px)`;
            });

            emojis.forEach(e => {
                e.vx += gravityX * 0.1; e.vy += gravityY * 0.1;
                e.x += e.vx; e.y += e.vy; e.vx *= 0.98; e.vy *= 0.98;
                if (e.x < 0 || e.x > window.innerWidth - 40) e.vx *= -0.7;
                if (e.y < 0 || e.y > window.innerHeight - 40) e.vy *= -0.7;
                e.el.style.transform = `translate(${e.x}px, ${e.y}px)`;
            });
            requestAnimationFrame(animate);
        }
    </script>
</body>
</html>
