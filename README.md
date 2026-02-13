<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Happy Birthday Gaayeee!</title>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <style>
        :root { --pink: #ff4d6d; --dark-pink: #c9184a; --bg-pink: #fff0f3; }

        body {
            margin: 0; padding: 0; overflow: hidden;
            font-family: 'SF Pro Display', -apple-system, sans-serif;
            background: var(--bg-pink); height: 100vh; width: 100vw; position: fixed; 
        }

        .birthday-bg {
            background-image: linear-gradient(rgba(0,0,0,0.3), rgba(0,0,0,0.3)), url('YOUR_BACKGROUND_IMAGE_URL');
            background-size: cover; background-position: center; background-repeat: no-repeat;
        }

        /* --- Lock Screen --- */
        #lock-screen {
            position: fixed; inset: 0; background: white; z-index: 9999;
            display: flex; flex-direction: column; justify-content: center; align-items: center;
        }
        input { padding: 12px; font-size: 16px; border: 2px solid var(--pink); border-radius: 12px; margin-bottom: 15px; text-align: center; outline: none; }
        #lock-screen button { padding: 12px 30px; background: var(--pink); color: white; border: none; border-radius: 12px; font-weight: bold; }

        /* --- Main UI --- */
        #main-container { display: none; height: 100%; width: 100%; position: relative; }
        #ui-layer { position: absolute; top: 5%; width: 100%; text-align: center; z-index: 20; pointer-events: none; }
        h1 { 
            color: #fff; font-size: 1.8rem; margin: 0; text-transform: uppercase; font-weight: 900;
            text-shadow: 2px 2px 0 var(--dark-pink), -2px -2px 0 var(--dark-pink), 2px -2px 0 var(--dark-pink), -2px 2px 0 var(--dark-pink), 0 5px 15px rgba(0,0,0,0.5);
        }

        /* --- Boxes & Emojis --- */
        .emoji { position: absolute; font-size: 2rem; user-select: none; pointer-events: none; will-change: transform; z-index: 1; opacity: 0.8; }
        .floating-box {
            position: absolute; cursor: pointer; z-index: 100;
            width: 80px; height: 80px; display: flex; flex-direction: column;
            justify-content: center; align-items: center; touch-action: none;
            transition: transform 0.1s ease-out;
        }
        .icon-circle { font-size: 2.8rem; filter: drop-shadow(0 4px 8px rgba(0,0,0,0.4)); }
        .box-label { 
            color: white; font-size: 0.55rem; font-weight: bold; background: var(--dark-pink); 
            border-radius: 6px; padding: 2px 5px; margin-top: -3px; box-shadow: 0 4px 10px rgba(0,0,0,0.3);
            white-space: nowrap; text-transform: uppercase;
        }

        /* --- Modals --- */
        .overlay {
            display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.92);
            z-index: 5000; justify-content: center; align-items: center; padding: 15px;
        }
        .modal-box {
            background: #fff; width: 95%; max-width: 380px; border-radius: 24px;
            position: relative; border: 4px solid var(--pink); overflow: hidden; text-align: center;
            animation: pop 0.3s ease-out;
        }
        @keyframes pop { from { transform: scale(0.8); } to { transform: scale(1); } }
        .close-btn { position: absolute; top: 10px; right: 15px; font-size: 2.5rem; color: var(--dark-pink); cursor: pointer; z-index: 6000; line-height: 1; }
        .modal-content { padding: 35px 20px; }
        .big-pink-text { font-weight: 900; font-size: 1.8rem; color: var(--pink); line-height: 1.3; text-transform: uppercase; }

        /* --- Memories Slider --- */
        .slider-wrapper { position: relative; width: 100%; background: #000; min-height: 250px; }
        .slide { display: none; width: 100%; }
        .slide img, .slide video { width: 100%; height: auto; max-height: 55vh; object-fit: contain; display: block; }
        .slide-desc { padding: 15px; background: #fff; color: #333; font-style: italic; font-size: 0.9rem; }
        .nav-btn {
            position: absolute; top: 50%; transform: translateY(-50%);
            background: var(--pink); color: #fff; border: none; width: 40px; height: 40px; border-radius: 50%; font-size: 1.2rem;
        }

        /* --- Specialized Styles --- */
        #love-percent { font-size: 3rem; font-weight: 900; color: var(--dark-pink); display: block; margin: 10px; }
        .flash-emergency { animation: flash 0.4s infinite; }
        @keyframes flash { 0% { background: #fff; } 50% { background: #ffccd5; } 100% { background: #fff; } }
        .coupon-box { text-align: left; background: #fff0f3; padding: 15px; border-radius: 15px; border: 2px dashed var(--pink); margin-top: 10px; }
    </style>
</head>
<body>

    <div id="lock-screen">
        <h2 style="color: var(--dark-pink); font-family: serif;">For My Love... ❤️</h2>
        <input type="password" id="pass" placeholder="👀👀👀">
        <button onclick="unlock()">OPEN MY HEART</button>
    </div>

    <div id="main-container">
        <div id="ui-layer"><h1>Happy Birthday, Gayee!🎂❤️✨</h1></div>

        <div id="letter-box" class="floating-box" onclick="openModal('letter-modal')">
            <div class="icon-circle">💌</div><div class="box-label">LETTER</div>
        </div>
        <div id="photo-box" class="floating-box" onclick="openModal('photo-modal')">
            <div class="icon-circle">📸</div><div class="box-label">MEMORIES</div>
        </div>
        <div id="pickup-box" class="floating-box" onclick="showPickup()">
            <div class="icon-circle">✨</div><div class="box-label">PICKUP LINES</div>
        </div>
        <div id="mood-box" class="floating-box" onclick="openModal('mood-modal')">
            <div class="icon-circle">🎭</div><div id="mood-label" class="box-label">CUTE</div>
        </div>
        <div id="secret-box" class="floating-box" onmouseover="runAway(this)" ontouchstart="runAway(this)" onclick="openModal('secret-modal')">
            <div class="icon-circle">🤫</div><div class="box-label">CATCH ME</div>
        </div>
        <div id="gift-box" class="floating-box" onclick="openModal('gift-modal')">
            <div class="icon-circle">🎁</div><div class="box-label">GIFT COUPONS</div>
        </div>
        <div id="mirror-box" class="floating-box" onclick="openModal('mirror-modal')">
            <div class="icon-circle">🪞</div><div class="box-label">MAGIC MIRROR</div>
        </div>
        <div id="love-box" class="floating-box" onclick="startLoveCounter()">
            <div class="icon-circle">🏋️‍♀️</div><div class="box-label">LOVE %</div>
        </div>
        <div id="void-box" class="floating-box" onclick="openModal('void-modal')">
            <div class="icon-circle">🕳️</div><div class="box-label">THE VOID</div>
        </div>
        <div id="alarm-box" class="floating-box" onclick="openModal('alarm-modal')">
            <div class="icon-circle">🚨</div><div class="box-label">EMERGENCY</div>
        </div>
    </div>

    <div id="letter-modal" class="overlay"><div class="modal-box"><span class="close-btn" onclick="closeModal('letter-modal')">&times;</span><div class="modal-content" style="text-align:left; font-family:serif; overflow-y:auto; max-height:60vh;">Gaayeee...<br><br>Gaayeee.....
I still remember the day I saw u, October 6th 2024. naa life lo jarigina the biggest moment.<br>
Naaku adi still oka dream la undi, charan gadu velli evaranna ammayi id kanukkundam ani anadam enti, vadu ninnu choodadam enti ninnu adagadam enti.<br>
Ah roju ninnu ah dress lo chala bavunnav. Ninna jariginatte undi edi antha but adi ayyi 1 year 4 months avtundi.<br>
Vadu entha try chesina nuvvu kadalaledu annappude nee meda chala respect perigindi. Adi oka reason neeku txt cheyyadaniki.<br>
Charan gadu nannu challenge chesadu edi inko reason. & ah challenge tesukoni manchi pani chesa.<br>
One of the best thing i ever did in my life was txting u.<br>
Ah challenge valla naa life lo the lowest phase lo naaku oka thodu doorikindi. naa future neetho unte bavundu ani anipinchindi.<br>
Nenu chala provoke chesa ninnu padeydaniki but nuvvu chala gattiganee nilabaddav…<br>
Chala close ayyam.nuvvu naaku ah time lo echhe priority naaku eppati varaku evvaru evvaledu. Istam perigindi nee meda.<br>
Nannu kalavadaniki clg lo nuvvu pade thippalu    asaluuu entha efforts pettavo. Oka side nunchi bhayapaduta oka side nunchi naatho matladutaa cute cute cute....<br>
I still remember nenu kanisam eye contact kuda evvalekapoyaa ninnu kalisinappudu. nenu okasari gattiga try chesa eye contact ki & ahhh your brown eyes...🤌🏼🤌🏼<br>
Evaduu undagaladu nee eyes ni choosi padakunda unde vadu. Nenu kuda vatiki padipoya.<br>
Kani nenu propose cheyyanu, adi naa ego kadu actually naaku antha easy ga propose cheyyali ani anipinchadu it should be like a celebration. Anduke cheyyaledu.<br>
Appatikee nenu confusion lo unna is it love or attraction ani. Manam chala close ayipoyamu appatiki. Frequent ga kalavadam start ayyindi.<br>
We know that we had feelings for each other. but bayatapadaledu anthe. It's one of the cutest phase we been through.<br>
Inka terrace meda kalisam matladukunnam matladukunnam everything seems good. But god had different plan ig.<br>
Appatiki inkaa chala close ayyamu & kavvinchukovadam start chesukunnam nuvvu undalevu ala cheyyalevu adi edi ani.Nenu anukunna that nenu chala control lo undagalanu ani. But deep inside ik that I cant control myself if I'm close to u.<br> 
September 18th 2024 magical day. Edi matram pakka god's plan. ah roju okarini okaram kavvinchukoni idk how it happened eppatiki kuda neneena ala chesindi ani anukunta eppatiki. Rainy season, cold weather moham lo moham pettukoni unnam evaru first move chestaraa ani. lol. Full ga tempt ayyi intiki vella ah roju. Emo edo teliani nervousness, Happiness.<br>
The same repeated the next day. Ik I cant control but nenu aagalekapoya it was the best magical moment in my life. Straight out of movie scene la untadi.<br>
Terrace meda, Rain lo, Solar panels kinda, okarini okaram choosukuntaa unnam. Theera choste oka hand tho Umbrella pattukoni inko hand tho nee waist ni grab chesi I pulled u close. Chala time moham lo moham pettukonee unnam evaru first move chestaraa ani. Deep down we know we need the kiss. & we did kiss.... Naa life lo antha romantic moment inkedi ledu. The best moment of my life adi.<br>
Time passed by... Manam matram inka inkaa close avtanee unnam. Elanti godavalu ochhina, Evadu madya lo ochhi provoke chesina we stood together.<br>
Nannu entha la chadivesav ante asaluuu… u know everything about me. <br>
Nannu nannu la choostav, chala prema choopistav, chala ardam chesukuntav, nannu bharistav.<br>
Naa life lo ki nuvvu ochhaka naaku chala peace ochhindi. Ofc drama kuda ochhindi but I love that ah matram undali leee appude life life la untadi.<br>
Nee lanti ammayi naaku dorikindi ante naaku inka nammabuddi kavatledu. I'm soo lucky to have u in my life.<br>
Nannu pichhi ga premistav, Nannu baga choosukunta ani antav naaku inkem kavali naaku. Nuvvu thappa.<br>
Neetho future anesukunna. Inka neethonee, nuvvu kuda fix ayipo.<br>
Naaku nuvvu thappa inka evvaru oddu. Naaku nuvvu chala ekkuva.<br>
Ykw nenu entha happy feel avtano telusa when my friends who know u say like, nee gayee andari la kadu ra friends tho unnappudu time istadi, steady ga undi ardam chesukuntadi, lucky ra babu nuvvu, maa vallu unnaru asal oooo gola chestaru ardam chesukokunda ani.<br>
Nenu chala lucky Bangaram.<br>
Nee la padhati ga unde ammayi ni, manchi ga unde ammayi ni, asal neeku unna qualities cheppukunta pote time saripodu.<br>
U r wife material! Naaku naa pillala ki amma ela ayite undali ani anukunnano anni qualities neelo unnayi.<br>
Nuvvu naa name ni nee name venaka add chesukunte andariki naa meda unna respect perugutadi because nee manchi theeru & your good character. Anthaki minchi oka abbayi ki inkem kavali.<br>
Ninnu badha pettakuda. Bangaram la choosukovali. Neeku eyy lotu raakunda choosukovali.<br>
Mana pair chala bavuntadi gaayee. U & Me cute cute things chestaa kalisi happy ga untee chala bavuntadiii.<br>
We are cute & ofc hot too!!.<br>
At last I feel like I found someone who loves me by the way I am. Someone who deserves all of my love.<br>
Nenu eppudu oka person ni intha trust cheyyaledu. Nenu intha adore kuda cheyyaledu. Nenu usual ga ammaylani antha trust cheyyanu kanipettukoni unta, nee vishyam lo kuda anthe unta but evvari meda leni trust nee meda undi, like nuvvu nee limits lo ne untav anna nammakam naaku undi. Nuvvu naa danivi anna feel nuvvu nalo teppinchavu. Naaku telisina ey ammayi valla bf ki ala teppinchi undadu. anthaki minchi naaku em akkarledu.<br>
I will give my 100% efforts for u. Nenu change avvanu. Nuvvu nuvvu la undali, nenu ninnu happy ga choosukovali.<br>
Neetho, Neekosam entha extint varaku vellali anna nenu ochhesta.<br>
Nuvvu naa destiny. Ninnu choosukuntaa Batikeyyochhu. Nee smile kosam em anna cheyyochhu.<br>
Ninnu happy ga chooste naa face lo naaku teliakundanee smile ochhestadi. Naa motham mood nee medee depend ayyi untadi.<br>
ILY soooo much gaayee….<br>
I want u to be my forever.<br>
ILY soo sooo soooo much!!!!!<br>
Mwah!!!<br>
Happy Birthday My Love!<br><br>
~ Nee Teeeeeeeennnuuuuuuuuu<br><br></div></div></div>

    <div id="photo-modal" class="overlay"><div class="modal-box"><span class="close-btn" onclick="closeModal('photo-modal')">&times;</span><div class="slider-wrapper" id="slider-root"><button class="nav-btn" style="left:5px; z-index:10" onclick="changeSlide(-1)">❮</button><button class="nav-btn" style="right:5px; z-index:10" onclick="changeSlide(1)">❯</button></div></div></div>

    <div id="pickup-modal" class="overlay"><div class="modal-box"><span class="close-btn" onclick="closeModal('pickup-modal')">&times;</span><div class="modal-content"><p id="pickup-txt" class="big-pink-text"></p></div></div></div>

    <div id="mood-modal" class="overlay"><div class="modal-box"><span class="close-btn" onclick="closeModal('mood-modal')">&times;</span><div class="modal-content"><p class="big-pink-text">No matter the mood, you're my favorite drama queen! ❤️</p></div></div></div>

    <div id="secret-modal" class="overlay"><div class="modal-box"><span class="close-btn" onclick="closeModal('secret-modal')">&times;</span><div class="modal-content"><p class="big-pink-text">Caught me! Just like I caught feelings for you... 😉</p></div></div></div>

    <div id="gift-modal" class="overlay"><div class="modal-box"><span class="close-btn" onclick="closeModal('gift-modal')">&times;</span><div class="modal-content"><b style="color:var(--dark-pink)">REDEEM FROM TEENU:</b><div class="coupon-list coupon-box">🎟️ 1x Unlimited Love<br>🎟️ 1x Unlimited Hugs<br>🎟️ 1x Unlimited Kisses<br>🎟️ 1x Win any argument</div></div></div></div>

    <div id="mirror-modal" class="overlay"><div class="modal-box"><span class="close-btn" onclick="closeModal('mirror-modal')">&times;</span><div class="modal-content"><p class="big-pink-text">Mirror broken! The beauty on this side is too high to process! 😍</p></div></div></div>

    <div id="love-modal" class="overlay"><div class="modal-box"><span class="close-btn" onclick="closeModal('love-modal')">&times;</span><div class="modal-content"><p>TEENU'S LOVE LEVEL:</p><span id="love-percent">0%</span><p id="love-status" style="font-weight:bold"></p></div></div></div>

    <div id="void-modal" class="overlay"><div class="modal-box" style="background:#111; color:white;"><span class="close-btn" style="color:white" onclick="closeModal('void-modal')">&times;</span><div class="modal-content"><p style="font-size:1.5rem">You've pulled me into your world and I never want to escape. ❤️</p></div></div></div>

    <div id="alarm-modal" class="overlay"><div class="modal-box flash-emergency"><span class="close-btn" onclick="closeModal('alarm-modal')">&times;</span><div class="modal-content"><p class="big-pink-text">🚨 CUTENESS OVERLOAD! 🚨<br><br>Report to Teenu for immediate hugs & Kisses!</p></div></div></div>

    <script>
        const pickups = ["Your eyes 🤌🏼", "Your smile is my favorite", "The way you care for me🤌🏼", "why soo beautiful!🤌🏼", "Em unnavee babuuu😘", "Pickup lines saripovu ninnu describe cheyyali ante","Andam ammay ayite nee la unda annattundeyyy 🎶", "Gaayeee, blush blush blush!! Abbah em navvutunnave babuu ninnu choostaa batikeyyochhu", "Naa heart ki password pettanu… adi “nee peru”","Nee kallalo choosaka Google Maps avasaram ledu… naa Destination nuvvee"];
        const moods = ["I'M ANGRY 😤", "I'M CUTE 🥰", "I'M HUNGRY 🍕", "I'M DRAMA 💅", "I'M YOURS ❤️"];
        const memories = [
            { type: 'image', url: 'WhatsApp Image 2026-02-13 at 3.11.21 PM.jpeg', desc: "I love the way u look at me ❤️🫂" },
            { type: 'image', url: 'WhatsApp Image 2026-02-13 at 3.20.04 PM.jpeg', desc: "Eh pic choosi i always feel like, manam oka intlo nuvvu ready avtaa nenu ninnu choostaa unte entha bavuntado ani👀👀👀" },
            { type: 'video', url: 'WhatsApp Video 2026-02-13 at 3.18.03 PM.mp4', desc: "Mana 1st spot lo ninnu 1st timecapture chesina moment 🎥" },
            { type: 'image', url: 'WhatsApp Image 2026-02-13 at 3.21.03 PM.jpeg', desc: "Happy Birthday My Love🫂❤️" }
        ];

        let boxes = [
            {id:'letter-box', x:20, y:120, vx:1, vy:0.8}, {id:'photo-box', x:220, y:150, vx:-0.8, vy:1},
            {id:'pickup-box', x:120, y:250, vx:1.2, vy:-0.7}, {id:'mood-box', x:40, y:350, vx:0.9, vy:1.1},
            {id:'secret-box', x:200, y:400, vx:1.4, vy:1.4}, {id:'gift-box', x:100, y:500, vx:-1, vy:-1},
            {id:'mirror-box', x:250, y:500, vx:0.7, vy:-1.2}, {id:'love-box', x:30, y:600, vx:1.1, vy:-0.9},
            {id:'void-box', x:150, y:650, vx:0.4, vy:0.4}, {id:'alarm-box', x:240, y:300, vx:1.5, vy:1.5}
        ];

        let curSlide = 0;

        function unlock() {
            if(document.getElementById('pass').value.toUpperCase() === "ILY") {
                document.getElementById('lock-screen').style.display = 'none';
                document.getElementById('main-container').style.display = 'block';
                document.body.classList.add('birthday-bg');
                initSlider();
                boxes.forEach(b => b.el = document.getElementById(b.id));
                setInterval(() => { document.getElementById('mood-label').innerText = moods[Math.floor(Math.random()*moods.length)]; }, 2000);
                startParty();
            } else { alert("Use 'ILY' bangaram! ❤️"); }
        }

        function initSlider() {
            const root = document.getElementById('slider-root');
            memories.forEach((m, i) => {
                const div = document.createElement('div'); div.className = 'slide';
                if(m.type === 'video') div.innerHTML = `<video src="${m.url}" playsinline muted loop controls></video><div class="slide-desc">${m.desc}</div>`;
                else div.innerHTML = `<img src="${m.url}"><div class="slide-desc">${m.desc}</div>`;
                root.appendChild(div);
            });
            showSlide(0);
        }

        function showSlide(n) {
            const slides = document.getElementsByClassName('slide');
            if(n >= slides.length) curSlide = 0; else if(n < 0) curSlide = slides.length-1; else curSlide = n;
            for(let s of slides) { s.style.display = 'none'; let v = s.querySelector('video'); if(v) v.pause(); }
            slides[curSlide].style.display = 'block';
        }
        function changeSlide(n) { showSlide(curSlide + n); }

        function showPickup() { document.getElementById('pickup-txt').innerText = pickups[Math.floor(Math.random()*pickups.length)]; openModal('pickup-modal'); }

        function startLoveCounter() {
            openModal('love-modal');
            let c = 0, d = document.getElementById('love-percent'), s = document.getElementById('love-status');
            let i = setInterval(() => {
                c += 17532; d.innerText = c.toLocaleString() + "%";
                if(c > 1000000) { clearInterval(i); d.innerText = "OVERLOAD!"; s.innerText = "Love too heavy for this phone! 🤯❤️"; }
            }, 40);
        }

        function runAway(el) { 
            const b = boxes.find(x => x.id === 'secret-box');
            b.x = Math.random() * (window.innerWidth - 100); b.y = Math.random() * (window.innerHeight - 100);
        }

        function openModal(id) { document.getElementById(id).style.display = 'flex'; }
        function closeModal(id) { document.getElementById(id).style.display = 'none'; }

        function startParty() {
            confetti({ particleCount: 150, spread: 70, origin: { y: 0.6 } });
            const types = ['😘', '❤️', '🌹', '💖', '🥰', '✨', '🎁', '💋'];
            for (let i = 0; i < 40; i++) {
                let el = document.createElement('div'); el.className = 'emoji'; el.innerHTML = types[i % types.length];
                document.getElementById('main-container').appendChild(el);
                boxes.push({ el, x: Math.random()*window.innerWidth, y: Math.random()*window.innerHeight, vx: (Math.random()-0.5)*2, vy: (Math.random()-0.5)*2, isEmoji: true });
            }
            animate();
        }

        function animate() {
            boxes.forEach(b => {
                b.x += b.vx; b.y += b.vy;
                if (b.x < 0 || b.x > window.innerWidth - (b.isEmoji ? 40 : 85)) b.vx *= -1;
                if (b.y < 0 || b.y > window.innerHeight - (b.isEmoji ? 40 : 85)) b.vy *= -1;
                if(b.el) b.el.style.transform = `translate3d(${b.x}px, ${b.y}px, 0)`;
            });
            requestAnimationFrame(animate);
        }
    </script>
</body>
</html>
