<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>뮤지컬 인사이드 미</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Do+Hyeon&display=swap" rel="stylesheet">
    <style>
        /* 외부 배경 그라데이션 및 폰트 설정 */
        body {
            font-family: 'Do Hyeon', sans-serif;
            background: linear-gradient(135deg, #f3e8ff, #ffc0cb, #e879f9);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 1rem;
            overflow: hidden;
            touch-action: pan-y;
        }

        /* 텍스트 그라데이션 및 그림자 효과 */
        .text-gradient {
            background-image: linear-gradient(to right, #ff69b4, #8b5cf6);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            color: transparent;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
        }

        /* 버튼 그라데이션 */
        .btn-gradient {
            background-image: linear-gradient(to right, #ff69b4, #8b5cf6);
            color: white;
            transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
            font-weight: 500;
        }

        .btn-gradient:hover {
            transform: scale(1.05);
            box-shadow: 0 10px 15px rgba(0,0,0,0.1);
        }

        .btn-gradient:active {
            transform: scale(0.98);
        }
        
        /* 스마트폰 프레임 스타일 */
        .phone-frame {
            width: 100%;
            max-width: 420px;
            height: 80vh;
            max-height: 700px;
            background-color: #f3e8ff;
            border-radius: 2.5rem;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.2), 0 0 0 10px #ffc0cb;
            border: 8px solid #ffc0cb;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 1rem;
            position: relative;
        }

        /* 노치 디자인 */
        .notch {
            position: absolute;
            top: 0.5rem;
            left: 50%;
            transform: translateX(-50%);
            width: 100px;
            height: 25px;
            background-color: #000;
            border-bottom-left-radius: 12px;
            border-bottom-right-radius: 12px;
            z-index: 10;
        }

        /* 내부 컨테이너에 배경 이미지 추가 및 블러 처리 */
        .game-inner-container {
            width: 100%;
            height: 100%;
            background-image: url('data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAYABgAAD/2wBDAAgGBgcGBQgHBwcJCQgKDBQNDAsLDBkSEw8UHRofHh0aHBwgJC4nICIsIxwcKDcpLDAxNDQ0Hyc5PTg4Ojo/PDY2/9sBDAgwICAgJCAgJChUMDQoMDAwMDQwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDQz/wAARCAUAA2ADASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEBAAAAAAAAAAECAwQFBgcICRAf/2Q==');
            background-size: cover;
            background-position: center;
            backdrop-filter: blur(8px);
            background-color: rgba(255, 255, 255, 0.5); /* 오버레이 효과 */
            border-radius: 2rem;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            padding: 1rem;
        }

        /* 글자 주변의 작은 SNS 아이콘 스타일 */
        .icon-small {
            font-size: 20px;
            position: absolute;
        }
        
        /* SNS 아이콘 위치 조정 */
        .icon-top-right { top: -10px; right: -10px; }
        .icon-bottom-left { bottom: -10px; left: -10px; }

        @keyframes pulse-icon {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.1); }
        }
        
        .icon-animated {
            animation: pulse-icon 2s infinite;
        }

        .start-page-content {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100%;
        }

        .result-container {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100%;
            padding-top: 4rem; /* 전체 콘텐츠를 아래로 내리기 위한 상단 패딩 */
        }

        .result-box {
            position: relative;
            padding: 1.5rem;
            background: rgba(255, 255, 255, 0.5);
            border-radius: 1rem;
            border: 1px solid rgba(200, 200, 200, 0.5);
            backdrop-filter: blur(5px);
            width: 100%;
        }

        .qr-section {
            display: flex;
            flex-direction: row;
            justify-content: center;
            align-items: center;
            gap: 1.5rem; /* 멘트와 QR 코드 사이의 간격 */
            margin-top: 1rem;
            margin-bottom: 2rem;
        }
    </style>
</head>
<body>

