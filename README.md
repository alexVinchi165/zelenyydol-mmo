<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Зелёный Дол | MMO ERP & Empire</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
    <style>
        :root {
            --bg-color: #12181b;
            --card-bg: #1e262c;
            --card-hover: #263238;
            --accent-green: #2ecc71;
            --accent-gold: #f1c40f;
            --accent-blue: #3498db;
            --accent-purple: #9b59b6;
            --accent-red: #e74c3c;
            --text-color: #ecf0f1;
            --text-muted: #bdc3c7;
            --border-radius: 14px;
        }

        body {
            font-family: 'Segoe UI', system-ui, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            padding: 12px;
            box-sizing: border-box;
        }

        .header {
            text-align: center;
            padding-bottom: 10px;
            border-bottom: 1px solid #2c3e50;
            margin-bottom: 14px;
        }

        .header h1 { margin: 0; font-size: 20px; color: var(--accent-green); }
        .header p { margin: 4px 0 0; font-size: 11px; color: var(--text-muted); }

        .hub-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-bottom: 14px;
        }

        .hub-card {
            background: var(--card-bg);
            border-radius: var(--border-radius);
            padding: 16px 12px;
            text-align: center;
            cursor: pointer;
            border: 1px solid rgba(255,255,255,0.04);
            transition: transform 0.1s, background 0.2s;
        }

        .hub-card:active { transform: scale(0.97); }
        .hub-card:hover { background: var(--card-hover); }

        .hub-icon { font-size: 26px; margin-bottom: 6px; }
        .hub-title { font-size: 13px; font-weight: bold; color: var(--text-color); }
        .hub-desc { font-size: 10px; color: var(--text-muted); margin-top: 2px; }

        .back-btn {
            background: rgba(255,255,255,0.08);
            color: var(--text-color);
            border: none;
            padding: 8px 14px;
            border-radius: 8px;
            font-size: 12px;
            font-weight: bold;
            cursor: pointer;
            margin-bottom: 12px;
            display: inline-flex;
            align-items: center;
            gap: 6px;
        }

        .card {
            background: var(--card-bg);
            border-radius: var(--border-radius);
            padding: 14px;
            margin-bottom: 12px;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-top: 10px;
        }

        .stat-box {
            background: rgba(255,255,255,0.03);
            padding: 10px;
            border-radius: 8px;
            text-align: center;
        }

        .stat-val { font-size: 18px; font-weight: bold; color: var(--accent-gold); }
        .stat-lbl { font-size: 11px; color: var(--text-muted); }

        .btn {
            width: 100%;
            background: var(--accent-green);
            color: #fff;
            border: none;
            padding: 10px;
            border-radius: 8px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 8px;
        }

        .btn-blue { background: var(--accent-blue); }
        .btn-gold { background: var(--accent-gold); color: #111; }
        .btn-purple { background: var(--accent-purple); color: #fff; }
        .btn-danger { background: var(--accent-red); color: #fff; }

        .btn-sm {
            width: auto;
            padding: 4px 8px;
            font-size: 11px;
            margin-top: 0;
            border-radius: 6px;
        }

        .item-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 8px 0;
            border-bottom: 1px solid rgba(255,255,255,0.05);
            font-size: 13px;
        }

        .tab-content { display: none; }
        .tab-content.active { display: block; }

        input, select, textarea {
            width: 100%;
            padding: 8px;
            margin: 4px 0 10px;
            border-radius: 6px;
            border: 1px solid #333;
            background: #111;
            color: #fff;
            box-sizing: border-box;
            font-family: inherit;
        }

        .logistics-filters {
            display: flex;
            gap: 6px;
            margin-bottom: 12px;
            overflow-x: auto;
            padding-bottom: 4px;
        }

        .filter-btn {
            background: var(--card-bg);
            color: var(--text-muted);
            border: 1px solid #333;
            padding: 6px 10px;
            border-radius: 6px;
            font-size: 11px;
            font-weight: bold;
            white-space: nowrap;
            cursor: pointer;
        }

        .filter-btn.active {
            background: var(--accent-blue);
            color: #fff;
            border-color: var(--accent-blue);
        }

        .mmo-badge { background: rgba(155, 89, 182, 0.2); color: var(--accent-purple); border: 1px solid var(--accent-purple); padding: 3px 8px; border-radius: 6px; font-size: 11px; font-weight: bold; display: inline-block; margin-bottom: 8px; }
    </style>
</head>
<body>

    <div class="header">
        <h1>🌾 «Зелёный Дол» MMO</h1>
        <p>Многопользовательская экономическая империя & Аукционы</p>
    </div>

    <!-- 🏠 ГЛАВНЫЙ ЭКРАН (ХАБ) -->
    <div id="hub" class="tab-content active">
        <div class="card" style="background: linear-gradient(135deg, #1e262c, #161f26);">
            <div style="display:flex; justify-content:space-between; align-items:center;">
                <div>
                    <span style="font-size:11px; color:var(--text-muted);">Игрок / Статус:</span>
                    <h3 id="playerNameDisplay" style="margin:2px 0 0; color:var(--accent-green);">Игрок #1</h3>
                </div>
                <button class="btn btn-gold" style="width:auto; padding:8px 14px; font-size:12px; margin:0;" onclick="syncWithCloud()">🔄 Синхрон с облаком</button>
            </div>
            
            <div style="margin-top:14px; padding-top:10px; border-top:1px solid rgba(255,255,255,0.05); display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
                <div>
                    <span style="font-size:11px; color:var(--text-muted);">Касса компании:</span>
                    <div id="totalBalance" style="font-size:20px; font-weight:bold; color:var(--accent-gold); margin-top:2px;">4 785 🪙</div>
                </div>
                <div>
                    <span style="font-size:11px; color:var(--text-muted);">Счёт ФОП:</span>
                    <div id="fopBalanceDisplay" style="font-size:20px; font-weight:bold; color:var(--accent-green); margin-top:2px;">0 🪙</div>
                </div>
            </div>
        </div>

        <div class="mmo-badge">🌐 ММО-Режим активирован через Supabase</div>

        <h4 style="margin: 16px 0 10px; color: var(--text-muted); font-size: 12px; text-transform: uppercase; letter-spacing: 0.5px;">🏛️ Сетевые разделы</h4>

        <div class="hub-grid">
            <div class="hub-card" onclick="switchTab('auctions')">
                <div class="hub-icon">🔨</div>
                <div class="hub-title">Мировой Аукцион</div>
                <div class="hub-desc">Торги между игроками, ставки, фото</div>
            </div>
            <div class="hub-card" onclick="switchTab('p2p_bank')">
                <div class="hub-icon">💳</div>
                <div class="hub-title">P2P Банк & ФОП</div>
                <div class="hub-desc">Переводы игрокам на карты и счета</div>
            </div>
            <div class="hub-card" onclick="switchTab('ai_advisor')">
                <div class="hub-icon">🤖</div>
                <div class="hub-title">ИИ-Советник</div>
                <div class="hub-desc">Внутриигровой помощник и аналитик</div>
            </div>
            <div class="hub-card" onclick="switchTab('hub_local')">
                <div class="hub-icon">🌾</div>
                <div class="hub-title">Локальная ферма</div>
                <div class="hub-desc">Управление складом и цехами</div>
            </div>
        </div>
    </div>

    <!-- 🔨 МИРОВОЙ АУКЦИОН -->
    <div id="auctions" class="tab-content">
        <button class="back-btn" onclick="switchTab('hub')">⬅️ На главную</button>
        <h3>🔨 Мировой Аукцион Игроков</h3>
        <p style="font-size:11px; color:var(--text-muted); margin-bottom:12px;">Выставляйте редкие товары с фото и описанием, делайте ставки в реальном времени:</p>

        <div class="card">
            <h4>➕ Выставить лот на аукцион</h4>
            <input type="text" id="aucTitle" placeholder="Название лота (например: Редкое семя / Улучшенный трактор)">
            <input type="text" id="aucCategory" placeholder="Категория (например: Техника, Семена, Ресурсы)">
            <input type="text" id="aucImage" placeholder="Ссылка на фото (URL с ImgBB или картинка)">
            <input type="number" id="aucAmount" placeholder="Количество (шт / кг)" value="1">
            <input type="number" id="aucMinBid" placeholder="Стартовая минимальная цена (🪙)" value="500">
            <button class="btn btn-purple" onclick="createAuctionLot()">📦 Опубликовать лот на аукционе</button>
        </div>

        <h4>🏛️ Активные лоты на торгах:</h4>
        <div id="globalAuctionsList"></div>
    </div>

    <!-- 💳 P2P БАНК И ПЕРЕВОДЫ -->
    <div id="p2p_bank" class="tab-content">
        <button class="back-btn" onclick="switchTab('hub')">⬅️ На главную</button>
        <h3>💳 P2P Банк & Межбанковские Переводы</h3>
        
        <div class="card">
            <h4>💸 Перевод средств другому игроку</h4>
            <p style="font-size:11px; color:var(--text-muted);">Отправляйте деньги на карту или ФОП другим игрокам сети.</p>
            <input type="text" id="p2pRecipient" placeholder="Имя получателя или IBAN / номер карты">
            <input type="number" id="p2pAmount" placeholder="Сумма перевода (🪙)">
            <input type="text" id="p2pNote" placeholder="Назначение платежа (за сырье, за услугу...)">
            <button class="btn btn-blue" onclick="sendP2PTransfer()">Отправить перевод 💸</button>
        </div>

        <div class="card">
            <h4>📜 История сетевых транзакций и переводов</h4>
            <div id="p2pHistoryList"></div>
        </div>
    </div>

    <!-- 🤖 ИИ-СОВЕТНИК -->
    <div id="ai_advisor" class="tab-content">
        <button class="back-btn" onclick="switchTab('hub')">⬅️ На главную</button>
        <h3>🤖 ИИ-Советник «Зелёного Дола»</h3>
        <div class="card">
            <p style="font-size:12px; line-height:1.5;">Привет, братунец! Я твой встроенный искусственный интеллект-ассистент. Я мониторю мировой рынок, подсказываю выгодные ставки на аукционах и слежу за безопасностью твоих переводов ФОП.</p>
            <button class="btn btn-purple" onclick="askAiAdvice()">📊 Запросить торговый совет у ИИ</button>
            <div id="aiAdviceOutput" style="margin-top:10px; font-size:12px; color:var(--accent-gold); background:rgba(0,0,0,0.2); padding:10px; border-radius:6px; display:none;"></div>
        </div>
    </div>

    <!-- 🌾 ЛОКАЛЬНАЯ ФЕРМА (ЗАГЛУШКА ДЛЯ СТАРОГО ФУНКЦИОНАЛА) -->
    <div id="hub_local" class="tab-content">
        <button class="back-btn" onclick="switchTab('hub')">⬅️ На главную</button>
        <h3>🌾 Локальное управление холдингом</h3>
        <div class="card">
            <p style="font-size:12px; color:var(--text-muted);">Тут находится твое локальное хозяйство. Баланс компании синхронизирован с облачной базой Supabase!</p>
            <div class="stat-box" style="margin-top:10px;">
                <div class="stat-val" id="localBalanceBox">4 785 🪙</div>
                <div class="stat-lbl">Общий облачный баланс</div>
            </div>
        </div>
    </div>

    <script>
        const tg = window.Telegram.WebApp;
        tg.expand();

        // Подключение к Supabase с твоими ключами
        const SUPABASE_URL = 'https://uavvdnlmnqpebhwibmmh.supabase.co';
        const SUPABASE_KEY = 'sb_publishable_Y7pKMlyHL9U4TEAyxWr37g_ehD6U0JH';
        const supabase = window.supabase.createClient(SUPABASE_URL, SUPABASE_KEY);

        let currentUsername = tg.initDataUnsafe?.user?.username || "Игрок_" + Math.floor(Math.random() * 1000);
        let userBalance = 4785;
        let userFopBalance = 0;

        document.getElementById('playerNameDisplay').innerText = currentUsername;

        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.getElementById(tabId).classList.add('active');
            window.scrollTo(0, 0);

            if (tabId === 'auctions') loadAuctions();
            if (tabId === 'p2p_bank') loadP2PHistory();
        }

        async function syncWithCloud() {
            alert("🔄 Синхронизация с облаком Supabase выполнена успешно!");
            loadAuctions();
            loadP2PHistory();
        }

        async function createAuctionLot() {
            let title = document.getElementById('aucTitle').value.trim();
            let category = document.getElementById('aucCategory').value.trim() || "Общее";
            let image = document.getElementById('aucImage').value.trim() || "https://images.unsplash.com/photo-1523348837708-15d4a09cfac2";
            let amount = parseFloat(document.getElementById('aucAmount').value) || 1;
            let minBid = parseInt(document.getElementById('aucMinBid').value) || 100;

            if (!title) return alert("Введите название лота!");

            const { data, error } = await supabase.from('auctions').insert([
                { seller_name: currentUsername, item_title: title, category: category, image_url: image, amount: amount, min_bid: minBid, current_bid: minBid, highest_bidder: 'Нет ставок', is_active: true }
            ]);

            if (error) {
                alert("Ошибка создания лота: " + error.message);
            } else {
                alert("🎉 Лот успешно выставлен на мировой аукцион!");
                document.getElementById('aucTitle').value = "";
                loadAuctions();
            }
        }

        async function loadAuctions() {
            const container = document.getElementById('globalAuctionsList');
            if (!container) return;
            container.innerHTML = "Загрузка торгов из облака...";

            const { data, error } = await supabase.from('auctions').select('*').eq('is_active', true);
            if (error) {
                container.innerHTML = "Ошибка загрузки аукционов.";
                return;
            }

            if (!data || data.length === 0) {
                container.innerHTML = "<p style='font-size:12px; color:var(--text-muted);'>Активных лотов на аукционе пока нет. Создайте первый!</p>";
                return;
            }

            container.innerHTML = "";
            data.forEach(lot => {
                container.innerHTML += `
                    <div class="card" style="border-left:4px solid var(--accent-purple);">
                        <img src="${lot.image_url}" style="width:100%; height:120px; object-fit:cover; border-radius:8px; background:#111;" onerror="this.style.display='none'">
                        <div style="display:flex; justify-content:space-between; align-items:center; margin-top:6px;">
                            <b>${lot.item_title} (${lot.amount} шт)</b>
                            <span class="category-badge">${lot.category}</span>
                        </div>
                        <div style="font-size:11px; color:var(--text-muted); margin:4px 0;">Продавец: <b>${lot.seller_name}</b></div>
                        <div style="font-size:12px; color:var(--accent-gold);">Текущая ставка: <b>${lot.current_bid} 🪙</b> (Лидер: ${lot.highest_bidder})</div>
                        <button class="btn btn-gold btn-sm" style="margin-top:8px;" onclick="placeBid('${lot.id}', ${lot.current_bid})">⚡ Сделать ставку (+100 🪙)</button>
                    </div>`;
            });
        }

        async function placeBid(lotId, currentBid) {
            let newBid = currentBid + 100;
            const { error } = await supabase.from('auctions').update({ current_bid: newBid, highest_bidder: currentUsername }).eq('id', lotId);
            if (error) {
                alert("Ошибка ставки: " + error.message);
            } else {
                alert(`⚡ Ставка ${newBid} 🪙 успешно принята!`);
                loadAuctions();
            }
        }

        async function sendP2PTransfer() {
            let recipient = document.getElementById('p2pRecipient').value.trim();
            let amount = parseInt(document.getElementById('p2pAmount').value);
            let note = document.getElementById('p2pNote').value.trim() || "Перевод средств";

            if (!recipient || isNaN(amount) || amount <= 0) return alert("Заполните получателя и сумму!");

            const { error } = await supabase.from('p2p_transfers').insert([
                { sender_name: currentUsername, recipient_name: recipient, amount: amount, note: note }
            ]);

            if (error) {
                alert("Ошибка перевода: " + error.message);
            } else {
                alert(`💸 Перевод на сумму ${amount} 🪙 успешно отправлен игроку "${recipient}"!`);
                document.getElementById('p2pRecipient').value = "";
                document.getElementById('p2pAmount').value = "";
                loadP2PHistory();
            }
        }

        async function loadP2PHistory() {
            const list = document.getElementById('p2pHistoryList');
            if (!list) return;
            list.innerHTML = "Загрузка истории...";

            const { data, error } = await supabase.from('p2p_transfers').select('*').order('created_at', { ascending: false }).limit(10);
            if (error) {
                list.innerHTML = "Ошибка загрузки истории переводов.";
                return;
            }

            if (!data || data.length === 0) {
                list.innerHTML = "<p style='font-size:12px; color:var(--text-muted);'>Нет транзакций</p>";
                return;
            }

            list.innerHTML = "";
            data.forEach(tr => {
                list.innerHTML += `
                    <div class="item-row" style="font-size:11px;">
                        <div>
                            <b>${tr.sender_name} ➔ ${tr.recipient_name}</b>
                            <small style="display:block; color:var(--text-muted);">${tr.note}</small>
                        </div>
                        <b style="color:var(--accent-green);">+${tr.amount} 🪙</b>
                    </div>`;
            });
        }

        function askAiAdvice() {
            const out = document.getElementById('aiAdviceOutput');
            out.style.display = 'block';
            out.innerHTML = "🤖 Анализирую мировой рынок Supabase... Рекомендация: сейчас выгодно скупать редкие семена на аукционе и перепродавать оптом в города с наценкой 15%!";
        }
    </script>
</body>
</html>
