<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <title>بازی کلمات درهم‌ریخته - پایه دوم</title>
    <style>
        @font-face {
            font-family: 'MyCustomFont';
            src: url('https://www.fontify.ir/wp-content/uploads/2023/07/Vazirmatn-FD-WOL.woff2') format('woff2');
            font-weight: normal;
            font-style: normal;
        }

        body { 
            font-family: 'MyCustomFont', Tahoma, sans-serif; 
            background-color: #e6f7ff; 
            text-align: center; 
            margin: 0;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
            position: relative; /* برای قرارگیری موسیقی */
        }

        .puppet {
            position: absolute;
            font-size: 60px;
            animation: float 4s infinite ease-in-out;
            z-index: 1;
        }
        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-20px); }
        }

        .container { 
            background: rgba(255, 255, 255, 0.95); 
            padding: 30px; 
            border-radius: 30px; 
            border: 8px solid #87ceeb;
            width: 90%; 
            max-width: 500px; 
            box-shadow: 0 10px 30px rgba(0,0,0,0.15);
            z-index: 10;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        h1 { color: #4682b4; margin-bottom: 5px; }
        .subtitle { color: #6495ed; font-size: 1.2em; font-weight: bold; margin-bottom: 15px; }
        .teacher-info { font-size: 0.9em; color: #555; margin-bottom: 20px; }

        .word-display { 
            font-size: 35px; 
            font-weight: bold; 
            color: #ff4500; 
            margin: 25px 0; 
            letter-spacing: 15px;
            word-break: break-all;
            line-height: 1.2;
        }
        input[type="text"] { 
            padding: 10px; 
            width: 80%; 
            border-radius: 10px; 
            border: 1px solid #ccc;
            margin-bottom: 10px;
            font-size: 1em;
        }
        button { 
            background: #ff69b4; 
            border: none; 
            padding: 12px 30px; 
            border-radius: 20px; 
            color: white; 
            cursor: pointer; 
            font-size: 1.1em; 
            margin: 5px;
            transition: 0.3s;
            box-shadow: 0 4px 8px rgba(255, 105, 180, 0.3);
        }
        button:hover { background: #da4ea2; transform: scale(1.05); }
        button:active { transform: scale(1); }
        .hidden { display: none; }

        /* استایل کارنامه */
        #resultScreen {
            margin-top: 30px;
            padding: 20px;
            background-color: #fff;
            border-radius: 20px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            width: 100%;
            box-sizing: border-box;
        }
        #resultScreen h2 {
            color: #4682b4;
            margin-bottom: 15px;
        }
        #resultTable {
            width: 100%;
            border-collapse: collapse;
            margin-top: 10px;
        }
        #resultTable th, #resultTable td {
            padding: 10px;
            border: 1px solid #e0e0e0;
            text-align: center;
        }
        #resultTable th {
            background-color: #f0f8ff;
            color: #4682b4;
        }
        #resultScreen button {
            background: #87ceeb; /* آبی */
            margin-top: 20px;
            padding: 10px 25px;
            font-size: 1em;
        }
        #resultScreen button:hover {
            background: #6495ed;
        }
        
        /* استایل موسیقی */
        .music-control {
            position: absolute;
            bottom: 20px;
            right: 20px;
            z-index: 100;
            background: rgba(255, 255, 255, 0.8);
            padding: 10px;
            border-radius: 15px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            display: flex;
            align-items: center;
        }
        .music-control button {
            background: none;
            border: none;
            font-size: 24px;
            cursor: pointer;
            margin: 0 5px;
            color: #4682b4;
        }
        .music-control button:hover {
            color: #ff69b4;
        }
    </style>
</head>
<body>

<!-- عروسک‌های متحرک -->
<div class="puppet" style="top: 10%; left: 10%;">🧸</div>
<div class="puppet" style="top: 10%; right: 10%;">🦄</div>
<div class="puppet" style="bottom: 10%; left: 10%;">🎀</div>
<div class="puppet" style="bottom: 10%; right: 10%;">🐥</div>

