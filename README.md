<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>تحدي المهارات الرقمية | أول ثانوي</title>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;900&display=swap" rel="stylesheet">
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Cairo', sans-serif;
        }

        body {
            background: linear-gradient(135deg, #0f172a, #1e1b4b, #311042);
            color: #f8fafc;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: rgba(30, 41, 59, 0.75);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            padding: 30px;
            width: 100%;
            max-width: 650px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5);
            text-align: center;
            position: relative;
        }

        h1 {
            font-size: 2rem;
            margin-bottom: 10px;
            background: linear-gradient(90deg, #38bdf8, #818cf8, #c084fc);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        p.subtitle {
            color: #94a3b8;
            margin-bottom: 25px;
            font-size: 1rem;
        }

        .screen {
            display: none;
        }

        .screen.active {
            display: block;
        }

        input[type="text"] {
            width: 100%;
            padding: 15px;
            border-radius: 12px;
            border: 2px solid #334155;
            background: #0f172a;
            color: #fff;
            font-size: 1.1rem;
            margin-bottom: 20px;
            text-align: center;
            outline: none;
            transition: 0.3s;
        }

        input[type="text"]:focus {
            border-color: #6366f1;
            box-shadow: 0 0 15px rgba(99, 102, 241, 0.4);
        }

        button.main-btn {
            background: linear-gradient(135deg, #6366f1, #a855f7);
            color: white;
            border: none;
            padding: 15px 35px;
            font-size: 1.1rem;
            font-weight: 700;
            border-radius: 12px;
            cursor: pointer;
            transition: 0.3s;
            box-shadow: 0 5px 15px rgba(99, 102, 241, 0.4);
            width: 100%;
        }

        button.main-btn:hover {
            transform: translateY(-2px);
            opacity: 0.9;
        }

        .quiz-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            font-size: 0.95rem;
            color: #94a3b8;
        }

        .badge {
            padding: 5px 12px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 700;
        }
        .badge.easy { background: #059669; color: #fff; }
        .badge.medium { background: #d97706; color: #fff; }
        .badge.hard { background: #dc2626; color: #fff; }

        .timer-bar {
            width: 100%;
            height: 8px;
            background: #334155;
            border-radius: 4px;
            margin-bottom: 25px;
            overflow: hidden;
        }

        .timer-progress {
            width: 100%;
            height: 100%;
            background: #38bdf8;
            transition: width 1s linear;
        }

        .question-text {
            font-size: 1.3rem;
            font-weight: 600;
            margin-bottom: 25px;
            line-height: 1.6;
        }

        .options-grid {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .option-btn {
            background: rgba(51, 65, 85, 0.6);
            border: 2px solid rgba(255, 255, 255, 0.05);
            padding: 15px;
            border-radius: 12px;
            color: #fff;
            font-size: 1rem;
            cursor: pointer;
            text-align: right;
            transition: 0.2s;
        }

        .option-btn:hover {
            background: rgba(99, 102, 241, 0.2);
            border-color: #6366f1;
        }

        .option-btn.correct {
            background: #059669 !important;
            border-color: #10b981 !important;
        }

        .option-btn.wrong {
            background: #dc2626 !important;
            border-color: #ef4444 !important;
        }

        .result-score {
            font-size: 3rem;
            font-weight: 900;
            color: #38bdf8;
            margin: 20px 0;
        }

        .footer-credit {
            margin-top: 30px;
            font-size: 0.85rem;
            color: #64748b;
            border-top: 1px solid rgba(255, 255, 255, 0.05);
            padding-top: 15px;
        }
    </style>
</head>
<body>

    <div class="container">
        <!-- شاشة البداية -->
        <div id="welcome-screen" class="screen active">
            <h1>🚀 تحدي المهارات الرقمية</h1>
            <p class="subtitle">اختبر معلوماتك في منهج أول ثانوي (الترم الأول)</p>
            <input type="text" id="username" placeholder="أدخل اسمك الكريم هنا..." autocomplete="off">
            <button class="main-btn" id="start-btn">ابدأ التحدي</button>
            <div class="footer-credit">إعداد الطالب: KHUBRANI SALMAN</div>
        </div>

        <!-- شاشة الأسئلة -->
        <div id="quiz-screen" class="screen">
            <div class="quiz-header">
                <span id="question-count">السؤال 1 من 15</span>
                <span id="difficulty-badge" class="badge easy">سهل</span>
            </div>
            <div class="timer-bar">
                <div id="timer-progress" class="timer-progress"></div>
            </div>
            <div id="question-container" class="question-text">نص السؤال سيظهر هنا</div>
            <div class="options-grid" id="options-container"></div>
        </div>

        <!-- شاشة النتيجة -->
        <div id="result-screen" class="screen">
            <h1>🎉 انتهى الاختبار!</h1>
            <p id="result-message" class="subtitle">أحسنت صنعاً</p>
            <div class="result-score" id="final-score">0 / 15</div>
            <button class="main-btn" id="restart-btn">إعادة الاختبار</button>
            <div class="footer-credit">إعداد الطالب: KHUBRANI SALMAN</div>
        </div>
    </div>

    <script>
        const questions = [
            // سهلة (1-5)
            { q: "ما هو المصطلح الذي يُطلق على حماية الأجهزة والبيانات من السرقة أو التلف؟", options: ["أمن المعلومات", "التصميم الرقمي", "البرمجة الهيكلية", "معالجة النصوص"], answer: 0, difficulty: "easy", diffText: "سهل" },
            { q: "من أمثلة كلمات المرور القوية:", options: ["12345678", "password", "Abc@987#xyz", "00000000"], answer: 2, difficulty: "easy", diffText: "سهل" },
            { q: "أي مما يلي يُعتبر وحدة تخزين خارجية رئيسية؟", options: ["قرص صلب خارجي (External HDD)", "المعالج (CPU)", "الذاكرة العشوائية (RAM)", "اللوحة الأم"], answer: 0, difficulty: "easy", diffText: "سهل" },
            { q: "ما هي الشبكة التي تربط بين أجهزة الحاسب حول العالم؟", options: ["الشبكة المحلية (LAN)", "الإنترنت (Internet)", "الشبكة الشخصية (PAN)", "الإنترانت (Intranet)"], answer: 1, difficulty: "easy", diffText: "سهل" },
            { q: "ما هو التطبيق الأكثر استخداماً لإنشاء العروض التقديمية التفاعلية؟", options: ["Microsoft Excel", "Microsoft Word", "Microsoft PowerPoint", "Microsoft Access"], answer: 2, difficulty: "easy", diffText: "سهل" },

            // متوسطة (6-10)
            { q: "ماذا يُقصد بمفهوم 'المواطنة الرقمية'؟", options: ["شراء الأجهزة الذكية عبر الإنترنت", "الاستخدام الآمن والمسؤول والأخلاقي للتقنية", "صيانة الأجهزة البرمجية", "تصميم مواقع الويب"], answer: 1, difficulty: "medium", diffText: "متوسط" },
            { q: "ما هو الهجوم الذي يتم فيه خداع المستخدم للكشف عن معلوماته السرية عبر رسائل وهمية؟", options: ["هجوم حجب الخدمة", "التصيد الاحتيالي (Phishing)", "التشفير المتماثل", "التثبيت النظيف"], answer: 1, difficulty: "medium", diffText: "متوسط" },
            { q: "تُعرف البيانات التي تم معالجتها وتنسيقها لتصبح ذات معنى بـ:", options: ["المدخلات الخام", "المعلومات (Information)", "الرموز العشوائية", "الملفات المؤقتة"], answer: 1, difficulty: "medium", diffText: "متوسط" },
            { q: "ما هي البرمجيات التي تهدف إلى جمع معلومات عن المستخدم والتجسس عليه دون علمه؟", options: ["برمجيات الفدية", "برمجيات التجسس (Spyware)", "جدار النار", "برامج ضغط الملفات"], answer: 1, difficulty: "medium", diffText: "متوسط" },
            { q: "ما هو النظام الذي يعمل كمراقبة وحائط دفاع للتحكم في حركة المرور بين الشبكة والإنترنت؟", options: ["المعالج", "الجدار الناري (Firewall)", "مزود الطاقة", "القرص الصلب"], answer: 1, difficulty: "medium", diffText: "متوسط" },

            // صعبة (11-15)
            { q: "أي من البروتوكولات التالية يُعتبر الأكثر أمناً لتصفح مواقع الويب وتأمين البيانات المتبادلة؟", options: ["HTTP", "FTP", "HTTPS", "SMTP"], answer: 2, difficulty: "hard", diffText: "صعب" },
            { q: "ما هي البرمجيات الخبيثة التي تقوم بتشفير ملفات الضحية وطلب فدية مالية مقابل فكها؟", options: ["أحصنة طروادة", "برمجيات الفدية (Ransomware)", "الفيروسات المتنقلة", "ديدان الحاسب"], answer: 1, difficulty: "hard", diffText: "صعب" },
            { q: "ماذا تعني عملية 'التشفير' (Encryption) في أمن البيانات؟", options: ["حذف الملفات نهائياً", "ضغط الملفات لتقليل حجمها", "تحويل البيانات إلى صيغة غير مخفية", "تحويل البيانات إلى رموز مبهمة لا تفهم إلا بمفتاح"], answer: 3, difficulty: "hard", diffText: "صعب" },
            { q: "ما هو مصطلح 'المصادقة ثنائية العوامل' (2FA)؟", options: ["استخدام كلمتي مرور متشابهة", "طريقة أمان تتطلب خطوتين أو طريقتين للتحقق من الهوية", "ربط جهازين حاسب معا", "تثبيت نظام تشغيل مزدوج"], answer: 1, difficulty: "hard", diffText: "صعب" },
            { q: "ما هو الهدف الأساسي من عملية النسخ الاحتياطي المنتظم للبيانات (Backup)؟", options: ["تسريع أداء الجهاز", "استعادة البيانات عند تعرضها للفقدان أو التلف", "توفير مساحة تخزينية إضافية", "منع ظهور الإعلانات"], answer: 1, difficulty: "hard", diffText: "صعب" }
        ];

        let currentQuestionIndex = 0;
        let score = 0;
        let timer;
        let timeLeft = 10;
        let userName = "";

        // ربط الأحداث برمجياً لضمان الاستجابة التامة
        document.getElementById("start-btn").addEventListener("click", startQuiz);
        document.getElementById("restart-btn").addEventListener("click", restartQuiz);
        
        // السماح بالضغط على زر Enter من لوحة المفاتيح
        document.getElementById("username").addEventListener("keypress", function(event) {
            if (event.key === "Enter") {
                startQuiz();
            }
        });

        function startQuiz() {
            const nameInput = document.getElementById("username").value.trim();
            if (nameInput === "") {
                alert("الرجاء إدخال اسمك الكريم قبل البدء!");
                return;
            }
            userName = nameInput;
            document.getElementById("welcome-screen").classList.remove("active");
            document.getElementById("quiz-screen").classList.add("active");
            loadQuestion();
        }

        function loadQuestion() {
            clearInterval(timer);
            timeLeft = 10;
            updateTimerBar();

            if (currentQuestionIndex >= questions.length) {
                showResults();
                return;
            }

            const q = questions[currentQuestionIndex];
            document.getElementById("question-count").innerText = `السؤال ${currentQuestionIndex + 1} من ${questions.length}`;
            
            const badge = document.getElementById("difficulty-badge");
            badge.innerText = q.diffText;
            badge.className = `badge ${q.difficulty}`;

            document.getElementById("question-container").innerText = q.q;

            const optionsContainer = document.getElementById("options-container");
            optionsContainer.innerHTML = "";

            q.options.forEach((opt, index) => {
                const btn = document.createElement("button");
                btn.className = "option-btn";
                btn.innerText = opt;
                btn.onclick = () => selectAnswer(index);
                optionsContainer.appendChild(btn);
            });

            startTimer();
        }

        function updateTimerBar() {
            const progress = document.getElementById("timer-progress");
            if(progress) progress.style.width = "100%";
        }

        function startTimer() {
            const progress = document.getElementById("timer-progress");
            
            timer = setInterval(() => {
                timeLeft--;
                if(progress) progress.style.width = `${(timeLeft / 10) * 100}%`;

                if (timeLeft <= 0) {
                    clearInterval(timer);
                    handleTimeout();
                }
            }, 1000);
        }

        function handleTimeout() {
            const q = questions[currentQuestionIndex];
            const buttons = document.querySelectorAll(".option-btn");
            
            if(buttons[q.answer]) buttons[q.answer].classList.add("correct");
            disableOptions();

            setTimeout(() => {
                currentQuestionIndex++;
                loadQuestion();
            }, 1500);
        }

        function selectAnswer(selectedIndex) {
            clearInterval(timer);
            const q = questions[currentQuestionIndex];
            const buttons = document.querySelectorAll(".option-dim, .option-btn");

            if (selectedIndex === q.answer) {
                buttons[selectedIndex].classList.add("correct");
                score++;
            } else {
                buttons[selectedIndex].classList.add("wrong");
                if(buttons[q.answer]) buttons[q.answer].classList.add("correct");
            }

            disableOptions();

            setTimeout(() => {
                currentQuestionIndex++;
                loadQuestion();
            }, 1500);
        }

        function disableOptions() {
            const buttons = document.querySelectorAll(".option-btn");
            buttons.forEach(btn => btn.style.pointerEvents = "none");
        }

        function showResults() {
            document.getElementById("quiz-screen").classList.remove("active");
            document.getElementById("result-screen").classList.add("active");
            
            document.getElementById("result-message").innerText = `بطلنا المميز: ${userName}`;
            document.getElementById("final-score").innerText = `${score} / ${questions.length}`;
        }

        function restartQuiz() {
            currentQuestionIndex = 0;
            score = 0;
            document.getElementById("result-screen").classList.remove("active");
            document.getElementById("welcome-screen").classList.add("active");
            document.getElementById("username").value = "";
        }
    </script>
</body>
</html>
