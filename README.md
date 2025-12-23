# -<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>活力打工小转盘</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', 'Microsoft YaHei', sans-serif;
        }
        
        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            padding: 20px;
            color: #333;
            overflow-x: hidden;
        }
        
        .container {
            max-width: 800px;
            width: 100%;
            text-align: center;
            position: relative;
            z-index: 1;
        }
        
        header {
            margin-bottom: 30px;
        }
        
        h1 {
            color: #2c3e50;
            font-size: 2.8rem;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
        }
        
        .subtitle {
            color: #7f8c8d;
            font-size: 1.2rem;
            margin-bottom: 20px;
        }
        
        .wheel-container {
            position: relative;
            width: 420px;
            height: 420px;
            margin: 0 auto 40px;
        }
        
        .wheel {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            position: relative;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
            transition: transform 4s cubic-bezier(0.2, 0.8, 0.3, 1);
            transform: rotate(0deg);
        }
        
        .wheel-item {
            position: absolute;
            width: 50%;
            height: 50%;
            transform-origin: bottom right;
            left: 0;
            top: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            text-align: center;
            padding: 20px;
            transform: rotate(calc(var(--i) * 60deg));
        }
        
        .wheel-item-content {
            transform: rotate(calc(var(--i) * -60deg + 30deg));
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        
        .wheel-item i {
            font-size: 3.5rem;
            filter: drop-shadow(2px 2px 4px rgba(0,0,0,0.2));
        }
        
        .pointer {
            position: absolute;
            top: -20px;
            left: 50%;
            transform: translateX(-50%);
            width: 40px;
            height: 50px;
            z-index: 10;
        }
        
        .pointer:before {
            content: '';
            position: absolute;
            width: 0;
            height: 0;
            border-left: 20px solid transparent;
            border-right: 20px solid transparent;
            border-top: 30px solid #e74c3c;
            filter: drop-shadow(0 3px 3px rgba(0,0,0,0.2));
        }
        
        .center-circle {
            position: absolute;
            width: 90px;
            height: 90px;
            background: white;
            border-radius: 50%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 5;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: #2c3e50;
            border: 6px solid #f1c40f;
            font-size: 1.2rem;
            text-align: center;
            line-height: 1.3;
        }
        
        .spin-button {
            background: linear-gradient(to right, #3498db, #2980b9);
            color: white;
            border: none;
            padding: 18px 45px;
            font-size: 1.4rem;
            font-weight: bold;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 8px 20px rgba(52, 152, 219, 0.4);
            transition: all 0.3s ease;
            margin-top: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
        }
        
        .spin-button:hover {
            transform: translateY(-3px);
            box-shadow: 0 12px 25px rgba(52, 152, 219, 0.5);
        }
        
        .spin-button:active {
            transform: translateY(1px);
        }
        
        .spin-button:disabled {
            background: linear-gradient(to right, #95a5a6, #7f8c8d);
            cursor: not-allowed;
            transform: none;
            box-shadow: 0 5px 15px rgba(149, 165, 166, 0.4);
        }
        
        .spin-button i {
            font-size: 1.5rem;
        }
        
        .result-container {
            background: white;
            border-radius: 15px;
            padding: 25px;
            margin-top: 40px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.08);
            display: none;
            animation: fadeIn 0.5s ease;
        }
        
        .result-title {
            color: #2c3e50;
            font-size: 1.6rem;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        
        .result-content {
            font-size: 2.2rem;
            font-weight: bold;
            color: #e74c3c;
            margin-bottom: 15px;
            padding: 15px;
            background: #f8f9fa;
            border-radius: 10px;
            border-left: 6px solid #3498db;
        }
        
        .result-desc {
            color: #7f8c8d;
            font-size: 1.1rem;
            line-height: 1.6;
        }
        
        .history {
            margin-top: 30px;
            background: white;
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.08);
            width: 100%;
        }
        
        .history h3 {
            color: #2c3e50;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .history-list {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            justify-content: center;
        }
        
        .history-item {
            background: #ecf0f1;
            padding: 8px 16px;
            border-radius: 30px;
            font-size: 0.95rem;
            color: #34495e;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        /* 弹窗样式 */
        .popup-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.85);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            opacity: 0;
            visibility: hidden;
            transition: all 0.5s ease;
        }
        
        .popup-overlay.active {
            opacity: 1;
            visibility: visible;
        }
        
        .popup {
            background: white;
            border-radius: 20px;
            width: 90%;
            max-width: 500px;
            padding: 40px 30px;
            text-align: center;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.3);
            transform: translateY(30px) scale(0.9);
            transition: transform 0.5s ease;
            position: relative;
            overflow: hidden;
        }
        
        .popup-overlay.active .popup {
            transform: translateY(0) scale(1);
        }
        
        .popup:before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 10px;
            background: linear-gradient(to right, #3498db, #9b59b6, #e74c3c, #f1c40f, #2ecc71);
        }
        
        .popup-icon {
            font-size: 5rem;
            color: #f1c40f;
            margin-bottom: 20px;
            animation: bounce 2s infinite;
        }
        
        .popup-title {
            color: #2c3e50;
            font-size: 2.2rem;
            margin-bottom: 10px;
        }
        
        .popup-activity {
            font-size: 3rem;
            font-weight: bold;
            color: #e74c3c;
            margin: 20px 0;
            padding: 20px;
            background: linear-gradient(135deg, #f8f9fa, #e8f4fc);
            border-radius: 15px;
            border: 3px dashed #3498db;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
        }
        
        .popup-activity i {
            font-size: 2.5rem;
        }
        
        .popup-desc {
            color: #7f8c8d;
            font-size: 1.2rem;
            line-height: 1.6;
            margin-bottom: 30px;
        }
        
        .countdown {
            font-size: 1.5rem;
            color: #3498db;
            margin-bottom: 25px;
            font-weight: bold;
        }
        
        .close-popup {
            background: linear-gradient(to right, #2ecc71, #27ae60);
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 1.2rem;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 5px 15px rgba(46, 204, 113, 0.4);
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin: 0 auto;
        }
        
        .close-popup:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(46, 204, 113, 0.6);
        }
        
        /* 烟花样式 */
        .firework {
            position: fixed;
            width: 10px;
            height: 10px;
            border-radius: 50%;
            pointer-events: none;
            z-index: 999;
        }
        
        footer {
            margin-top: 40px;
            color: #7f8c8d;
            font-size: 0.9rem;
            text-align: center;
            padding-top: 20px;
            border-top: 1px solid #eee;
            width: 100%;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-20px); }
        }
        
        /* 转盘区域颜色 */
        .wheel-item:nth-child(1) { background: #3498db; } /* 爬楼梯去 */
        .wheel-item:nth-child(2) { background: #2ecc71; } /* 原地深蹲 */
        .wheel-item:nth-child(3) { background: #e74c3c; } /* 倒杯水喝 */
        .wheel-item:nth-child(4) { background: #f39c12; } /* 做一段锦 */
        .wheel-item:nth-child(5) { background: #9b59b6; } /* 伸个懒腰 */
        .wheel-item:nth-child(6) { background: #1abc9c; } /* 给花浇水 */
        
        /* 响应式设计 */
        @media (max-width: 600px) {
            .wheel-container {
                width: 340px;
                height: 340px;
            }
            
            h1 {
                font-size: 2.2rem;
            }
            
            .wheel-item i {
                font-size: 2.8rem;
            }
            
            .result-content {
                font-size: 1.8rem;
            }
            
            .popup-activity {
                font-size: 2.2rem;
                padding: 15px;
            }
            
            .popup-activity i {
                font-size: 2rem;
            }
            
            .popup-title {
                font-size: 1.8rem;
            }
            
            .popup-icon {
                font-size: 4rem;
            }
            
            .center-circle {
                width: 75px;
                height: 75px;
                font-size: 1rem;
            }
        }
        
        @media (max-width: 400px) {
            .wheel-container {
                width: 300px;
                height: 300px;
            }
            
            .wheel-item i {
                font-size: 2.5rem;
            }
            
            .popup {
                padding: 30px 20px;
            }
            
            .popup-activity {
                font-size: 1.8rem;
            }
            
            .center-circle {
                width: 70px;
                height: 70px;
                font-size: 0.9rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1><i class="fas fa-redo-alt"></i> 活力打工小转盘</h1>
            <p class="subtitle">工作间隙，抽个活动放松一下，为你的工作日注入活力</p>
        </header>
        
        <div class="wheel-container">
            <div class="pointer"></div>
            <div class="wheel" id="wheel">
                <!-- 转盘项将通过JavaScript动态生成 -->
            </div>
            <div class="center-circle">活力<br>转盘</div>
        </div>
        
        <button class="spin-button" id="spin-button">
            <i class="fas fa-play-circle"></i> 放松时间
        </button>
        
        <div class="result-container" id="result-container">
            <h2 class="result-title"><i class="fas fa-star"></i> 本次放松活动</h2>
            <div class="result-content" id="result-content">爬楼梯去</div>
            <p class="result-desc" id="result-desc">站起来活动一下，爬几层楼梯，促进血液循环，提升心率</p>
        </div>
        
        <div class="history">
            <h3><i class="fas fa-history"></i> 今日放松记录</h3>
            <div class="history-list" id="history-list">
                <!-- 历史记录将通过JavaScript动态生成 -->
            </div>
        </div>
        
        <footer>
            <p>活力打工小转盘 © 2023 | 设计用于工作间隙放松，每1-2小时使用一次效果更佳</p>
        </footer>
    </div>
    
    <!-- 弹窗 -->
    <div class="popup-overlay" id="popup-overlay">
        <div class="popup">
            <div class="popup-icon">
                <i class="fas fa-trophy"></i>
            </div>
            <h2 class="popup-title">放松时间到！</h2>
            <div class="popup-activity" id="popup-activity">
                <!-- 活动名称和图标将通过JavaScript动态生成 -->
            </div>
            <p class="popup-desc" id="popup-desc">站起来活动一下，爬几层楼梯，促进血液循环，提升心率</p>
            <div class="countdown" id="countdown">3秒后开始活动</div>
            <button class="close-popup" id="close-popup">
                <i class="fas fa-check-circle"></i> 开始活动
            </button>
        </div>
    </div>
    
    <!-- 烟花容器 -->
    <div id="fireworks-container"></div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // 转盘数据
            const activities = [
                { 
                    name: "爬楼梯去", 
                    icon: "fas fa-walking",
                    description: "站起来活动一下，爬几层楼梯，促进血液循环，提升心率"
                },
                { 
                    name: "原地深蹲", 
                    icon: "fas fa-running",
                    description: "做10-15个深蹲，锻炼腿部肌肉，增强核心力量"
                },
                { 
                    name: "倒杯水喝", 
                    icon: "fas fa-glass-whiskey",
                    description: "补充水分很重要，站起来走动一下，倒杯水慢慢喝"
                },
                { 
                    name: "做一段锦", 
                    icon: "fas fa-hands",
                    description: "做几分钟八段锦或简单的拉伸运动，舒缓肩颈压力"
                },
                { 
                    name: "伸个懒腰", 
                    icon: "fas fa-child",
                    description: "站起来伸个大大的懒腰，舒展身体，放松肌肉"
                },
                { 
                    name: "给花浇水", 
                    icon: "fas fa-leaf",
                    description: "照顾一下办公桌绿植，浇水整理，放松眼睛和心情"
                }
            ];
            
            const wheel = document.getElementById('wheel');
            const spinButton = document.getElementById('spin-button');
            const resultContainer = document.getElementById('result-container');
            const resultContent = document.getElementById('result-content');
            const resultDesc = document.getElementById('result-desc');
            const historyList = document.getElementById('history-list');
            const popupOverlay = document.getElementById('popup-overlay');
            const popupActivity = document.getElementById('popup-activity');
            const popupDesc = document.getElementById('popup-desc');
            const countdownEl = document.getElementById('countdown');
            const closePopupBtn = document.getElementById('close-popup');
            const fireworksContainer = document.getElementById('fireworks-container');
            
            let isSpinning = false;
            let spinCount = 0;
            let history = [];
            let countdownInterval;
            let currentRotation = 0;
            
            // 初始化转盘（只显示图标，不显示文字）
            function initWheel() {
                wheel.innerHTML = '';
                
                activities.forEach((activity, index) => {
                    const angle = 60 * index;
                    
                    const item = document.createElement('div');
                    item.className = 'wheel-item';
                    item.style.setProperty('--i', index);
                    item.style.transform = `rotate(${angle}deg)`;
                    
                    const content = document.createElement('div');
                    content.className = 'wheel-item-content';
                    
                    // 只创建图标，不创建文字
                    const icon = document.createElement('i');
                    icon.className = activity.icon;
                    
                    content.appendChild(icon);
                    item.appendChild(content);
                    wheel.appendChild(item);
                });
            }
            
            // 烟花效果
            function createFireworks() {
                fireworksContainer.innerHTML = '';
                
                for (let i = 0; i < 20; i++) {
                    setTimeout(() => {
                        createFirework();
                    }, i * 100);
                }
            }
            
            function createFirework() {
                const firework = document.createElement('div');
                firework.className = 'firework';
                
                const colors = ['#3498db', '#e74c3c', '#f1c40f', '#2ecc71', '#9b59b6', '#1abc9c'];
                const color = colors[Math.floor(Math.random() * colors.length)];
                firework.style.backgroundColor = color;
                
                const x = Math.random() * window.innerWidth;
                const y = Math.random() * window.innerHeight;
                firework.style.left = `${x}px`;
                firework.style.top = `${y}px`;
                
                fireworksContainer.appendChild(firework);
                
                const size = Math.random() * 30 + 10;
                firework.style.width = `${size}px`;
                firework.style.height = `${size}px`;
                
                firework.animate([
                    { transform: 'scale(0.1) rotate(0deg)', opacity: 1 },
                    { transform: `scale(1) rotate(${Math.random() * 360}deg)`, opacity: 0.8 },
                    { transform: 'scale(0.1) rotate(720deg)', opacity: 0 }
                ], {
                    duration: 1500,
                    easing: 'cubic-bezier(0.215, 0.610, 0.355, 1)'
                });
                
                setTimeout(() => {
                    if (firework.parentNode) {
                        firework.parentNode.removeChild(firework);
                    }
                }, 1500);
            }
            
            // 旋转转盘
            function spinWheel() {
                if (isSpinning) return;
                
                isSpinning = true;
                spinButton.disabled = true;
                spinButton.innerHTML = '<i class="fas fa-spinner fa-spin"></i> 转盘中...';
                
                // 随机选择结果
                const randomIndex = Math.floor(Math.random() * activities.length);
                const selectedActivity = activities[randomIndex];
                
                // 计算旋转角度
                const targetSegmentCenter = randomIndex * 60 + 30;
                const fullRotations = 5 * 360;
                const targetRotation = currentRotation + fullRotations + (360 - targetSegmentCenter);
                currentRotation = targetRotation % 360;
                
                // 应用旋转
                wheel.style.transform = `rotate(${targetRotation}deg)`;
                
                // 旋转结束后显示结果
                setTimeout(() => {
                    // 显示弹窗
                    showPopup(selectedActivity);
                    
                    // 显示下方结果区域
                    resultContent.textContent = selectedActivity.name;
                    resultDesc.textContent = selectedActivity.description;
                    resultContainer.style.display = 'block';
                    
                    // 添加到历史记录
                    addToHistory(selectedActivity);
                    
                    // 重置按钮状态
                    spinButton.innerHTML = '<i class="fas fa-play-circle"></i> 放松时间';
                    spinButton.disabled = false;
                    isSpinning = false;
                    spinCount++;
                }, 4000);
            }
            
            // 显示弹窗
            function showPopup(activity) {
                // 设置弹窗内容（包含图标和文字）
                popupActivity.innerHTML = `
                    <i class="${activity.icon}"></i>
                    <span>${activity.name}</span>
                `;
                popupDesc.textContent = activity.description;
                
                // 显示弹窗
                popupOverlay.classList.add('active');
                
                // 触发烟花效果
                createFireworks();
                
                // 开始倒计时
                startCountdown();
            }
            
            // 开始倒计时
            function startCountdown() {
                let countdown = 3;
                countdownEl.textContent = `${countdown}秒后开始活动`;
                
                clearInterval(countdownInterval);
                
                countdownInterval = setInterval(() => {
                    countdown--;
                    if (countdown > 0) {
                        countdownEl.textContent = `${countdown}秒后开始活动`;
                    } else {
                        countdownEl.textContent = "时间到！开始活动吧！";
                        clearInterval(countdownInterval);
                    }
                }, 1000);
            }
            
            // 关闭弹窗
            function closePopup() {
                popupOverlay.classList.remove('active');
                clearInterval(countdownInterval);
            }
            
            // 添加到历史记录
            function addToHistory(activity) {
                const now = new Date();
                const timeString = now.getHours().toString().padStart(2, '0') + ':' + 
                                  now.getMinutes().toString().padStart(2, '0');
                
                history.unshift({
                    name: activity.name,
                    icon: activity.icon,
                    time: timeString
                });
                
                // 只保留最近6条记录
                if (history.length > 6) {
                    history.pop();
                }
                
                updateHistoryDisplay();
            }
            
            // 更新历史记录显示
            function updateHistoryDisplay() {
                historyList.innerHTML = '';
                
                history.forEach(item => {
                    const historyItem = document.createElement('div');
                    historyItem.className = 'history-item';
                    historyItem.innerHTML = `
                        <i class="${item.icon}"></i>
                        <span>${item.name} (${item.time})</span>
                    `;
                    historyList.appendChild(historyItem);
                });
                
                // 如果没有历史记录，显示提示
                if (history.length === 0) {
                    const emptyMsg = document.createElement('div');
                    emptyMsg.className = 'history-item';
                    emptyMsg.textContent = '暂无记录，点击"放松时间"开始';
                    emptyMsg.style.opacity = '0.7';
                    historyList.appendChild(emptyMsg);
                }
            }
            
            // 事件监听
            spinButton.addEventListener('click', spinWheel);
            closePopupBtn.addEventListener('click', closePopup);
            
            // 点击弹窗背景也可以关闭
            popupOverlay.addEventListener('click', function(e) {
                if (e.target === this) {
                    closePopup();
                }
            });
            
            // 初始化
            initWheel();
            updateHistoryDisplay();
            
            // 初始显示一条示例结果
            setTimeout(() => {
                resultContainer.style.display = 'block';
            }, 1000);
        });
    </script>
</body>
</html>
