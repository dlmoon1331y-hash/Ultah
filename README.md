index.html.
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ucapan Ulang Tahun</title>
<style>
    body {
        font-family: 'Poppins', sans-serif;
        margin: 0;
        padding: 0;
        background: linear-gradient(135deg, #ffafbd, #ffc3a0);
        color: #fff;
        text-align: center;
        overflow: hidden;
    }
    .page {
        display: none;
        height: 100vh;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        padding: 20px;
        animation: fadeIn 0.5s ease forwards;
    }
    .page.active {
        display: flex;
    }
    button {
        background: #ff5f6d;
        border: none;
        padding: 12px 24px;
        color: white;
        font-size: 1.2em;
        border-radius: 8px;
        cursor: pointer;
        margin-top: 20px;
        transition: 0.3s;
    }
    button:hover {
        background: #ff3c4a;
    }
    input {
        padding: 10px;
        font-size: 1.1em;
        border: none;
        border-radius: 5px;
        margin-top: 15px;
        text-align: center;
    }
    @keyframes fadeIn {
        from {opacity: 0;}
        to {opacity: 1;}
    }
    canvas {
        position: fixed;
        top: 0;
        left: 0;
        pointer-events: none;
    }
    #finalMessage {
        font-size: 1.5em;
        max-width: 600px;
        line-height: 1.5;
    }
    /* Animasi lucu popup */
    .funny-popup {
        position: fixed;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%) scale(0);
        background: rgba(255,255,255,0.9);
        color: #ff5f6d;
        font-size: 1.5em;
        font-weight: bold;
        padding: 15px 25px;
        border-radius: 12px;
        animation: popupBounce 1s ease forwards;
        z-index: 9999;
    }
    @keyframes popupBounce {
        0% { transform: translate(-50%, -50%) scale(0); opacity: 0; }
        40% { transform: translate(-50%, -50%) scale(1.2); opacity: 1; }
        60% { transform: translate(-50%, -50%) scale(0.9); }
        80% { transform: translate(-50%, -50%) scale(1.05); }
        100% { transform: translate(-50%, -50%) scale(1); opacity: 1; }
    }
</style>
</head>
<body>

<div id="page1" class="page active">
    <h1>Halo! 🎉</h1>
    <button onclick="nextPage(2)">Mulai</button>
</div>

<div id="page2" class="page">
    <h2>Siapa namamu?</h2>
    <input type="text" id="nameInput" placeholder="Masukkan nama">
    <button onclick="checkName()">Lanjut</button>
</div>

<div id="page3" class="page">
    <h2>Makanan favorit kamu?</h2>
    <input type="text" id="foodInput" placeholder="Masukkan makanan">
    <button onclick="checkFood()">Lanjut</button>
</div>

<div id="page4" class="page">
    <h2>Tahun lahir kamu?</h2>
    <input type="number" id="yearInput" placeholder="Misal: 2000">
    <button onclick="calcAge()">Lihat Ucapan</button>
</div>

<div id="page5" class="page">
    <h1 id="finalMessage"></h1>
</div>

<canvas id="confettiCanvas"></canvas>

<script>
    let correctName = "Yunita";
    let correctFood = "Coklat";
    let umur = "18";
    let inputName = "2007";

    function nextPage(num) {
        document.querySelectorAll(".page").forEach(p => p.classList.remove("active"));
        document.getElementById("page" + num).classList.add("active");
    }

    function showFunnyPopup(text, callback) {
        let popup = document.createElement("div");
        popup.className = "funny-popup";
        popup.innerHTML = text;
        document.body.appendChild(popup);
        setTimeout(() => {
            popup.remove();
            if (callback) callback();
        }, 1500);
    }

    function checkName() {
        inputName = document.getElementById("nameInput").value.trim();
        if(inputName.toLowerCase() === correctName.toLowerCase()) {
            showFunnyPopup("🎉 Benar banget! 😄", () => nextPage(3));
        } else {
            alert("Namanya salah 😅");
        }
    }

    function checkFood() {
        let food = document.getElementById("foodInput").value.trim();
        if(food.toLowerCase() === correctFood.toLowerCase()) {
            showFunnyPopup("🍫 Mantap! 👍", () => nextPage(4));
        } else {
            alert("Makanan salah 🍽️");
        }
    }

    function calcAge() {
        let year = parseInt(document.getElementById("yearInput").value);
        if(!year || year > new Date().getFullYear()) {
            alert("Masukkan tahun lahir yang valid");
            return;
        }
        umur = new Date().getFullYear() - year;
        showFunnyPopup(`🤣 Udah makin tua aja nih (${umur} th)`, () => {
            let message = `Selamat ulang tahun yaa ${inputName}! 🎂 Sekarang kamu ${umur} tahun. Semoga menjadi pribadi yang lebih baik yaa,panjang umur & bahagia selalu! inget yaa kalau ada masalah jangan lupa curhat sama yang diatas ,sekali lagi happy birthday yunn💖`;
            nextPage(5);
            typeWriter(message, "finalMessage", 50);
            startConfetti();
        });
    }

    function typeWriter(text, elementId, speed) {
        let i = 0;
        let element = document.getElementById(elementId);
        element.innerHTML = "";
        let typing = setInterval(() => {
            if (i < text.length) {
                element.innerHTML += text.charAt(i);
                i++;
            } else {
                clearInterval(typing);
            }
        }, speed);
    }

    // Confetti Animation
    const canvas = document.getElementById('confettiCanvas');
    const ctx = canvas.getContext('2d');
    let confetti = [];
    let W = window.innerWidth;
    let H = window.innerHeight;
    canvas.width = W;
    canvas.height = H;

    function randomColor() {
        const colors = ['#ff5f6d', '#ffc371', '#47e7b2', '#1ecbe1', '#f5a623'];
        return colors[Math.floor(Math.random() * colors.length)];
    }

    function ConfettiPiece() {
        this.x = Math.random() * W;
        this.y = Math.random() * H - H;
        this.r = Math.random() * 6 + 4;
        this.d = Math.random() * 100;
        this.color = randomColor();
        this.tilt = Math.floor(Math.random() * 10) - 10;
        this.tiltAngleIncremental = (Math.random() * 0.07) + 0.05;
        this.tiltAngle = 0;

        this.draw = function() {
            ctx.beginPath();
            ctx.lineWidth = this.r / 2;
            ctx.strokeStyle = this.color;
            ctx.moveTo(this.x + this.tilt + this.r / 4, this.y);
            ctx.lineTo(this.x + this.tilt, this.y + this.tilt + this.r / 4);
            ctx.stroke();
        }
    }

    function startConfetti() {
        confetti = [];
        for (let i = 0; i < 150; i++) {
            confetti.push(new ConfettiPiece());
        }
        requestAnimationFrame(drawConfetti);
    }

    function drawConfetti() {
        ctx.clearRect(0, 0, W, H);
        for (let i = 0; i < confetti.length; i++) {
            let p = confetti[i];
            p.tiltAngle += p.tiltAngleIncremental;
            p.y += (Math.cos(p.d) + 3 + p.r / 2) / 2;
            p.x += Math.sin(p.d);
            p.tilt = Math.sin(p.tiltAngle) * 15;

            if (p.y > H) {
                p.x = Math.random() * W;
                p.y = -10;
                p.tilt = Math.floor(Math.random() * 10) - 10;
            }
            p.draw();
        }
        requestAnimationFrame(drawConfetti);
    }
</script>

</body>
</html>
