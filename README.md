# phonics-train.github.io
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Phonics Train - 自然拼读小火车 | 支持单词录音</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
        }

        body {
            background: linear-gradient(145deg, #7ec8e0 0%, #b5e2f7 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Comic Neue', 'Comic Sans MS', 'Chalkboard SE', 'Segoe UI Emoji', 'Quicksand', cursive, sans-serif;
            padding: 20px;
        }

        .cloud {
            position: fixed;
            background: rgba(255,255,245,0.9);
            border-radius: 1000px;
            z-index: 0;
            pointer-events: none;
        }
        .cloud1 { width: 100px; height: 60px; top: 20px; left: 5%; }
        .cloud1:before, .cloud1:after { content: ''; position: absolute; background: inherit; border-radius: 50%; }
        .cloud1:before { width: 50px; height: 50px; top: -25px; left: 15px; }
        .cloud1:after { width: 70px; height: 70px; top: -35px; left: 45px; }
        .cloud2 { width: 140px; height: 80px; bottom: 30px; right: 3%; }
        .cloud2:before, .cloud2:after { content: ''; position: absolute; background: inherit; border-radius: 50%; }
        .cloud2:before { width: 65px; height: 65px; top: -30px; left: 20px; }
        .cloud2:after { width: 85px; height: 85px; top: -40px; left: 70px; }

        .train-container {
            background: #fff9ef;
            border-radius: 80px 80px 70px 70px;
            box-shadow: 0 30px 40px rgba(0, 0, 0, 0.2);
            padding: 20px 25px 35px 25px;
            max-width: 1300px;
            width: 100%;
            position: relative;
            z-index: 2;
        }

        .track {
            background: repeating-linear-gradient(90deg, #9b6a3c, #9b6a3c 30px, #c9843e 30px, #c9843e 60px);
            height: 20px;
            border-radius: 30px;
            margin-bottom: 20px;
        }

        .train-stage {
            background: #f1e4ce;
            border-radius: 65px;
            padding: 25px 20px;
            margin-bottom: 20px;
            box-shadow: inset 0 0 0 5px #fffaf0, 0 15px 30px rgba(0, 0, 0, 0.15);
            position: relative;
        }

        .train-lineup {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
            flex-wrap: wrap;
            animation: gentleFloat 2s ease-in-out infinite;
        }

        @keyframes gentleFloat {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-3px); }
        }

        .engine {
            background: linear-gradient(135deg, #ffb347, #ff9f2c);
            border-radius: 50px 25px 25px 50px;
            min-width: 150px;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 14px 12px;
            box-shadow: 8px 8px 0 #b4621a;
            animation: trainBounce 1.2s ease-in-out infinite, trainWobble 3s infinite;
        }
        @keyframes trainBounce {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-5px); }
        }
        @keyframes trainWobble {
            0%, 100% { transform: rotate(0deg); }
            25% { transform: rotate(0.8deg); }
            75% { transform: rotate(-0.6deg); }
        }
        .engine:before {
            content: "🚂";
            font-size: 58px;
            position: absolute;
            left: -32px;
            top: 18px;
            animation: chugChug 0.5s infinite alternate;
        }
        @keyframes chugChug {
            from { transform: scale(1) translateX(0); }
            to { transform: scale(1.05) translateX(2px); }
        }
        .engine-face { font-size: 62px; }
        .engine-name {
            font-size: 22px;
            font-weight: bold;
            background: white;
            padding: 5px 16px;
            border-radius: 50px;
            color: #c25d1a;
        }
        .wheel-deco { display: flex; gap: 12px; margin-top: 8px; }
        .wheel {
            width: 24px; height: 24px;
            background: #2f2a24;
            border-radius: 50%;
            border: 2px solid #c9a87b;
        }

        .current-carriage {
            background: #fff6ea;
            border-radius: 55px;
            min-width: 500px;
            text-align: center;
            padding: 20px;
            box-shadow: 0 12px 0 #c29a5e;
            animation: carriageSway 2.5s ease-in-out infinite;
        }
        @keyframes carriageSway {
            0%, 100% { transform: rotate(0deg); }
            50% { transform: rotate(0.3deg); }
        }

        .image-area { margin-bottom: 15px; }
        .word-image {
            width: 100px; height: 100px;
            background: #f9e8d0;
            border-radius: 35px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            font-size: 60px;
            box-shadow: 0 8px 15px rgba(0,0,0,0.1);
        }

        .word-header {
            cursor: pointer;
            padding: 12px;
            border-radius: 50px;
            background: #fffaef;
            margin-bottom: 20px;
            border: 2px solid #ffe0b5;
            transition: all 0.2s;
            position: relative;
        }
        .word-header:hover { background: #fff3e0; transform: scale(0.98); }
        .full-word {
            font-size: 58px;
            font-weight: bold;
            letter-spacing: 8px;
            color: #4a2a12;
            text-shadow: 3px 3px 0 #ffd89a;
        }
        .read-hint { font-size: 13px; color: #d98c2b; margin-top: 6px; }
        .audio-badge {
            position: absolute;
            right: 15px;
            top: 15px;
            font-size: 20px;
        }

        .phonics-container {
            display: flex;
            justify-content: center;
            gap: 12px;
            flex-wrap: wrap;
            margin: 15px 0;
        }
        .sound-card {
            background: linear-gradient(145deg, #fbe9ce, #f7e0be);
            min-width: 80px;
            padding: 10px 16px;
            border-radius: 40px;
            cursor: pointer;
            box-shadow: 0 6px 0 #b68b54;
            transition: all 0.1s;
            text-align: center;
            position: relative;
        }
        .sound-card:active { transform: translateY(3px); box-shadow: 0 2px 0 #b68b54; }
        .sound-text { font-size: 36px; font-weight: bold; font-family: monospace; color: #6b3f1a; }
        .sound-label { font-size: 11px; background: #e6c894; padding: 2px 10px; border-radius: 30px; margin-top: 4px; }

        .letter-card {
            background: linear-gradient(145deg, #fbe9ce, #f7e0be);
            width: 80px;
            height: 80px;
            border-radius: 35px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: 0 6px 0 #b68b54;
            transition: all 0.1s;
            position: relative;
        }
        .letter-card:active { transform: translateY(3px); box-shadow: 0 2px 0 #b68b54; }
        .letter { font-size: 48px; font-weight: bold; font-family: monospace; color: #6b3f1a; }
        .letter-badge {
            font-size: 9px;
            position: absolute;
            bottom: 4px;
            right: 6px;
            opacity: 0.6;
        }

        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin: 15px 0;
        }
        .nav-btn {
            background: #ffcd7e;
            border: none;
            font-size: 1.2rem;
            padding: 8px 24px;
            border-radius: 50px;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0 5px 0 #aa6f3c;
        }
        .nav-btn:active { transform: translateY(2px); box-shadow: 0 2px 0 #aa6f3c; }
        .counter {
            background: #e6c38a;
            border-radius: 50px;
            padding: 8px 20px;
            font-weight: bold;
            font-size: 1.1rem;
        }
        .control-bar {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 10px;
        }
        .speaker-btn {
            background: #ffbd5a;
            border: none;
            padding: 8px 20px;
            border-radius: 40px;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0 4px 0 #9b6234;
        }
        .speaker-btn:active { transform: translateY(2px); box-shadow: 0 2px 0 #9b6234; }
        .info {
            background: #f9efdb;
            border-radius: 40px;
            padding: 10px 15px;
            text-align: center;
            font-size: 0.8rem;
            margin-top: 15px;
            color: #6b4a28;
        }
        .whistle { position: absolute; right: 20px; top: -5px; font-size: 28px; }
        .highlight {
            animation: flash 0.25s ease-out;
        }
        @keyframes flash {
            0% { background-color: #fff0cc; }
            50% { background-color: #ffdd99; box-shadow: 0 0 0 6px rgba(255,200,100,0.5); }
            100% { background-color: inherit; }
        }
        @media (max-width: 750px) {
            .letter-card { width: 65px; height: 65px; }
            .letter { font-size: 38px; }
            .sound-text { font-size: 28px; }
            .full-word { font-size: 42px; letter-spacing: 4px; }
            .current-carriage { min-width: 320px; }
        }
    </style>
</head>
<body>
<div class="cloud cloud1"></div>
<div class="cloud cloud2"></div>

<div class="train-container">
    <div class="track"></div>
    <div class="train-stage">
        <div class="train-lineup">
            <div class="engine">
                <div class="engine-face">🚂</div>
                <div class="engine-name">Phonics Train</div>
                <div class="wheel-deco"><div class="wheel"></div><div class="wheel"></div></div>
            </div>
            <div class="current-carriage" id="currentCarriage">
                <div class="image-area"><div class="word-image" id="wordImage">🗺️</div></div>
                <div class="word-header" id="wordHeader">
                    <div class="full-word" id="fullWordDisplay">map</div>
                    <div class="read-hint">🎵 点击听完整单词发音</div>
                    <div class="audio-badge" id="audioBadge">📀</div>
                </div>
                <div class="phonics-container" id="phonicsContainer"></div>
                <div class="read-hint">✨ 点击字母/音节卡片听拼读发音 | 单词录音放在 audio/words/ 文件夹 ✨</div>
            </div>
        </div>
        <div class="whistle">🎵</div>
    </div>

    <div class="nav-buttons">
        <button class="nav-btn" id="prevBtn">◀ 上一节</button>
        <div class="counter" id="counterDisplay">🐻 1 / 17</div>
        <button class="nav-btn" id="nextBtn">下一节 ▶</button>
    </div>
    <div class="control-bar">
        <button class="speaker-btn" id="repeatWordBtn">🔊 重读当前单词</button>
        <button class="speaker-btn" id="tourAllBtn">🚂 火车巡游</button>
    </div>
    <div class="info">
        🎈 <strong>支持单词录音版</strong> 🎈<br>
        • <strong>单词录音</strong>：放入 <strong>audio/words/单词名.mp3</strong> (如 map.mp3)<br>
        • <strong>字母/音节录音</strong>：放入 <strong>audio/字母.mp3</strong> (如 a.mp3, an.mp3)<br>
        • 如无音频文件，自动使用TTS发音<br>
        • 点击大单词区域 → 播放完整单词录音 | 点击字母卡片 → 播放字母拼读
    </div>
</div>

<script>
    // 单词列表及图片
    const wordsWithImages = [
        { word: "map", emoji: "🗺️" }, { word: "mad", emoji: "😠" }, { word: "mom", emoji: "👩‍👧" },
        { word: "mat", emoji: "🧘" }, { word: "man", emoji: "👨" }, { word: "nap", emoji: "😴" },
        { word: "net", emoji: "🥅" }, { word: "not", emoji: "🚫" }, { word: "nip", emoji: "🦀" },
        { word: "box", emoji: "📦" }, { word: "dog", emoji: "🐕" }, { word: "top", emoji: "🔝" },
        { word: "hot", emoji: "🌶️" }, { word: "pan", emoji: "🍳" }, { word: "pat", emoji: "🤚" },
        { word: "pop", emoji: "🍿" }, { word: "pin", emoji: "📌" }
    ];
    
    // 需要音频的字母和音节
    const audioKeys = ['a', 'b', 'd', 'e', 'g', 'h', 'i', 'm', 'n', 'o', 'p', 't', 'x', 'an', 'in'];
    
    let currentIndex = 0;
    
    // ==================== 音频播放函数 ====================
    
    // 播放单词录音（优先使用 audio/words/单词.mp3）
    async function playWordAudio(word, elementToHighlight = null) {
        if (!word) return;
        
        if (elementToHighlight) {
            elementToHighlight.classList.add('highlight');
            setTimeout(() => elementToHighlight.classList.remove('highlight'), 300);
        }
        
        const audioUrl = `audio/words/${word}.mp3`;
        const audio = new Audio(audioUrl);
        
        return new Promise((resolve) => {
            audio.onended = () => resolve();
            audio.onerror = () => {
                // 单词录音不存在，降级使用 TTS
                playTTS(word, 0.75).then(resolve);
            };
            audio.play().catch(() => {
                playTTS(word, 0.75).then(resolve);
            });
        });
    }
    
    // 播放字母/音节发音
    async function playPhonemeSound(key, elementToHighlight = null) {
        if (!key) return;
        
        if (elementToHighlight) {
            elementToHighlight.classList.add('highlight');
            setTimeout(() => elementToHighlight.classList.remove('highlight'), 250);
        }
        
        const audioUrl = `audio/${key}.mp3`;
        const audio = new Audio(audioUrl);
        
        return new Promise((resolve) => {
            audio.onended = () => resolve();
            audio.onerror = () => {
                // 音频文件不存在，降级使用 TTS
                playTTS(key, 0.68).then(resolve);
            };
            audio.play().catch(() => {
                playTTS(key, 0.68).then(resolve);
            });
        });
    }
    
    function playTTS(text, rate = 0.7) {
        return new Promise((resolve) => {
            const utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = 'en-US';
            utterance.rate = rate;
            utterance.pitch = 1.0;
            utterance.onend = () => resolve();
            utterance.onerror = () => resolve();
            window.speechSynthesis.speak(utterance);
            setTimeout(() => resolve(), 1200);
        });
    }
    
    function delay(ms) { return new Promise(r => setTimeout(r, ms)); }
    
    // 播放完整单词（优先使用单词录音）
    async function speakFullWord(word, element) {
        window.speechSynthesis.cancel();
        await delay(30);
        await playWordAudio(word, element);
    }
    
    // 解析单词片段（识别 an 和 in 音节）
    function parseWordIntoSegments(word) {
        const lower = word.toLowerCase();
        const segments = [];
        let i = 0;
        while (i < lower.length) {
            if (i + 1 < lower.length) {
                const two = lower.substr(i, 2);
                if (two === 'an' || two === 'in') {
                    segments.push({ type: 'syllable', text: two });
                    i += 2;
                    continue;
                }
            }
            segments.push({ type: 'letter', text: lower[i] });
            i++;
        }
        return segments;
    }
    
    // 检查音频文件是否存在（用于显示徽章）
    function checkAudioExists(key, type = 'phoneme') {
        // 实际检查需要异步，这里简化处理，显示默认徽章
        return false;
    }
    
    // ==================== UI 更新 ====================
    function updateDisplay() {
        const data = wordsWithImages[currentIndex];
        const word = data.word;
        document.getElementById('fullWordDisplay').innerText = word;
        document.getElementById('wordImage').innerHTML = data.emoji;
        document.getElementById('counterDisplay').innerHTML = `🐻 ${currentIndex+1} / ${wordsWithImages.length}`;
        
        const segments = parseWordIntoSegments(word);
        const container = document.getElementById('phonicsContainer');
        container.innerHTML = '';
        
        segments.forEach(seg => {
            const card = document.createElement('div');
            card.className = seg.type === 'syllable' ? 'sound-card' : 'letter-card';
            const textSpan = document.createElement('div');
            textSpan.className = seg.type === 'syllable' ? 'sound-text' : 'letter';
            textSpan.innerText = seg.text;
            card.appendChild(textSpan);
            
            if (seg.type === 'syllable') {
                const label = document.createElement('div');
                label.className = 'sound-label';
                label.innerText = '🔊 音节';
                card.appendChild(label);
            } else {
                const label = document.createElement('div');
                label.className = 'sound-label';
                if (seg.text === 'a') label.innerText = '/æ/';
                else if (seg.text === 'p') label.innerText = '/p/';
                else if (seg.text === 'i') label.innerText = '/ɪ/';
                else label.innerText = '🔊';
                card.appendChild(label);
            }
            
            card.addEventListener('click', async (e) => {
                e.stopPropagation();
                await playPhonemeSound(seg.text, card);
            });
            container.appendChild(card);
        });
        
        // 重新绑定单词头部
        const header = document.getElementById('wordHeader');
        const newHeader = header.cloneNode(true);
        header.parentNode.replaceChild(newHeader, header);
        newHeader.addEventListener('click', async () => {
            const carriage = document.getElementById('currentCarriage');
            await speakFullWord(word, carriage);
        });
    }
    
    // ==================== 导航 ====================
    function prevWord() {
        currentIndex = (currentIndex - 1 + wordsWithImages.length) % wordsWithImages.length;
        updateDisplay();
        window.speechSynthesis.cancel();
    }
    function nextWord() {
        currentIndex = (currentIndex + 1) % wordsWithImages.length;
        updateDisplay();
        window.speechSynthesis.cancel();
    }
    async function repeatCurrentWord() {
        const word = wordsWithImages[currentIndex].word;
        const carriage = document.getElementById('currentCarriage');
        await speakFullWord(word, carriage);
    }
    async function tourAllWords() {
        window.speechSynthesis.cancel();
        await delay(100);
        const total = wordsWithImages.length;
        const startIdx = currentIndex;
        for (let i = 0; i < total; i++) {
            const idx = (startIdx + i) % total;
            const word = wordsWithImages[idx].word;
            currentIndex = idx;
            updateDisplay();
            const carriage = document.getElementById('currentCarriage');
            await speakFullWord(word, carriage);
            await delay(500);
        }
        await playTTS("All done! Choo choo!", 0.85);
    }
    
    // 绑定按钮
    function bindButtons() {
        const prev = document.getElementById('prevBtn');
        const next = document.getElementById('nextBtn');
        const repeat = document.getElementById('repeatWordBtn');
        const tour = document.getElementById('tourAllBtn');
        
        const newPrev = prev.cloneNode(true);
        prev.parentNode.replaceChild(newPrev, prev);
        newPrev.addEventListener('click', prevWord);
        
        const newNext = next.cloneNode(true);
        next.parentNode.replaceChild(newNext, next);
        newNext.addEventListener('click', nextWord);
        
        const newRepeat = repeat.cloneNode(true);
        repeat.parentNode.replaceChild(newRepeat, repeat);
        newRepeat.addEventListener('click', repeatCurrentWord);
        
        const newTour = tour.cloneNode(true);
        tour.parentNode.replaceChild(newTour, tour);
        newTour.addEventListener('click', async () => {
            window.speechSynthesis.cancel();
            await delay(50);
            tourAllWords();
        });
    }
    
    // 初始化
    function init() {
        updateDisplay();
        bindButtons();
        window.addEventListener('beforeunload', () => {
            if (window.speechSynthesis) window.speechSynthesis.cancel();
        });
    }
    
    if (document.readyState === 'loading') {
        document.addEventListener('DOMContentLoaded', init);
    } else {
        init();
    }
</script>
</body>
</html>
