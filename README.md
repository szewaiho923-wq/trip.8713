<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>雪國旅記 | 深度指南</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- PWA 配置 -->
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="雪國旅記">
    <link rel="apple-touch-icon" href="chiikawa-icon.png">
    
    <style>
        :root {
            /* === 富士藍主題 === */
            --fuji-blue: #5B8FB9;       
            --bg-color: #F8F9FA;        
            --card-bg: #FFFFFF;         
            --text-main: #2C3E50;       
            --text-sub: #5D6D7E;        
            --line-color: #D6E4F0;      
            
            /* 分類顏色 (用於圓圈與標籤) */
            --c-transport: #5B8FB9;     /* 藍：交通 */
            --c-food: #F39C12;          /* 橘：食 (必吃) */
            --c-spot: #27AE60;          /* 綠：景 (必拍) */
            --c-hotel: #795548;         /* 棕：住 (取代紫色) */
            --c-shop: #E74C3C;          /* 紅：購 (必買) */
            
            --radius: 16px;
        }

        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
        
        body {
            margin: 0; padding: 0;
            background-color: var(--bg-color);
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
            color: var(--text-main);
            padding-bottom: 100px;
            overscroll-behavior-y: none;
        }

        /* === 頂部區域 === */
        .header-area {
            position: sticky; top: 0; z-index: 100;
            background: rgba(248, 249, 250, 0.98);
            backdrop-filter: blur(10px);
            padding-top: env(safe-area-inset-top, 20px);
            padding-bottom: 10px;
            border-bottom: 1px solid rgba(0,0,0,0.05);
        }

        .app-title {
            text-align: center; font-size: 1.2rem; font-weight: 700; color: var(--fuji-blue);
            margin: 8px 0 15px 0; letter-spacing: 1px; display: flex; align-items: center; justify-content: center; gap: 8px;
        }

        /* Date Tabs */
        .date-tabs {
            display: flex; overflow-x: auto; padding: 0 15px; gap: 10px;
            scrollbar-width: none;
        }
        .date-tabs::-webkit-scrollbar { display: none; }
        
        .tab-item {
            flex: 0 0 auto;
            background: #fff;
            padding: 8px 16px;
            border-radius: 20px;
            text-align: center;
            color: var(--text-sub);
            transition: all 0.2s;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
            border: 1px solid #eee;
            min-width: 72px;
        }
        .tab-item.active {
            background: var(--fuji-blue);
            color: #fff;
            transform: scale(1.05);
            box-shadow: 0 4px 10px rgba(91, 143, 185, 0.3);
            border-color: var(--fuji-blue);
        }
        .tab-day { font-size: 0.9rem; font-weight: 700; display: block; }
        .tab-date { font-size: 0.75rem; opacity: 0.9; }

        /* === 內容容器 === */
        .container { padding: 20px; max-width: 600px; margin: 0 auto; }
        .page-section { display: none; animation: fadeIn 0.3s ease-out; }
        .page-section.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }

        /* === 天氣 Widget === */
        .weather-widget {
            background: linear-gradient(135deg, #A1C4FD 0%, #C2E9FB 100%);
            border-radius: 20px; padding: 15px 20px;
            color: white; margin-bottom: 25px;
            display: flex; justify-content: space-between; align-items: center;
            box-shadow: 0 8px 20px rgba(161, 196, 253, 0.4);
        }
        .w-city { font-size: 1.3rem; font-weight: 700; }
        .w-temp { font-size: 2.2rem; font-weight: 300; }
        .w-desc { font-size: 0.9rem; opacity: 0.9; font-weight: 500; }

        /* === Timeline 時間軸 === */
        .timeline { position: relative; padding-left: 5px; }

        .t-item {
            position: relative;
            padding-left: 55px; /* 留給圓圈的空間 */
            padding-bottom: 35px;
        }

        /* 垂直連線 */
        .t-item::before {
            content: ''; position: absolute;
            left: 24px; top: 45px; bottom: -10px; width: 2px;
            background-color: var(--line-color); z-index: 0;
        }
        .t-item:last-child::before { display: none; }

        /* 圓形 Logo */
        .t-circle {
            position: absolute; left: 0; top: 0;
            width: 50px; height: 50px; border-radius: 50%;
            background: #fff; display: flex; justify-content: center; align-items: center;
            z-index: 1; font-size: 1.2rem; color: #fff;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
            border: 4px solid var(--bg-color);
        }

        /* 分類顏色 */
        .type-transport .t-circle { background: var(--c-transport); }
        .type-restaurant .t-circle { background: var(--c-food); }
        .type-spot .t-circle { background: var(--c-spot); }
        .type-hotel .t-circle { background: var(--c-hotel); }
        .type-shop .t-circle { background: var(--c-shop); }

        /* 右側卡片 */
        .t-card {
            background: var(--card-bg);
            border-radius: var(--radius);
            padding: 18px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.04);
            border: 1px solid #f0f0f0;
        }

        .t-time { 
            font-family: monospace; font-size: 0.9rem; font-weight: 700; color: #999; 
            margin-bottom: 6px; display: block; 
        }
        
        /* 標題與標籤 */
        .t-header { display: flex; align-items: flex-start; justify-content: space-between; gap: 10px; margin-bottom: 8px; }
        .t-title { font-size: 1.15rem; font-weight: 700; color: var(--text-main); line-height: 1.4; flex: 1; }
        
        .tag { 
            display: inline-block; padding: 3px 8px; border-radius: 6px; 
            font-size: 0.75rem; color: #fff; font-weight: 700; white-space: nowrap; 
            margin-top: 2px;
        }

        /* 長描述文字 */
        .t-desc { 
            font-size: 0.95rem; color: #555; line-height: 1.7; 
            text-align: justify; margin-bottom: 15px; 
        }

        /* 導航按鈕 */
        .btn-nav {
            display: inline-flex; align-items: center; justify-content: center;
            width: 100%; color: var(--fuji-blue); text-decoration: none; 
            font-size: 0.9rem; font-weight: 600;
            background: #EBF5FB; padding: 12px; border-radius: 12px;
            transition: background 0.2s;
        }
        .btn-nav:active { background: #D6EAF8; }
        .btn-nav i { margin-right: 6px; }

        /* === 準備與資訊 === */
        .section-header { 
            font-size: 1.1rem; font-weight: 700; margin: 25px 0 12px; 
            color: var(--fuji-blue); border-left: 4px solid var(--fuji-blue); 
            padding-left: 12px; 
        }
        .prep-item { 
            background: #fff; padding: 14px 18px; border-radius: 12px; margin-bottom: 10px; 
            display: flex; align-items: center; box-shadow: 0 2px 4px rgba(0,0,0,0.03);
        }
        .prep-check { 
            width: 22px; height: 22px; border: 2px solid #ddd; border-radius: 6px; margin-right: 14px; 
            display: flex; align-items: center; justify-content: center; color: #fff; flex-shrink: 0;
        }
        .prep-item.checked .prep-check { background: var(--fuji-blue); border-color: var(--fuji-blue); }
        .prep-item.checked span { color: #ccc; text-decoration: line-through; }

        .info-card { 
            background: #fff; border-radius: 12px; padding: 18px; 
            margin-bottom: 12px; box-shadow: 0 2px 4px rgba(0,0,0,0.03); 
        }
        .info-row { display: flex; justify-content: space-between; margin-bottom: 8px; font-size: 0.95rem; }
        .info-key { color: #888; }
        .info-val { font-weight: 500; color: #333; text-align: right; }

        /* === 底部導航 === */
        .bottom-nav {
            position: fixed; bottom: 25px; left: 50%; transform: translateX(-50%);
            width: 85%; max-width: 380px;
            background: rgba(255,255,255,0.95); backdrop-filter: blur(15px);
            border-radius: 50px;
            box-shadow: 0 8px 30px rgba(0,0,0,0.15);
            display: flex; justify-content: space-around;
            padding: 12px 0; z-index: 200; border: 1px solid rgba(255,255,255,0.5);
        }
        .nav-btn {
            background: none; border: none;
            display: flex; flex-direction: column; align-items: center;
            color: #B0BEC5; font-size: 0.75rem; font-weight: 500; width: 70px;
            padding: 0;
        }
        .nav-btn i { font-size: 1.3rem; margin-bottom: 4px; transition: 0.2s; }
        .nav-btn.active { color: var(--fuji-blue); }
        .nav-btn.active i { transform: translateY(-2px); }

    </style>
</head>
<body>

    <div class="header-area">
        <div class="app-title"><i class="fa-solid fa-snowflake"></i> 雪國旅記</div>
        <div class="date-tabs" id="tabs-container"></div>
    </div>

    <!-- Page 1: 準備 -->
    <div id="page-prep" class="page-section">
        <div class="container">
            <div class="section-header">衣物與保暖</div>
            <div id="list-clothes"></div>
            <div class="section-header">電子與文件</div>
            <div id="list-docs"></div>
            <div class="section-header">個人護理</div>
            <div id="list-care"></div>
        </div>
    </div>

    <!-- Page 2: 行程 -->
    <div id="page-itinerary" class="page-section active">
        <div class="container">
            <!-- 天氣 Widget -->
            <div class="weather-widget">
                <div>
                    <div class="w-city" id="w-city">載入中...</div>
                    <div class="w-desc" id="w-desc">更新天氣資訊...</div>
                </div>
                <div class="w-temp" id="w-temp">--°</div>
            </div>

            <!-- Timeline -->
            <div class="timeline" id="itinerary-content"></div>
        </div>
    </div>

    <!-- Page 3: 資訊 -->
    <div id="page-info" class="page-section">
        <div class="container">
            <div class="section-header">航班資訊</div>
            <div class="info-card">
                <div style="font-weight:700; margin-bottom:8px; color:var(--c-transport);">去程 (12/20 - 21)</div>
                <div class="info-row"><span class="info-key">MU5352</span><span class="info-val">深圳 19:20 ➝ 上海 21:35</span></div>
                <div class="info-row"><span class="info-key">MU529</span><span class="info-val">上海 09:15 ➝ 名古屋 12:35</span></div>
            </div>
            <div class="info-card">
                <div style="font-weight:700; margin-bottom:8px; color:var(--c-transport);">回程 (1/3 - 4)</div>
                <div class="info-row"><span class="info-key">MU2026</span><span class="info-val">名古屋 19:15 ➝ 西安 23:25</span></div>
                <div class="info-row"><span class="info-key">MU2331</span><span class="info-val">西安 12:00 ➝ 深圳 14:40</span></div>
            </div>

            <div class="section-header">住宿總表 (自動彙整)</div>
            <div id="hotel-list-complete">
                <!-- JS 自動生成 -->
            </div>

            <div class="section-header">緊急聯絡</div>
            <div class="info-card" style="background:#FFF5F5; border:1px solid #FFCDD2; text-align:center;">
                <div style="color:#D32F2F; font-weight:700; margin-bottom:5px;">駐日代表處急難救助</div>
                <a href="tel:+818010097179" style="font-size:1.4rem; font-weight:800; color:#D32F2F; text-decoration:none;">+81-80-1009-7179</a>
            </div>
        </div>
    </div>

    <!-- Bottom Nav -->
    <div class="bottom-nav">
        <button class="nav-btn" onclick="switchPage('page-prep', this)">
            <i class="fa-solid fa-suitcase"></i>準備
        </button>
        <button class="nav-btn active" onclick="switchPage('page-itinerary', this)">
            <i class="fa-solid fa-map-location-dot"></i>行程
        </button>
        <button class="nav-btn" onclick="switchPage('page-info', this)">
            <i class="fa-solid fa-circle-info"></i>資訊
        </button>
    </div>

    <script>
        // === 1. 100% 完整行程・深度指南版 ===
        // tags: 必吃, 必拍, 必買, 必住, 必玩, 世界遺產, 名物
        const itinerary = [
            {
                day: "Day 1", date: "12/20", city: "Shanghai", weather: "5°C 晴",
                items: [
                    { time: "18:00", type: "transport", title: "深圳機場貴賓室手續", desc: "抵達深圳寶安國際機場後，請務必先前往處理貴賓室事宜。\n★位置：通過安檢前，從國內出發大堂的 3號閘口 進入，直行至 C登機區域 的 C24號櫃檯。\n★流程：出示並驗證憑證，工作人員將發出「憑證券」。憑此券通過安檢後，即可進入指定的貴賓休息室（視登機口位置安排），享受舒適的候機時光。", nav: "深圳寶安國際機場" },
                    { time: "19:20", type: "transport", title: "飛往上海", desc: "搭乘中國東方航空 MU5352 航班，由深圳 (SZX) 飛往上海浦東 (PVG)。預計飛行時間約 2小時 15分鐘，抵達時間為 21:35。抵達後請前往行李轉盤提取行李，準備前往酒店休息。", nav: "上海浦東國際機場" },
                    { time: "22:30", type: "transport", title: "漢庭酒店接駁巴士", desc: "漢庭酒店提供貼心的免費接機服務，無需擔心深夜交通問題。\n★ T1航站樓：請前往 3層出發層 3號門 等候。\n★ T2航站樓：請前往 3層出發層 29號門 等候。\n★ 注意事項：接機服務最晚至凌晨 02:10 結束。需於當天致電預約，請撥打接機專用電話：13816979688，並提前10分鐘到達候車點。", nav: "上海浦東國際機場 T2" },
                    { time: "23:00", type: "hotel", title: "漢庭酒店 (浦東機場)", desc: "入住漢庭上海浦東國際機場聞居路酒店。這是一家專為過境旅客設計的酒店，設施齊全，床鋪舒適。辦理入住時，建議順便確認明早送機的班次時間（凌晨4:30開始服務）。若您計畫明日前往迪士尼，酒店也有提供定點接送（早上7:10出發），需提前諮詢前台。", nav: "漢庭酒店上海浦東機場聞居路店" }
                ]
            },
            {
                day: "Day 2", date: "12/21", city: "Nagoya", weather: "8°C 晴",
                items: [
                    { time: "07:00", type: "transport", title: "前往機場", desc: "搭乘酒店免費送機巴士前往浦東機場。送機服務從凌晨 04:30 開始，上午平均 1.5 小時一班，建議提早下樓候車，以免錯過航班。", nav: "上海浦東國際機場" },
                    { time: "09:15", type: "transport", title: "飛往名古屋", desc: "搭乘東方航空 MU529 航班飛往日本名古屋中部國際機場 (NGO)。預計飛行時間約 2小時 20分，於日本時間 12:35 抵達。入境後請填寫 Visit Japan Web 或紙本入境卡。", nav: "中部國際機場" },
                    { time: "13:47", type: "transport", title: "名鐵特急", desc: "從中部國際機場搭乘「名鐵常滑・空港線特急（名鐵岐阜行）」，前往名鐵名古屋站。車程約 37 分鐘，費用 ¥980。這是一段舒適的鐵道旅程，沿途可欣賞伊勢灣的海景。", nav: "名鐵名古屋站" },
                    { time: "14:30", type: "spot", title: "購買交通票券", desc: "抵達名古屋站後，請在自動售票機或櫃檯購買「名古屋地鐵＋巴士一天Pass」(ドニチエコきっぷ)，費用 ¥620。這張票券在週末假日特別划算，今日所有的市區移動都靠它了。", nav: "名鐵名古屋站" },
                    { time: "15:00", type: "hotel", title: "東橫INN 名古屋丸之內", desc: "搭乘地鐵櫻通線或鶴舞線至「丸之內站」，步行前往酒店辦理入住並寄存行李。東橫INN是標準的日式商務旅館，乾淨且功能齊全，還附有免費早餐。", nav: "東橫INN 名古屋丸之內" },
                    { time: "16:30", type: "restaurant", title: "熱田蓬萊軒 本店", desc: "來到名古屋，絕對不能錯過這家擁有140年歷史的鰻魚飯名店！必點招牌「鰻魚飯三吃 (Hitsumabushi)」。炭火慢烤的鰻魚外皮酥脆、肉質軟嫩，搭配秘製醬汁香氣四溢。\n第一吃：品嚐原味。\n第二吃：加入海苔、蔥花、芥末。\n第三吃：淋上高湯做成茶泡飯。", tags: ["必吃"], nav: "熱田蓬萊軒 本店" },
                    { time: "17:30", type: "spot", title: "熱田神宮", desc: "日本三大神宮之一，地位僅次於伊勢神宮。這裡供奉著三神器之一的「草薙劍」。漫步在被千年古樹環繞的參道上，空氣中瀰漫著神聖莊嚴的氣息。這裡是名古屋的心靈綠洲，非常適合散步祈福，感受遠離塵囂的寧靜。", tags: ["必拍"], nav: "熱田神宮" },
                    { time: "19:00", type: "shop", title: "LalaPort 名古屋", desc: "前往港區的大型購物中心 LalaPort。這裡有全名古屋最大的 Workman 女子店 (Workman Girl)，非常適合在此補齊接下來雪地行程所需的機能服飾、防滑雪靴與保暖配件，價格實惠且設計時尚。", tags: ["必買"], nav: "LalaPort 名古屋" },
                    { time: "21:00", type: "restaurant", title: "風來坊", desc: "宵夜場！前往「風來坊 名站五丁目店」品嚐名古屋名物——手羽先 (炸雞翅)。這裡的雞翅外皮炸得酥脆，裹上帶有胡椒香氣的特製醬汁，鹹香微辣，是搭配啤酒的絕佳選擇。", tags: ["必吃"], nav: "風來坊 名站五丁目店" }
                ]
            },
            {
                day: "Day 3", date: "12/22", city: "Gifu", weather: "6°C 多雲",
                items: [
                    { time: "08:23", type: "transport", title: "移動至岐阜", desc: "從丸之內出發，經名古屋站轉乘 JR東海道本線（快速）前往岐阜站。車程約 20 分鐘，費用 ¥470。岐阜是織田信長的發跡地，充滿歷史氛圍。", nav: "岐阜站" },
                    { time: "09:10", type: "hotel", title: "岐阜舒適酒店", desc: "抵達岐阜後，先前往位於車站旁的 Comfort Hotel Gifu 寄存行李。這家飯店地理位置極佳，緊鄰車站與巴士總站。", nav: "Comfort Hotel Gifu" },
                    { time: "09:27", type: "transport", title: "前往大垣", desc: "搭乘 JR 東海道本線前往大垣站，車程約 12 分鐘，費用 ¥250。大垣市以豐富的地下水資源聞名，被稱為「水之都」。", nav: "大垣站" },
                    { time: "09:50", type: "spot", title: "大垣城", desc: "參觀大垣城。這裡是日本戰國時代關原之戰的重要戰略據點，石田三成曾以此為大本營。白色的天守閣在藍天下格外醒目，內部展示了豐富的歷史文物與戰國史料。", nav: "大垣城" },
                    { time: "10:30", type: "restaurant", title: "金蝶園総本家", desc: "來到大垣必吃の名物甜點——水羊羹 (水まんじゅう)。這家老店使用大垣優質的地下水製作，羊羹呈現半透明狀，浸泡在冰涼的水中，口感清爽滑嫩，甜而不膩，是視覺與味覺的雙重享受。", tags: ["必吃"], nav: "金蝶園総本家" },
                    { time: "11:26", type: "transport", title: "返回岐阜", desc: "搭乘 JR 返回岐阜站，準備下午的登山行程。", nav: "岐阜站" },
                    { time: "12:15", type: "restaurant", title: "金華山展望餐廳", desc: "搭乘金華山纜車上山後，在展望餐廳「Le Pont de Ciel」享用午餐。這裡最大的賣點是絕佳的視野，可以一邊用餐，一邊俯瞰長良川蜿蜒流過岐阜市區的壯麗景色。", nav: "岐阜城" },
                    { time: "14:00", type: "spot", title: "岐阜城 & 金華山", desc: "參觀聳立於金華山頂的岐阜城。這裡是織田信長宣示「天下布武」的起點，在此可以感受信長當年的雄心壯志。山頂還有一個可愛的「松鼠村」，可以戴上手套，讓松鼠跳到手上吃飼料，非常療癒。", tags: ["必拍"], nav: "岐阜城" },
                    { time: "17:30", type: "shop", title: "Workman Plus", desc: "下山後，若仍需採購裝備，可前往 Workman Plus 岐阜金園店。這裡的貨源通常比市區更充足，可以買到高CP值的防寒衣物。", nav: "Workman Plus Gifu Kinzono" },
                    { time: "19:00", type: "restaurant", title: "居酒屋革命 酔っ手羽", desc: "晚餐在岐阜站前的「酔っ手羽」居酒屋解決。這是一家氣氛熱鬧的大眾居酒屋，提供各種平價又美味的日式料理，非常適合在一天行程結束後放鬆小酌。", nav: "岐阜站" },
                    { time: "21:00", type: "hotel", title: "岐阜舒適酒店", desc: "回到飯店辦理入住。Comfort Hotel 提供免費的自助早餐與迎賓咖啡，床鋪舒適，讓您能好好休息。", nav: "Comfort Hotel Gifu" }
                ]
            },
            {
                day: "Day 4", date: "12/23", city: "Gero", weather: "2°C 偶飄雪",
                items: [
                    { time: "08:30", type: "spot", title: "Gifu City Tower 43", desc: "早餐後，散步至岐阜最高樓——Gifu City Tower 43。搭乘專用電梯直達 43 樓的免費展望台，可以將濃尾平原、長良川以及遠處的群山盡收眼底，視野極佳。", nav: "Gifu City Tower 43" },
                    { time: "11:44", type: "transport", title: "JR 高山本線", desc: "從岐阜站搭乘 JR 高山本線（高山行）前往下呂溫泉。車程約 2 小時，費用 ¥1690。這段鐵路沿著飛驒川行駛，窗外是壯麗的峽谷與溪流景色，被譽為日本最美的鐵道路線之一。", nav: "下呂站" },
                    { time: "15:00", type: "hotel", title: "TAOYA 下呂", desc: "抵達下呂站後，搭乘接駁車前往今晚的住宿——TAOYA 下呂。這是一家主打「All Inclusive (全包式)」服務的豪華溫泉旅館。房費已包含晚餐、早餐，甚至晚餐時段的酒水飲料、宵夜拉麵都無限暢飲。在被稱為日本三大名泉的「美人之湯」露天風呂中，一邊泡湯一邊欣賞雪景，是此行最奢華的享受。", tags: ["必住"], nav: "TAOYA 下呂" }
                ]
            },
            {
                day: "Day 5", date: "12/24", city: "Takayama", weather: "0°C 小雪",
                items: [
                    { time: "11:35", type: "transport", title: "JR 高山本線", desc: "告別下呂溫泉，繼續搭乘 JR 高山本線前往高山。車程約 1 小時，費用 ¥990。隨著海拔升高，窗外的雪景也會越來越厚。", nav: "高山站" },
                    { time: "12:45", type: "hotel", title: "KOKO酒店飛騨高山", desc: "抵達高山站後，步行前往 KOKO HOTEL Hida Takayama 辦理入住並寄存行李。這家飯店將是我們接下來三晚的基地，位置優越，方便前往各個景點。", nav: "KOKO HOTEL Hida Takayama" },
                    { time: "13:00", type: "spot", title: "三町筋老街", desc: "高山最著名的景點「三町筋」。這裡保留了江戶時代的風貌，黑木格柵的傳統建築並排而立，兩旁流淌著清澈的水渠。漫步在老街，一定要嚐嚐這裡的街頭美食，特別是放在仙貝上的「飛驒牛握壽司」(推薦：こって牛)，入口即化的油脂香氣令人難忘。", tags: ["必吃","必逛"], nav: "高山三町筋" },
                    { time: "15:15", type: "spot", title: "日枝神社", desc: "位於老街南端的日枝神社，被認為是動畫電影《你的名字》中宮水神社的原型之一。巨大的紅色鳥居與參道兩旁的古杉樹，營造出神祕莊嚴的氛圍。", tags: ["必拍"], nav: "日枝神社" },
                    { time: "16:00", type: "spot", title: "飛驒天滿宮", desc: "供奉學問之神菅原道真的神社，環境清幽，適合安靜地參拜。", nav: "飛驒天滿宮" },
                    { time: "17:00", type: "restaurant", title: "丸明 飛騨高山店", desc: "晚餐時刻，來到高山怎能不吃飛驒牛？「丸明」是由當地精肉店直營的燒肉餐廳，能以相對實惠的價格提供最高等級的 A5 飛驒牛。看著美麗油花的肉片在烤網上滋滋作響，入口軟嫩多汁，絕對是人生必吃的美食體驗。", tags: ["必吃"], nav: "丸明 高山" },
                    { time: "19:00", type: "shop", title: "超市採買", desc: "晚餐後，前往當地的超市「駿河屋」或「Boss Foods Market」。建議購買一些飯糰、麵包、零食與飲品，作為明天前往白川鄉的午餐備用，因為白川鄉餐廳較少且容易客滿。", nav: "Boss Foods Market" }
                ]
            },
            {
                day: "Day 6", date: "12/25", city: "Shirakawa", weather: "-3°C 大雪",
                items: [
                    { time: "07:20", type: "transport", title: "濃飛巴士", desc: "一早前往高山濃飛巴士中心，搭乘巴士前往白川鄉。這段路程是著名的觀光路線，建議提前預約或提早排隊。", nav: "白川鄉" },
                    { time: "09:00", type: "spot", title: "白川鄉合掌村", desc: "抵達被厚厚白雪覆蓋的童話世界——白川鄉合掌村。這裡是聯合國世界文化遺產，一棟棟呈人字型的茅草屋頂小屋矗立在雪地中，彷彿薑餅屋般可愛。強烈建議搭乘接駁車前往「城山展望台」，從高處俯瞰整個被白雪包圍的聚落，那種震撼與寧靜美得讓人屏息。", tags: ["必拍","世界遺產"], nav: "城山展望台 (白川鄉)" },
                    { time: "12:00", type: "restaurant", title: "白水園", desc: "在合掌村內的「白水園」享用午餐。推薦品嚐在地鄉土料理「朴葉味噌定食」。在乾燥的朴葉上放上味噌、蔥花與香菇炭烤，鹹香的滋味非常下飯，暖胃又暖心。", nav: "白水園" },
                    { time: "14:00", type: "transport", title: "濃飛巴士", desc: "結束夢幻的白川鄉之旅，搭乘巴士返回高山。", nav: "高山濃飛巴士中心" },
                    { time: "17:30", type: "restaurant", title: "Mikado (みかど)", desc: "晚餐在高山市區的「みかど」食堂用餐。這裡提供溫暖樸實的日式定食、拉麵與鄉土料理，深受當地人喜愛。", nav: "みかど" },
                    { time: "20:00", type: "hotel", title: "KOKO酒店飛騨高山", desc: "回到飯店休息，準備明天的行程。", nav: "KOKO HOTEL Hida Takayama" }
                ]
            },
            {
                day: "Day 7", date: "12/26", city: "Hida", weather: "-1°C 陰",
                items: [
                    { time: "08:30", type: "spot", title: "宮川朝市", desc: "早餐後前往日本三大朝市之一的「宮川朝市」。沿著宮川河岸，當地農家擺攤販售新鮮的蘋果、味噌、漬物與手工藝品。推薦買些飛驒蘋果，鮮脆多汁。", tags: ["必逛"], nav: "宮川朝市" },
                    { time: "10:30", type: "restaurant", title: "味藏天國", desc: "午餐再次挑戰飛驒牛！「味藏天國」是 JA 農協直營的燒肉店，肉質有絕對保證，且價格比一般餐廳更親民。這裡的飛驒牛拼盤份量十足，是高山人氣極高的排隊名店。", tags: ["必吃"], nav: "味藏天國" },
                    { time: "12:30", type: "transport", title: "JR 高山本線", desc: "搭乘 JR 前往飛驒古川，車程約 20 分鐘。這座小鎮因動畫電影《你的名字》而聲名大噪。", nav: "飛驒古川站" },
                    { time: "14:00", type: "spot", title: "飛驒古川", desc: "進行《你的名字》聖地巡禮。必看景點包括車站的天橋、圖書館外觀，以及最著名的「瀨戶川白壁土藏街」。清澈的水溝中養著色彩斑斕的鯉魚（冬季可能會移至深水區過冬），搭配白色的倉庫建築，景色寧靜優美。", tags: ["必拍"], nav: "瀨戶川白壁土藏街" },
                    { time: "16:00", type: "transport", title: "JR 返回", desc: "搭乘 JR 返回高山站。", nav: "高山站" },
                    { time: "18:00", type: "restaurant", title: "中華そば M", desc: "晚餐品嚐高山另一項名物——高山拉麵。這家「中華そば M」以法式料理的手法重新詮釋傳統拉麵，醬油湯底清爽富有層次，搭配細捲麵與飛驒牛叉燒，風味獨特。", nav: "中華そば M" },
                    { time: "20:00", type: "hotel", title: "KOKO酒店飛騨高山", desc: "回到飯店，整理行李，明天將前往深山溫泉區。", nav: "KOKO HOTEL Hida Takayama" }
                ]
            },
            {
                day: "Day 8", date: "12/27", city: "Okuhida", weather: "-6°C 嚴寒",
                items: [
                    { time: "07:00", type: "spot", title: "購買奧飛驒套票", desc: "在高山濃飛巴士中心購買「奧飛驒套票」(奥飛騨まるごと・バリューきっぷ)，這張套票包含了高山至新穗高的巴士以及新穗高纜車來回票，非常划算。", nav: "高山濃飛巴士中心" },
                    { time: "07:00", type: "transport", title: "濃飛巴士", desc: "搭乘巴士前往新平湯溫泉的「禪通寺前」站，先去今晚的民宿寄存行李，輕裝上陣。", nav: "新平湯溫泉" },
                    { time: "08:54", type: "transport", title: "濃飛巴士", desc: "繼續搭乘巴士前往終點站——新穗高高空纜車。", nav: "新穗高高空纜車" },
                    { time: "10:00", type: "spot", title: "新穗高纜車", desc: "搭乘日本唯一的雙層纜車，在雲端漫步。隨著纜車爬升，四周的景色從森林轉變為壯麗的雪山峻嶺。抵達海拔 2156 米的西穗高口展望台，360 度被北阿爾卑斯山的雪白群峰包圍，那一刻的震撼與感動無法用言語形容。別忘了在觀景台與著名的「雪人」合影。", tags: ["必拍","絕景"], nav: "新穗高高空纜車" },
                    { time: "13:30", type: "restaurant", title: "うな亭", desc: "在纜車站附近或返回新平湯途中享用午餐。", nav: "新穗高高空纜車" },
                    { time: "15:00", type: "hotel", title: "飛驒民家 甚九郎", desc: "入住位於新平湯溫泉的傳統合掌造民宿「甚九郎」。這裡保留了古老的日本建築風格，晚餐是在圍爐裏旁享用炭火燒烤的飛驒牛與岩魚，氣氛溫馨。民宿內還有露天溫泉，可以獨享雪見風呂。", tags: ["必住"], nav: "新平湯溫泉" }
                ]
            },
            {
                day: "Day 9", date: "12/28", city: "Hirayu", weather: "-4°C 滑雪",
                items: [
                    { time: "09:41", type: "transport", title: "濃飛巴士", desc: "從禪通寺前搭乘巴士前往平湯溫泉。", nav: "平湯溫泉" },
                    { time: "10:00", type: "hotel", title: "湯之平館", desc: "抵達平湯溫泉後，先前往今晚的住宿「湯之平館」寄存行李。", nav: "湯之平館" },
                    { time: "10:30", type: "transport", title: "接駁巴士", desc: "搭乘接駁車前往平湯溫泉滑雪場。", nav: "平湯溫泉滑雪場" },
                    { time: "10:45", type: "spot", title: "平湯溫泉滑雪場", desc: "這是一個小而美的滑雪場，非常適合初學者與家庭。這裡的雪質是著名的「粉雪」，鬆軟如棉花糖，摔倒了也不會痛。即使不滑雪，也可以租個雪盆玩耍，或是單純欣賞被雪覆蓋的寧靜森林。", tags: ["必玩"], nav: "平湯溫泉滑雪場" },
                    { time: "12:30", type: "restaurant", title: "Ankiya (あんき屋)", desc: "滑雪場內的餐廳，必點滑雪後熱騰騰的咖哩飯或山菜料理，補充體力。", nav: "あんき屋" },
                    { time: "15:00", type: "hotel", title: "湯之平館", desc: "Check-in。這是一家擁有百年歷史的溫泉旅館，古色古香。其露天溫泉深受好評，能一邊泡著濁湯一邊賞雪，消除滑雪後的疲勞。", nav: "湯之平館" }
                ]
            },
            {
                day: "Day 10", date: "12/29", city: "Matsumoto", weather: "3°C 風大",
                items: [
                    { time: "10:30", type: "restaurant", title: "Alps Horn", desc: "退房後，在平湯巴士站旁的餐廳享用早午餐，推薦飛驒牛陶板燒與平湯拉麵。", nav: "Alps Kaido Hirayu" },
                    { time: "12:55", type: "transport", title: "特急巴士", desc: "搭乘特急巴士穿越安房隧道，前往松本巴士總站。車程約 1.5 小時，費用 ¥2800。", nav: "松本巴士總站" },
                    { time: "15:00", type: "hotel", title: "東橫INN 松本站", desc: "抵達松本，入住位於車站旁的東橫INN 松本站東口。松本是一座充滿藝術氣息的城市，草間彌生的美術館就在這裡。", nav: "東橫INN 松本站東口" },
                    { time: "16:00", type: "spot", title: "松本城 (外觀)", desc: "步行前往松本城。它是日本現存最古老的五重六階木造天守，因其黑色的外觀而被稱為「烏鴉城」。在雪山背景的映襯下，松本城顯得格外雄偉帥氣。", nav: "松本城" }
                ]
            },
            {
                day: "Day 11", date: "12/30", city: "Nagano", weather: "-2°C 雪",
                items: [
                    { time: "06:54", type: "transport", title: "JR 線", desc: "搭乘 JR 線從松本前往長野站，車程約 1 小時。", nav: "長野站" },
                    { time: "08:40", type: "spot", title: "購買套票", desc: "在長野電鐵長野站購買「SNOW MONKEY PASS」(約¥3200)。這張票券包含了前往雪猴公園的電車、巴士以及公園門票。", nav: "長野電鐵長野站" },
                    { time: "09:00", type: "transport", title: "長電巴士", desc: "搭乘長電巴士前往雪猴公園。", nav: "地獄谷野猿公苑" },
                    { time: "10:00", type: "spot", title: "雪猴公園", desc: "這是一生必看一次的奇景！在冰天雪地中，野生的日本獼猴（雪猴）臉紅紅地泡在溫泉裡，一臉享受的模樣超級療癒。要看到牠們需要步行一段約 30 分鐘的山路，但當你看到小猴子依偎在媽媽懷裡泡湯的畫面，一切辛苦都值得了。", tags: ["必拍","奇景"], nav: "地獄谷野猿公苑" },
                    { time: "12:30", type: "transport", title: "長野電鐵", desc: "從湯田中搭乘長野電鐵前往小布施。", nav: "小布施" },
                    { time: "13:00", type: "spot", title: "小布施", desc: "小布施被稱為「栗子之鄉」，街道整齊優雅，充滿文藝氣息。", nav: "小布施" },
                    { time: "13:30", type: "restaurant", title: "竹風堂 / 小布施堂", desc: "午餐必吃竹風堂的栗子飯，甜點則推薦小布施堂著名的「朱雀」蒙布朗，濃郁的栗子香氣令人回味。", tags: ["必吃"], nav: "小布施堂" },
                    { time: "18:00", type: "hotel", title: "東橫INN 長野站", desc: "返回長野站，入住東橫INN 長野站東口。", nav: "東橫INN 長野站東口" }
                ]
            },
            {
                day: "Day 12", date: "12/31", city: "Togakushi", weather: "-5°C 嚴寒",
                items: [
                    { time: "06:50", type: "transport", title: "觀光巴士", desc: "從長野站搭乘觀光巴士前往戶隱奧社入口。", nav: "戶隱神社奧社" },
                    { time: "08:00", type: "spot", title: "戶隱神社奧社", desc: "著名的能量景點。穿過隨神門，映入眼簾的是延續約 2 公里的參道，兩旁種植著樹齡超過 400 年的巨大杉樹。冬季時，杉樹與參道被白雪覆蓋，形成一條白色的神聖通道，氣氛莊嚴肅穆。", tags: ["必拍"], nav: "戶隱神社奧社" },
                    { time: "12:00", type: "restaurant", title: "戶隱蕎麥麵 山口屋", desc: "戶隱是蕎麥麵的發源地之一。必吃日本三大蕎麥麵之一的「戶隱蕎麥麵」，特色是使用名為「ぼっち盛り」的圓形束狀擺盤，口感勁道，香氣十足。", tags: ["必吃"], nav: "戶隱蕎麥麵山口屋" },
                    { time: "14:55", type: "transport", title: "觀光巴士", desc: "搭乘巴士返回長野站。", nav: "長野站" },
                    { time: "18:00", type: "restaurant", title: "拉麵 Misoya", desc: "今天是跨年夜，依照習俗要吃蕎麥麵，但來碗熱騰騰的信州味噌拉麵也是絕佳選擇。「拉麵 Misoya」專賣味噌拉麵，湯頭濃郁暖胃。", nav: "らあめん みそや" },
                    { time: "20:00", type: "hotel", title: "東橫INN 長野站", desc: "續住長野，準備迎接新年。", nav: "東橫INN 長野站東口" }
                ]
            },
            {
                day: "Day 13", date: "1/1", city: "Nagano", weather: "1°C 參拜",
                items: [
                    { time: "09:00", type: "spot", title: "善光寺 (初詣)", desc: "今天是日本的新年元旦，我們將前往善光寺進行「初詣」（新年第一次參拜）。善光寺是日本極具代表性的古剎，據說一生一定要來參拜一次。現場會非常熱鬧，充滿了節慶的氣氛。", tags: ["必去"], nav: "善光寺" },
                    { time: "14:00", type: "transport", title: "特急信濃", desc: "告別雪國長野，搭乘 JR 特急信濃號 (Shinano) 返回名古屋。車程約 3 小時。", nav: "名古屋站" },
                    { time: "17:00", type: "hotel", title: "東橫INN 名古屋錦", desc: "抵達名古屋後，入住位於繁華街區的東橫INN 名古屋錦，方便逛街與用餐。", nav: "東橫INN 名古屋錦" }
                ]
            },
            {
                day: "Day 14", date: "1/2", city: "Nagoya", weather: "9°C 晴",
                items: [
                    { time: "06:45", type: "restaurant", title: "kako 柳橋店", desc: "體驗名古屋獨特的早餐文化。在這家復古咖啡店點一杯咖啡，就會免費送上厚片吐司。強烈推薦加點自家製的「小倉紅豆餡」，與奶油吐司是絕配。", tags: ["必吃"], nav: "kako 柳橋店" },
                    { time: "10:00", type: "shop", title: "Workman Plus", desc: "如果還有購物需求，可前往市區的 Workman Plus 補貨。", nav: "Workman Plus" },
                    { time: "12:00", type: "restaurant", title: "HARBS", desc: "HARBS 是發源於名古屋的甜點名店，這裡的水果千層蛋糕是絕對的夢幻逸品！薄薄的蛋皮夾著清爽不膩的鮮奶油與滿滿的新鮮水果，份量十足。", tags: ["必吃"], nav: "HARBS 名鉄名古屋" },
                    { time: "15:00", type: "shop", title: "大須商店街", desc: "被稱為名古屋的秋葉原，這條充滿活力的商店街有許多古著店、電器行與街頭小吃，非常好逛。", tags: ["必逛"], nav: "大須觀音" },
                    { time: "19:00", type: "restaurant", title: "山本屋本店", desc: "晚餐品嚐名古屋名物：味噌煮烏龍麵。這裡的麵條使用只加水不加鹽的工法，口感偏硬有嚼勁，搭配濃郁的赤味噌湯頭，風味獨特。", nav: "山本屋本店" },
                    { time: "21:00", type: "hotel", title: "東橫INN 名古屋錦", desc: "續住。", nav: "東橫INN 名古屋錦" }
                ]
            },
            {
                day: "Day 15", date: "1/3", city: "Nagoya", weather: "10°C 晴",
                items: [
                    { time: "08:00", type: "restaurant", title: "客美多咖啡", desc: "再次體驗經典的名古屋早餐文化，Komeda Coffee 源自名古屋，舒適的座位與美味的早餐是開始一天最好的方式。", nav: "Komeda Coffee" },
                    { time: "09:00", type: "spot", title: "名古屋城", desc: "日本三大名城之一，最大的特色是天守閣屋頂上那對金光閃閃的「金鯱」。這裡曾是德川家康的居城，重建後的本丸御殿金碧輝煌，展現了昔日武家的威望與奢華。", tags: ["必拍"], nav: "名古屋城" },
                    { time: "15:00", type: "transport", title: "前往機場", desc: "搭乘名鐵特急前往中部國際機場，準備辦理登機手續。", nav: "中部國際機場" },
                    { time: "19:15", type: "transport", title: "飛往西安", desc: "搭乘東方航空 MU2026 航班，由名古屋 (NGO) 飛往西安 (XIY)。抵達時間為 23:25，需在西安過夜中轉。", nav: "中部國際機場" }
                ]
            },
            {
                day: "Day 16", date: "1/4", city: "Xian", weather: "5°C 返程",
                items: [
                    { time: "12:00", type: "transport", title: "返回深圳", desc: "搭乘東方航空 MU2331 航班，由西安 (XIY) 飛往深圳 (SZX)。預計 14:40 抵達。帶著滿滿的雪國回憶與戰利品，平安回家！", nav: "西安咸陽國際機場" }
                ]
            }
        ];

        // === 2. 準備清單資料 ===
        const prepData = {
            clothes: ["發熱衣褲 (多套)", "防風厚羽絨", "中層刷毛外套", "防水雪褲", "毛帽/圍巾/手套", "防滑雪靴"],
            docs: ["護照 & 影本", "日幣現金", "信用卡", "網卡/漫遊", "訂房確認單"],
            care: ["感冒/暈車藥", "高保濕乳液", "護唇膏", "防曬霜", "暖暖包", "牙刷毛巾"]
        };

        // === 3. 程式邏輯 ===
        const API_KEY = '14e048eab663dcb763045ba32d8e63c1';
        const cityMap = {
            "Shanghai": "Shanghai,CN", "Nagoya": "Nagoya,JP", "Gifu": "Gifu-shi,JP",
            "Gero": "Gero,JP", "Takayama": "Takayama,JP", "Shirakawa": "Shirakawa,JP",
            "Hida": "Hida,JP", "Okuhida": "Takayama,JP", "Hirayu": "Takayama,JP",
            "Matsumoto": "Matsumoto,JP", "Nagano": "Nagano,JP", "Togakushi": "Nagano,JP",
            "Xian": "Xian,CN"
        };

        function init() {
            renderTabs();
            renderPrepList('list-clothes', prepData.clothes);
            renderPrepList('list-docs', prepData.docs);
            renderPrepList('list-care', prepData.care);
            loadDay(0);
            renderHotelList();
        }

        function renderTabs() {
            const container = document.getElementById('tabs-container');
            container.innerHTML = itinerary.map((d, i) => `
                <div class="tab-item ${i===0?'active':''}" onclick="loadDay(${i})">
                    <span class="tab-day">${d.day}</span>
                    <span class="tab-date">${d.date}</span>
                </div>
            `).join('');
        }

        async function loadDay(index) {
            document.querySelectorAll('.tab-item').forEach((t, i) => t.classList.toggle('active', i === index));
            const activeTab = document.querySelector('.tab-item.active');
            if(activeTab) activeTab.scrollIntoView({ behavior: 'smooth', inline: 'center' });

            const data = itinerary[index];
            const container = document.getElementById('itinerary-content');

            // Render Weather
            document.getElementById('w-city').innerText = data.city;
            document.getElementById('w-desc').innerText = "更新中...";
            try {
                const res = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${cityMap[data.city] || 'Tokyo,JP'}&units=metric&lang=zh_tw&appid=${API_KEY}`);
                const wData = await res.json();
                if(wData.cod === 200) {
                    document.getElementById('w-temp').innerText = Math.round(wData.main.temp) + "°";
                    document.getElementById('w-desc').innerText = wData.weather[0].description;
                }
            } catch(e) {}

            // Render Timeline
            let html = '';
            data.items.forEach(item => {
                let typeClass = "type-spot";
                let icon = "fa-camera";
                
                if(item.type === 'transport') { typeClass = "type-transport"; icon = "fa-plane"; }
                else if(item.type === 'restaurant') { typeClass = "type-restaurant"; icon = "fa-utensils"; }
                else if(item.type === 'hotel') { typeClass = "type-hotel"; icon = "fa-bed"; }
                else if(item.type === 'shop') { typeClass = "type-shop"; icon = "fa-bag-shopping"; }

                let tagsHtml = '';
                if(item.tags) {
                    item.tags.forEach(t => {
                        let bg = "#888";
                        if(t.includes("必吃")) bg = "var(--c-food)";
                        if(t.includes("必拍") || t.includes("絕景")) bg = "var(--c-spot)";
                        if(t.includes("必住")) bg = "var(--c-hotel)";
                        if(t.includes("必逛") || t.includes("必買")) bg = "var(--c-shop)";
                        tagsHtml += `<span class="tag" style="background:${bg}">${t}</span>`;
                    });
                }

                html += `
                <div class="t-item ${typeClass}">
                    <div class="t-circle"><i class="fa-solid ${icon}"></i></div>
                    <div class="t-card">
                        <span class="t-time">${item.time}</span>
                        <div class="t-header">
                            <div class="t-title">${item.title}</div>
                            <div>${tagsHtml}</div>
                        </div>
                        <div class="t-desc">${item.desc}</div>
                        <a href="https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(item.nav)}" target="_blank" class="btn-nav">
                            <i class="fa-solid fa-location-arrow"></i> 導航
                        </a>
                    </div>
                </div>`;
            });
            container.innerHTML = html;
            window.scrollTo({top: 0, behavior: 'smooth'});
        }

        function renderPrepList(id, items) {
            document.getElementById(id).innerHTML = items.map(i => `
                <div class="prep-item" onclick="this.classList.toggle('checked')">
                    <div class="prep-check"><i class="fa-solid fa-check"></i></div><span>${i}</span>
                </div>`).join('');
        }

        function renderHotelList() {
            const container = document.getElementById('hotel-list-complete');
            let html = '';
            itinerary.forEach(d => {
                d.items.forEach(item => {
                    if(item.type === 'hotel' && !item.title.includes('寄存')) {
                        html += `
                        <div class="info-card" style="margin-bottom:8px; padding:12px;">
                            <div style="display:flex; justify-content:space-between; align-items:center;">
                                <div>
                                    <div style="font-weight:700; color:#333;">${item.title}</div>
                                    <div style="font-size:0.85rem; color:#777;">${d.date} (${d.day})</div>
                                </div>
                                <a href="https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(item.nav)}" target="_blank" style="color:var(--fuji-blue); font-size:1.2rem;">
                                    <i class="fa-solid fa-location-dot"></i>
                                </a>
                            </div>
                        </div>`;
                    }
                });
            });
            container.innerHTML = html;
        }

        function switchPage(pid, btn) {
            document.querySelectorAll('.page-section').forEach(p => p.classList.remove('active'));
            document.getElementById(pid).classList.add('active');
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
        }

        window.onload = init;
    </script>
</body>
</html>