<div class="phone-frame">
    <div class="notch"></div>
    <div id="game-container" class="game-inner-container">
        <div id="game-content" class="text-gray-800 w-full px-4">
            <!-- 게임 내용이 여기에 동적으로 렌더링됩니다. -->
        </div>
    </div>
</div>

<script>
    // 게임 상태를 관리하는 객체
    let currentStage = 'start';
    let resultsMap = '';

    // 게임 질문과 선택지를 정의하는 객체
    const questions = {
        'start': {
            text: "내 마음 속에는 어떤 '나'가 살고 있을까?",
            subtext: "질문에 답하고 내 안의 진짜 '나'를 만나보세요.",
            choices: [
                { text: '시작하기', next: 'q1_intro' }
            ]
        },
        'q1_intro': {
            text: "당신은 텅 빈 공간에 서 있습니다. 저 멀리 희미한 문 세 개가 보이네요. 당신의 마음을 가장 끌어당기는 문은 무엇인가요?",
            choices: [
                { text: '논리의 문', next: 'q1_A' },
                { text: '감성의 문', next: 'q1_B' },
                { text: '관찰의 문', next: 'q1_C' }
            ]
        },
        'q1_A': {
            text: "문 안으로 들어서자, 복잡한 퍼즐이 나타납니다. '당신에게 가장 중요한 것'을 나타내는 단어는?",
            choices: [
                { text: '계획', next: 'result_A' },
                { text: '경험', next: 'result_B' },
                { text: '감정', next: 'result_C' }
            ]
        },
        'q1_B': {
            text: "문 안으로 들어서자, 다채로운 물감들이 보입니다. 당신의 감정을 가장 잘 표현하는 색깔은?",
            choices: [
                { text: '뜨거운 빨강', next: 'result_B' },
                { text: '차분한 파랑', next: 'result_C' },
            ]
        },
        'q1_C': {
            text: "문 안으로 들어서자, 작은 의자 하나가 놓여 있습니다. 당신은 그 의자에 앉아 무엇을 하시겠습니까?",
            choices: [
                { text: '조용히 주변을 둘러본다', next: 'result_C' },
                { text: '누군가 오기를 기다린다', next: 'result_A' },
                { text: '자리에서 일어난다', next: 'result_B' }
            ]
        }
    };

    // 게임 결과를 정의하는 객체
    const results = {
        'result_A': {
            title: '논리적인 해결사',
            description: "당신의 마음속에는 냉철하고 이성적인 '해결사'가 숨어 있습니다. 문제를 똑바로 마주하고, 계획을 세워 하나씩 해결해나가는 당신의 모습은 주변 사람들에게 든든한 존재입니다. #계획형인간 #냉정하지만따뜻해 #해결사",
        },
        'result_B': {
            title: '자유로운 예술가',
            description: "당신의 내면은 정해진 틀을 벗어나 자유롭게 유영하는 '예술가'입니다. 감정을 숨기기보다 솔직하게 표현하고, 새로운 경험과 영감을 찾아 떠나는 것을 두려워하지 않죠. #즉흥적 #감성충만 #예술가",
        },
        'result_C': {
            title: '고요한 관찰자',
            description: "당신의 마음속에는 세상의 모든 것을 조용히 바라보는 '관찰자'가 살고 있습니다. 화려하게 드러내기보다 홀로 사색하며 내면의 소리에 귀 기울이는 당신은 누구보다 깊고 풍부한 감성을 가졌습니다. #내향적 #사색가 #생각부자",
        }
    };

    // DOM 요소 선택
    const gameContent = document.getElementById('game-content');
    
    // 게임 상태를 렌더링하는 함수
    function renderGame() {
        gameContent.innerHTML = '';
        const state = currentStage;

        if (state === 'start') {
            const questionData = questions[state];
            const questionHtml = `
                <div class="start-page-content">
                    <h1 id="game-title" class="text-4xl font-extrabold text-gradient leading-tight mb-8">
                        인사이드 미<br><span class="text-2xl font-semibold tracking-wide" style="color: #6d28d9; opacity: 0.5;">INSIDE ME</span>
                    </h1>
                    <p class="text-2xl font-semibold mb-2 text-gray-700">${questionData.text}</p>
                    <p class="text-sm text-gray-500 mb-8">${questionData.subtext}</p>
                    <button
                        class="choice-btn w-full max-w-xs btn-gradient font-medium py-3 px-6 rounded-xl shadow-lg"
                        data-next-stage="${questionData.choices[0].next}"
                    >
                        ${questionData.choices[0].text}
                    </button>
                </div>
            `;
            gameContent.innerHTML = questionHtml;

            document.querySelectorAll('.choice-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    currentStage = e.target.dataset.nextStage;
                    renderGame();
                });
            });

        } else if (state in questions) {
            const questionData = questions[state];
            const questionHtml = `
                <h1 id="game-title" class="text-4xl font-extrabold text-gradient leading-tight mb-8">
                    인사이드 미<br><span class="text-2xl font-semibold tracking-wide" style="color: #6d28d9; opacity: 0.5;">INSIDE ME</span>
                </h1>
                <div class="relative w-full">
                    <span class="icon-small absolute icon-top-right icon-animated" style="animation-delay: 0.1s;">💖</span>
                    <span class="icon-small absolute icon-bottom-left icon-animated" style="animation-delay: 0.3s;">💬</span>
                    <p class="text-xl font-semibold mb-6 text-gray-700">${questionData.text}</p>
                </div>
                <div id="choices-container" class="flex flex-col space-y-4 w-full">
                    ${questionData.choices.map((choice, index) => `
                        <button
                            class="choice-btn w-full btn-gradient font-medium py-3 px-6 rounded-xl shadow-lg"
                            data-next-stage="${choice.next}"
                        >
                            ${choice.text}
                        </button>
                    `).join('')}
                </div>
            `;
            gameContent.innerHTML = questionHtml;

            document.querySelectorAll('.choice-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    currentStage = e.target.dataset.nextStage;
                    renderGame();
                });
            });

        } else if (state in results) {
            const resultData = results[state];
            const resultHtml = `
                <div class="result-container">
                    <h1 class="text-4xl font-extrabold text-gradient leading-tight mb-8">
                        인사이드 미
                    </h1>
                    <div class="result-box p-6 bg-white/50 rounded-xl border border-gray-200 relative w-full">
                        <span class="icon-small absolute icon-top-right icon-animated" style="animation-delay: 0.1s;">📸</span>
                        <span class="icon-small absolute icon-bottom-left icon-animated" style="animation-delay: 0.3s;">⭐</span>
                        <h2 class="text-2xl font-extrabold text-gradient mb-3">${resultData.title}</h2>
                        <p class="text-gray-800 mb-6 leading-relaxed">${resultData.description}</p>
                    </div>
                    <div class="qr-section">
                        <div class="flex-1 text-center">
                            <p class="text-sm text-gray-700 mb-2">
                                <span class="text-gradient font-bold">IS인별과 함께</span><br/>
                                <span class="text-gradient font-bold">또 다른 나</span>를 찾고 싶다면,<br/>
                                뮤지컬 <span class="text-gradient font-bold">인사이드 미!</span>
                            </p>
                        </div>
                        <div class="flex-none">
                            <img src="https://api.qrserver.com/v1/create-qr-code/?size=80x80&data=https://www.instagram.com/insideme_musical?igsh=MThuNmF6Z3oycHJjZg%3D%3D" alt="QR code to Instagram" class="w-20 h-20 rounded-xl" />
                        </div>
                    </div>
                    <button id="restart-btn" class="mt-4 btn-gradient font-semibold py-3 px-8 rounded-xl shadow-lg">
                        다시 시작하기
                    </button>
                </div>
            `;
            gameContent.innerHTML = resultHtml;

            document.getElementById('restart-btn').addEventListener('click', () => {
                currentStage = 'start';
                renderGame();
            });
        }
    }

    // 초기 게임 렌더링
    renderGame();
</script>

</body>
</html>