<!-- کنترل موسیقی -->
<div class="music-control">
    <button id="playMusicBtn" onclick="toggleMusic()">▶️</button>
    <audio id="backgroundMusic" loop>
        <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mpeg">
        مرورگر شما از پخش صدا پشتیبانی نمی‌کند.
    </audio>
    <span>موسیقی پس‌زمینه</span>
</div>


<div class="container" id="gameContainer">
    <h1 id="gameTitle">بازی آموزشی</h1>
    <p class="teacher-info">آموزگار: شایسته صفی</p>
    
    <div id="startScreen">
        <input type="text" id="studentName" placeholder="نام دانش‌آموز را وارد کنید">
        <br><br><button onclick="startGame()">شروع بازی</button>
    </div>

    <div id="quizScreen" class="hidden">
        <p id="studentDisplay"></p>
        <div class="word-display" id="scrambledWord"></div>
        <div id="optionsContainer">
            <button id="btn1" onclick="checkAnswer(0)"></button>
            <button id="btn2" onclick="checkAnswer(1)"></button>
        </div>
    </div>

    <div id="resultScreen" class="hidden">
        <h2>کارنامه بازی</h2>
        <p id="studentResultName"></p>
        <table id="resultTable">
            <thead>
                <tr>
                    <th>سوال</th>
                    <th>کلمه اصلی</th>
                    <th>پاسخ شما</th>
                    <th>نتیجه</th>
                </tr>
            </thead>
            <tbody id="resultBody">
                <!-- نتایج در اینجا اضافه خواهند شد -->
            </tbody>
        </table>
        <button onclick="restartGame()">بازی مجدد</button>
    </div>
</div>

<script>
    const words = [
        "کتابخانه", "مدرسه", "معلم", "شاگرد", "نیمکت", "دفترچه", 
        "نقاشی", "خورشید", "ستاره", "سنجاقک", "قورباغه", "کشاورز", 
        "مهربان", "دانشمند", "میکروب", "تابستان", "زمستان", "دبستان", 
        "آسمان", "خانواده"
    ];

    let currentQuestionIndex = 0;
    let shuffledWordsForQuiz = [];
    let studentAnswers = []; // برای ذخیره پاسخ‌های هر سوال
    let correctAnswersCount = 0;

    const backgroundMusic = document.getElementById('backgroundMusic');
    const playMusicBtn = document.getElementById('playMusicBtn');

    function toggleMusic() {
        if (backgroundMusic.paused) {
            backgroundMusic.play();
            playMusicBtn.innerText = "⏸️";
        } else {
            backgroundMusic.pause();
            playMusicBtn.innerText = "▶️";
        }
    }

    function scramble(word) {
        let arr = word.split('');
        // اطمینان از اینکه حروف تکراری به درستی ترکیب میشوند
        for (let i = arr.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [arr[i], arr[j]] = [arr[j], arr[i]];
        }
        return arr.join(' ');
    }

    function startGame() {
        let nameInput = document.getElementById('studentName');
        let name = nameInput.value.trim();
        if (!name) return alert("لطفاً نام دانش‌آموز را وارد کنید.");
        
        document.getElementById('studentDisplay').innerText = `${name} عزیز، کلمه زیر را تشخیص بده:`;
        document.getElementById('startScreen').classList.add('hidden');
        document.getElementById('quizScreen').classList.remove('hidden');
        
        shuffledWordsForQuiz = [...words].sort(() => Math.random() - 0.5);
        studentAnswers = []; // پاکسازی پاسخ‌های قبلی
        currentQuestionIndex = 0;
        correctAnswersCount = 0;
        
        showQuestion();
        toggleMusic(); // شروع پخش موسیقی
    }

    function showQuestion() {
        if (currentQuestionIndex >= shuffledWordsForQuiz.length) {
            showResults();
            return;
        }
        
        let word = shuffledWordsForQuiz[currentQuestionIndex];
        document.getElementById('scrambledWord').innerText = scramble(word);
        
        // تولید یک گزینه غلط متفاوت
        let wrongOption = words[Math.floor(Math.random() * words.length)];
        while (wrongOption === word) {
            wrongOption = words[Math.floor(Math.random() * words.length)];
        }
        
        let options = [word, wrongOption].sort(() => Math.random() - 0.5);
        document.getElementById('btn1').innerText = options[0];
        document.getElementById('btn2').innerText = options[1];
        document.getElementById('btn1').style.backgroundColor = '#ff69b4'; // بازنشانی رنگ دکمه‌ها
        document.getElementById('btn2').style.backgroundColor = '#ff69b4';
    }

    function checkAnswer(buttonIndex) {
        const selectedButton = document.getElementById(`btn${buttonIndex + 1}`);
        const correctAnswer = shuffledWordsForQuiz[currentQuestionIndex];
        const userAnswer = selectedButton.innerText;
        
        let isCorrect = (userAnswer === correctAnswer);
        
        studentAnswers.push({
            question: currentQuestionIndex + 1,
            correct: correctAnswer,
            user: userAnswer,
            isCorrect: isCorrect
        });
        
        if (isCorrect) {
            selectedButton.style.backgroundColor = '#90ee90'; // سبز روشن برای پاسخ صحیح
            correctAnswersCount++;
            // alert("آفرین! درست بود."); // پیغام موقت
        } else {
            selectedButton.style.backgroundColor = '#f08080'; // قرمز روشن برای پاسخ غلط
            document.getElementById(`btn${(buttonIndex === 0 ? 1 : 0) + 1}`).style.backgroundColor = '#90ee90'; // سبز کردن دکمه پاسخ صحیح
            // alert("اشتباه بود! کلمه درست: " + correctAnswer); // پیغام موقت
        }
        
        // جلوگیری از کلیک‌های متعدد روی دکمه‌ها تا رفتن به سوال بعدی
        document.getElementById('btn1').onclick = null;
        document.getElementById('btn2').onclick = null;

        // رفتن به سوال بعدی پس از چند ثانیه
        setTimeout(() => {
            currentQuestionIndex++;
            if (currentQuestionIndex < shuffledWordsForQuiz.length) {
                showQuestion();
            } else {
                showResults();
            }
        }, 1500); // 1.5 ثانیه تاخیر
    }

    function showResults() {
        document.getElementById('quizScreen').classList.add('hidden');
        document.getElementById('resultScreen').classList.remove('hidden');
        
        document.getElementById('studentResultName').innerText = `نتایج ${document.getElementById('studentName').value}:`;
        const resultBody = document.getElementById('resultBody');
        resultBody.innerHTML = ''; // پاکسازی جدول قبلی
        
        studentAnswers.forEach(answer => {
            const row = resultBody.insertRow();
            row.innerHTML = `
                <td>${answer.question}</td>
                <td>${answer.correct}</td>
                <td>${answer.user}</td>
                <td style="color: ${answer.isCorrect ? 'green' : 'red'};">${answer.isCorrect ? 'صحیح ✅' : 'غلط ❌'}</td>
            `;
        });

        // اضافه کردن امتیاز کلی
        const finalRow = resultBody.insertRow();
        finalRow.innerHTML = `
            <td colspan="3" style="font-weight: bold;">امتیاز کل:</td>
            <td style="font-weight: bold; color: ${correctAnswersCount >= (shuffledWordsForQuiz.length / 2) ? 'green' : 'orange'};">${correctAnswersCount} از ${shuffledWordsForQuiz.length}</td>
        `;
    }

    function restartGame() {
        document.getElementById('resultScreen').classList.add('hidden');
        document.getElementById('startScreen').classList.remove('hidden');
        document.getElementById('studentName').value = ''; // پاک کردن فیلد نام
        // توقف موسیقی هنگام شروع مجدد
        backgroundMusic.pause();
        backgroundMusic.currentTime = 0; // بازگرداندن به ابتدا
        playMusicBtn.innerText = "▶️";
    }
    
    // اطمینان از اینکه دکمه پخش موسیقی در ابتدا درست نمایش داده شود
    window.onload = () => {
        if (backgroundMusic.paused) {
            playMusicBtn.innerText = "▶️";
        } else {
            playMusicBtn.innerText = "⏸️";
        }
    };

</script>
</body>
</html>
