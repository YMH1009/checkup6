
<head>
    <meta charset="UTF-8" />
    <title>健檢流程控制台</title>
    <script src="https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            text-align: center;
            background: #f9f9f9;
        }

        h2,
        h3 {
            margin-bottom: 10px;
        }

        .option,
        .station {
            display: inline-block;
            margin: 8px;
        }

        .btn {
            width: 140px;
            height: 60px;
            font-size: 16px;
            font-weight: bold;
            border: none;
            border-radius: 10px;
            background-color: #ddd;
            cursor: pointer;
            transition: 0.3s;
        }

        .btn.selected {
            background-color: #1E90FF;
            color: white;
        }

        .btn.done {
            background-color: #32CD32;
            color: white;
        }

        .btn.rejected {
            background-color: #FF60AF;
            color: white;
        }

        .btn.recheck {
            background-color: #FFA042;
            color: white;
        }

        .hidden {
            display: none;
        }

        #addons {
            border: 1px solid #ccc;
            padding: 15px;
            margin: 15px auto;
            border-radius: 10px;
            background: white;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
            max-width: 800px;
        }

        #signature {
            border: 2px solid #000;
            width: 300px;
            height: 150px;
            margin: 10px auto;
            cursor: crosshair;
            background: white;
        }

        #signatureContainer {
            text-align: center;
            margin: 20px 0;
        }

        .signature-controls {
            margin: 10px 0;
        }

        .signature-controls button {
            margin: 5px;
            padding: 8px 15px;
            background: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        .signature-controls button:hover {
            background: #0056b3;
        }

        #total {
            font-size: 18px;
            font-weight: bold;
            margin: 10px 0;
        }

        .package-section {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            margin: 10px 0;
        }

        .package-box {
            border: 1px solid #aaa;
            border-radius: 10px;
            padding: 10px;
            margin: 5px;
            width: 220px;
            background: #fdfdfd;
            box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
        }

        .package-box h4 {
            margin: 5px 0;
        }

        .package-items {
            margin-top: 5px;
        }

        .package-items button {
            display: block;
            width: 120px;
            height: 40px;
            margin: 5px auto;
            font-size: 14px;
            border-radius: 8px;
            border: none;
            background-color: #eee;
            cursor: pointer;
        }

        .package-items button.selected {
            background-color: #1E90FF;
            color: white;
        }

        .package-items button.exclusive-option {
            border: 2px solid #FF6B35;
            position: relative;
        }

        .package-items button.exclusive-option.selected {
            background-color: #FF6B35;
            color: white;
            border-color: #FF6B35;
        }

        .package-items button.exclusive-option:not(.selected) {
            background-color: #FFE5D9;
            color: #FF6B35;
        }

        .btn.exclusive-global {
            border: 2px solid #FF8A95;
            position: relative;
        }

        .btn.exclusive-global.selected {
            background-color: #FF8A95;
            color: white;
            border-color: #FF8A95;
        }

        .btn.exclusive-global:not(.selected) {
            background-color: #F3E5F5;
            color: #FF8A95;
        }

        .gift-item {
            width: 150px;
            height: 50px;
            margin: 5px;
            font-size: 13px;
            background-color: #FFB3BA;
            color: #333;
            border: 2px solid #FF8A95;
            transition: all 0.3s ease;
        }

        .gift-item:disabled {
            background-color: #F0F0F0;
            color: #999;
            cursor: not-allowed;
            border-color: #CCC;
        }

        .gift-item.selected {
            background-color: #FF4757;
            color: white;
            border-color: #FF3742;
            transform: scale(1.05);
        }

        .gift-item.auto-selected {
            background-color: #2ECC71;
            color: white;
            border-color: #27AE60;
        }

        #selectedOptions button {
            margin: 5px;
        }

        .single-item {
            width: 160px;
            height: 50px;
            margin: 5px;
            font-size: 14px;
        }

        .single-item.selected {
            background-color: #FF6347;
            color: white;
        }

        .status-indicator {
            position: fixed;
            top: 20px;
            right: 20px;
            background: #28a745;
            color: white;
            padding: 10px 15px;
            border-radius: 5px;
            font-size: 14px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
        }

        #freeGifts {
            border: 2px solid #FF6B6B;
            border-radius: 10px;
            padding: 15px;
            margin: 20px auto;
            max-width: 800px;
            background: linear-gradient(135deg, #FFE5E5, #FFF0F0);
            box-shadow: 0 3px 10px rgba(255, 107, 107, 0.3);
        }

        #freeGifts h3 {
            color: #FF4757;
            margin-bottom: 15px;
            font-size: 20px;
        }

        .additional-items {
            border: 2px solid #FF8C00;
            border-radius: 15px;
            padding: 20px;
            margin: 20px auto;
            max-width: 900px;
            background: linear-gradient(135deg, #FFF8DC, #FFFACD);
            box-shadow: 0 4px 15px rgba(255, 140, 0, 0.2);
        }

        .additional-items-header {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 10px;
            margin-bottom: 15px;
        }

        .additional-items h3 {
            color: #FF6600;
            font-size: 18px;
            text-align: center;
            border-bottom: 2px solid #FF8C00;
            padding-bottom: 8px;
            margin: 0;
        }

        .basic-checkup {
            border: 2px solid #4682B4;
            border-radius: 15px;
            padding: 20px;
            margin: 20px auto;
            max-width: 900px;
            background: linear-gradient(135deg, #E6F3FF, #F0F8FF);
            box-shadow: 0 4px 15px rgba(70, 130, 180, 0.2);
        }

        .basic-checkup-header {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 10px;
            margin-bottom: 15px;
        }

        .basic-checkup h3 {
            color: #4682B4;
            font-size: 18px;
            text-align: center;
            border-bottom: 2px solid #4682B4;
            padding-bottom: 8px;
            margin: 0;
        }

        .basic-checkup-input {
            width: 100px;
            padding: 8px;
            font-size: 14px;
            border: 1px solid #ccc;
            border-radius: 5px;
            text-align: center;
        }

        .basic-checkup-confirm {
            padding: 8px 15px;
            background: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
        }

        .basic-checkup-confirm:hover {
            background: #0056b3;
        }

        .basic-checkup-correct {
            padding: 8px 15px;
            background: #FF8C00;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
        }

        .basic-checkup-correct:hover {
            background: #e07b00;
        }

        .selected-options {
            border: 2px solid #32CD32;
            border-radius: 15px;
            padding: 20px;
            margin: 20px auto;
            max-width: 900px;
            background: linear-gradient(135deg, #F0FFF0, #F5FFFA);
            box-shadow: 0 4px 15px rgba(50, 205, 50, 0.2);
        }

        .selected-options h3 {
            color: #228B22;
            margin-bottom: 15px;
            font-size: 18px;
            text-align: center;
            border-bottom: 2px solid #32CD32;
            padding-bottom: 8px;
        }

        .exclusive-hint {
            background: #FFF3E0;
            border: 1px solid #FF9800;
            border-radius: 5px;
            padding: 8px;
            margin: 10px 0;
            font-size: 12px;
            color: #E65100;
        }

        .input-section {
            margin: 20px auto;
            max-width: 600px;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 15px;
        }

        .input-group {
            display: flex;
            flex-direction: row;
            flex-wrap: wrap;
            gap: 10px;
            justify-content: center;
        }

        .input-group input {
            width: 120px;
            padding: 8px;
            font-size: 14px;
            border: 1px solid #ccc;
            border-radius: 5px;
            text-align: center;
        }

        .button-group {
            display: flex;
            flex-direction: row;
            flex-wrap: wrap;
            gap: 10px;
            justify-content: center;
        }

        .input-section button {
            padding: 8px 20px;
            background: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            width: 80px;
            height: 36px;
            font-size: 14px;
            writing-mode: horizontal-tb !important;
            text-orientation: mixed !important;
        }

        .input-section .correct-btn {
            background: #FF8C00;
        }

        .input-section .ap-btn {
            background-color: #FF69B4 !important;
            color: white !important;
        }

        .input-section button:hover {
            background: #0056b3;
        }

        .input-section .correct-btn:hover {
            background: #e07b00;
        }

        .info-display {
            margin: 10px auto;
            font-size: 32px;
            font-weight: bold;
            color: #333;
        }

        .barcode-container {
            margin: 10px auto;
        }

        .c13-input-container {
            display: inline-flex;
            align-items: center;
            gap: 10px;
        }

        .c13-input {
            width: 100px;
            padding: 8px;
            font-size: 14px;
            border: 1px solid #ccc;
            border-radius: 5px;
            text-align: center;
        }

        .c13-confirm {
            padding: 8px 15px;
            background: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
        }

        .c13-confirm:hover {
            background: #0056b3;
        }

        .c13-correct {
            padding: 8px 15px;
            background: #FF8C00;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
        }

        .c13-correct:hover {
            background: #e07b00;
        }
    </style>
</head>

<body>
    <div class="status-indicator" id="statusIndicator">已選擇: 0/2</div>

    <div id="page1">
        <div class="input-section">
            <div class="input-group">
                <input type="text" inputmode="numeric" id="barcodeInput" placeholder="輸入條碼號碼">
                <input type="text" inputmode="numeric" id="serialInput" placeholder="輸入流水號">
                <input type="number" id="ageInput" placeholder="輸入年齡" min="0" max="150">
            </div>
            <div class="button-group">
                <button onclick="confirmInputs()">確定</button>
                <button class="correct-btn" onclick="correctInputs()">更改</button>
                <button class="btn ap-btn" onclick="toggleAP()">AP</button>
            </div>
        </div>
        <div id="infoDisplayPage1" class="info-display"></div>
        <canvas id="barcodePage1" class="barcode-container"></canvas>

        <h2>公費項目 (選2)</h2>
        <div id="options">
            <div class="option">
                <button class="btn" id="opt1" onclick="toggleOption(this)">腹部超音波</button>
            </div>
            <div class="option">
                <button class="btn" id="opt2" onclick="toggleOption(this)">婦科超音波</button>
            </div>
            <div class="option">
                <button class="btn" id="opt3" onclick="toggleOption(this)">頸動脈超音波</button>
            </div>
            <div class="option">
                <button class="btn" id="opt4" onclick="toggleOption(this)">甲狀腺超音波</button>
            </div>
            <div class="option">
                <button class="btn" id="opt5" onclick="toggleOption(this)">攝護腺超音波</button>
            </div>
            <div class="option">
                <button class="btn" id="opt6" onclick="toggleOption(this)">乳房超音波</button>
            </div>
        </div>

        <div id="specialOperations">
            <h3>特殊作業</h3>
            <div class="package-section">
                <button class="btn" id="spec03" onclick="toggleSpecial('03')">03游離輻射</button>
                <button class="btn" id="spec05" onclick="toggleSpecial('05')">05鉛</button>
                <button class="btn" id="spec12" onclick="toggleSpecial('12')">12正己烷</button>
            </div>
        </div>

        <div style="margin-top:20px;">
            <button class="btn" onclick="toggleAddons()">加做項目</button>
        </div>

        <div id="addons" class="hidden">
            <h3>加做項目套餐</h3>
            <div id="top-packages" class="package-section">
                <div class="package-box">
                    <button class="btn" id="pkgA" onclick="togglePackage('A')">A套餐 優惠價3000元</button>
                    <div class="package-items" id="itemsA">
                        <button data-price="900" class="pkg-item" data-pkg="A" id="pkgA1">同半胱胺酸 900元</button>
                        <button data-price="900" class="pkg-item" data-pkg="A" id="pkgA2">高敏感C反應蛋白 900元</button>
                        <button data-price="800" class="pkg-item" data-pkg="A" id="pkgA3">心肌旋轉蛋白 800元</button>
                        <button data-price="1600" class="pkg-item" data-pkg="A" id="pkgA4">B型利納肽前驅物 1600元</button>
                        <button data-price="1200" class="pkg-item exclusive-option" data-pkg="A" data-single="true" id="pkgA5">頸動脈超音波 1200元</button>
                        <button data-price="1200" class="pkg-item exclusive-option" data-pkg="A" data-single="true" id="pkgA6">眼底攝影 1200元</button>
                    </div>
                    <div class="exclusive-hint">
                        💡 頸動脈超音波與眼底攝影二選一
                    </div>
                </div>
                <div class="package-box">
                    <button class="btn" id="pkgB" onclick="togglePackage('B')">B套餐 優惠價2200元</button>
                    <div class="package-items" id="itemsB">
                        <button data-price="900" class="pkg-item" data-pkg="B" id="pkgB1">甲狀腺功能 900元</button>
                        <button data-price="800" class="pkg-item" data-pkg="B" id="pkgB2">自體免疫疾病檢查 800元</button>
                        <button data-price="500" class="pkg-item" data-pkg="B" id="pkgB3">電解質檢查 500元</button>
                        <button data-price="600" class="pkg-item" data-pkg="B" id="pkgB4">B肝抗原抗體 600元</button>
                        <button data-price="600" class="pkg-item" data-pkg="B" id="pkgB5">C肝檢查 600元</button>
                        <button data-price="800" class="pkg-item" data-pkg="B" id="pkgB6">胰島素 800元</button>
                        <button data-price="800" class="pkg-item" data-pkg="B" id="pkgB7">維生素D 800元</button>
                    </div>
                </div>
                <div class="package-box">
                    <button class="btn" id="pkgC" onclick="togglePackage('C')">C套餐 優惠價2500元</button>
                    <div class="package-items" id="itemsC">
                        <button data-price="500" class="pkg-item" data-pkg="C" id="pkgC1">解脂酶 500元</button>
                        <button data-price="800" class="pkg-item" data-pkg="C" id="pkgC2">胃癌 800元</button>
                        <button data-price="700" class="pkg-item" data-pkg="C" id="pkgC3">胰臟癌 700元</button>
                        <button data-price="1200" class="pkg-item" data-pkg="C" id="pkgC4">C13 1200元</button>
                    </div>
                </div>
            </div>

            <div id="bottom-packages" class="package-section">
                <div class="package-box">
                    <button class="btn" id="pkgD" onclick="togglePackage('D')">D套餐(男) 優惠價2000元</button>
                    <div class="package-items" id="itemsD">
                        <button data-price="800" class="pkg-item" data-pkg="D" id="pkgD1">鼻咽癌 800元</button>
                        <button data-price="800" class="pkg-item" data-pkg="D" id="pkgD2">鱗狀上皮細胞癌 800元</button>
                        <button data-price="800" class="pkg-item" data-pkg="D" id="pkgD3">肺腺癌 800元</button>
                        <button data-price="700" class="pkg-item" data-pkg="D" id="pkgD4">肺癌 700元</button>
                    </div>
                </div>
                <div class="package-box">
                    <button class="btn" id="pkgE" onclick="togglePackage('E')">E套餐(女) 優惠價2000元</button>
                    <div class="package-items" id="itemsE">
                        <button data-price="800" class="pkg-item" data-pkg="E" id="pkgE1">鱗狀上皮細胞癌 800元</button>
                        <button data-price="800" class="pkg-item" data-pkg="E" id="pkgE2">肺腺癌 800元</button>
                        <button data-price="800" class="pkg-item" data-pkg="E" id="pkgE3">卵巢癌 800元</button>
                        <button data-price="700" class="pkg-item" data-pkg="E" id="pkgE4">肺癌 700元</button>
                    </div>
                </div>
                <div class="package-box">
                    <button class="btn" id="pkgF" onclick="togglePackage('F')">F套餐 優惠價2000元</button>
                    <div class="package-items" id="itemsF">
                        <button data-price="900" class="pkg-item" data-pkg="F" id="pkgF1">腎上腺皮質素 900元</button>
                        <button data-price="600" class="pkg-item" data-pkg="F" id="pkgF2">睪固酮(男) 600元</button>
                        <button data-price="600" class="pkg-item" data-pkg="F" id="pkgF3">雌二醇(女) 600元</button>
                        <button data-price="600" class="pkg-item" data-pkg="F" id="pkgF4">黃體生成激素 600元</button>
                        <button data-price="600" class="pkg-item" data-pkg="F" id="pkgF5">濾泡刺激素 600元</button>
                    </div>
                </div>
            </div>

            <div style="margin: 20px 0;">
                <h3>單項加做項目</h3>
                <div class="package-section">
                    <button data-price="1200" class="single-item btn" id="singleHRV">HRV 1200元</button>
                    <button data-price="600" class="single-item btn" id="singleAbdominal">腹部超音波 600元</button>
                    <button data-price="600" class="single-item btn" id="singleThyroid">甲狀腺超音波 600元</button>
                    <button data-price="600" class="single-item btn" id="singleProstate">攝護腺超音波 600元</button>
                    <button data-price="800" class="single-item btn" id="singleBreast">乳房超音波 800元</button>
                    <button data-price="600" class="single-item btn" id="singleGynecology">婦科超音波 600元</button>
                    <button data-price="2500" class="single-item btn" id="singleE66">E66 2500元</button>
                    <button data-price="4800" class="single-item btn" id="singleE110">E110 4800元</button>
                </div>
            </div>

            <div id="total">總金額: 0 元</div>

            <div id="freeGifts">
                <h3>🎁 免費贈送項目</h3>
                <div class="gift-info">
                    滿7500元可選擇1項超音波檢查 | 滿9000元自動加贈IGE免疫球蛋白
                </div>
                <div class="package-section">
                    <button class="gift-item btn" id="giftAbdominal" onclick="selectGift(this)" disabled>腹部超音波</button>
                    <button class="gift-item btn" id="giftGynecology" onclick="selectGift(this)" disabled>婦科超音波</button>
                    <button class="gift-item btn" id="giftBreast" onclick="selectGift(this)" disabled>乳房超音波</button>
                    <button class="gift-item btn" id="giftThyroid" onclick="selectGift(this)" disabled>甲狀腺超音波</button>
                    <button class="gift-item btn" id="giftCarotid" onclick="selectGift(this)" disabled>頸動脈超音波</button>
                    <button class="gift-item btn" id="giftProstate" onclick="selectGift(this)" disabled>攝護腺超音波</button>
                    <button class="gift-item btn" id="giftIGE" onclick="selectGift(this)" disabled>IGE免疫球蛋白</button>
                </div>
            </div>

            <div id="signatureContainer">
                <h4>請在下方簽名：</h4>
                <canvas id="signature" width="300" height="150"></canvas>
                <div class="signature-controls">
                    <button onclick="clearSignature()">清除簽名</button>
                </div>
            </div>

            <button class="btn" onclick="takeScreenshot()">長截圖</button>
            <br />
            <br />
        </div>

        <button class="btn" onclick="confirmPage1()">OK</button>
    </div>

    <div id="page2" class="hidden">
        <div id="infoDisplayPage2" class="info-display"></div>
        <canvas id="barcodePage2" class="barcode-container"></canvas>
        <h2>健檢關卡進度</h2>

        <div class="basic-checkup">
            <div class="basic-checkup-header">
                <h3>🏥 基本檢查關卡</h3>
                <input type="number" id="basicCheckupInput" class="basic-checkup-input" placeholder="輸入數值" min="0">
                <button class="basic-checkup-confirm" onclick="confirmBasicCheckupInput()">確定</button>
                <button class="basic-checkup-correct" onclick="correctBasicCheckupInput()">更正</button>
            </div>
            <div class="station">
                <button class="btn" id="weightFat" onclick="markDone(this)">體重體脂</button>
            </div>
            <div class="station">
                <button class="btn" id="boneDensity" onclick="markDone(this)">骨質密度</button>
            </div>
            <div class="station">
                <button class="btn" id="waistHearing" onclick="markDone(this)">腰圍聽力</button>
            </div>
            <div class="station">
                <button class="btn" id="vision" onclick="markDone(this)">視力</button>
            </div>
            <div class="station">
                <button class="btn" id="eyePressure" onclick="markDone(this)">眼壓</button>
            </div>
            <div class="station">
                <button class="btn" id="bloodPressure" onclick="markDone(this)">血壓</button>
            </div>
            <div class="station">
                <button class="btn" id="consultation" onclick="markDone(this)">諮詢領管</button>
            </div>
            <div class="station">
                <button class="btn" id="blood" onclick="markDone(this)">抽血</button>
            </div>
            <div class="station">
                <button class="btn" id="ecg" onclick="markDone(this)">心電圖</button>
            </div>
            <div class="station">
                <button class="btn" id="doctor" onclick="markDone(this)">醫師</button>
            </div>
            <div class="station">
                <button class="btn" id="urine" onclick="markDone(this)">尿液</button>
            </div>
            <div class="station">
                <button class="btn" id="xray" onclick="markDone(this)">X光</button>
            </div>
        </div>

        <div class="selected-options">
            <h3>🔹 公費項目 (選2)</h3>
            <div id="selectedOptions"></div>
        </div>

        <div class="additional-items">
            <div class="additional-items-header">
                <h3>🔸 加做項目</h3>
                <div id="c13InputContainer" class="c13-input-container hidden">
                    <input type="text" id="c13Input" class="c13-input">
                    <button class="c13-confirm" onclick="confirmC13Input()">確定</button>
                    <button class="c13-correct" onclick="correctC13Input()">更改</button>
                </div>
            </div>
            <div id="additionalItems"></div>
        </div>

        <br />
        <button class="btn" onclick="goBack()">返回</button>
        <button class="btn" style="background-color: red; color: white;" onclick="uploadData()">資料回傳</button>
    </div>

    <script>
        let selected = [];
        let selectedButtons = {};
        let packageTotals = {
            A: 0,
            B: 0,
            C: 0,
            D: 0,
            E: 0,
            F: 0
        };
        let selectedGift = null;
        let currentPage = 1;
        let addonsVisible = false;
        let isDrawing = false;
        let canvas, ctx;
        let barcodeValue = '';
        let serialNumber = '';
        let age = '';
        let basicCheckupValue = '';
        let inputsLocked = false;
        let c13InputValue = '';
        let c13InputLocked = false;
        let specialSelected = [];
        let isAP = false;

        const allowedItems = [
            '頸動脈超音波', '眼底攝影', 'C13', 'HRV', '腹部超音波',
            '甲狀腺超音波', '攝護腺超音波', '乳房超音波', '婦科超音波', 'E66', 'E110'
        ];

        function extractItemName(text) {
            return text.replace(/\s+\d+元$/, '').trim();
        }

        function isAllowedItem(itemText) {
            const itemName = extractItemName(itemText);
            return allowedItems.includes(itemName);
        }

        function handlePackageAExclusive(clickedBtn) {
            const pkgA5 = document.getElementById("pkgA5");
            const pkgA6 = document.getElementById("pkgA6");
            if (clickedBtn.id === "pkgA5") {
                pkgA5.classList.add("selected");
                pkgA6.classList.remove("selected");
            } else if (clickedBtn.id === "pkgA6") {
                pkgA6.classList.add("selected");
                pkgA5.classList.remove("selected");
            }
        }

        function clearAllCarotidSelections(source) {
            const carotidSelectors = ['#pkgA5', '#giftCarotid'];
            if (source !== 'pkgA5') {
                carotidSelectors.push('#opt3');
            }
            carotidSelectors.forEach(selector => {
                const btn = document.querySelector(selector);
                if (btn && btn.classList.contains('selected')) {
                    btn.classList.remove('selected');
                    if (selector === '#opt3') {
                        selected = selected.filter(x => x !== '頸動脈超音波');
                        delete selectedButtons['opt3'];
                    }
                    if (selector === '#giftCarotid' && selectedGift === 'giftCarotid') {
                        selectedGift = null;
                    }
                }
            });
        }

        function clearAllEyeSelections(source) {
            const eyeSelectors = ['#pkgA6'];
            if (source !== 'pkgA6') {
                eyeSelectors.push('#opt8');
            }
            eyeSelectors.forEach(selector => {
                const btn = document.querySelector(selector);
                if (btn && btn.classList.contains('selected')) {
                    btn.classList.remove('selected');
                    if (selector === '#opt8') {
                        selected = selected.filter(x => x !== '眼底攝影');
                        delete selectedButtons['opt8'];
                    }
                }
            });
        }

        function generateBarcode(value, canvasId) {
            if (value && !isNaN(value) && value.trim() !== '') {
                JsBarcode(`#${canvasId}`, value, {
                    format: "CODE128",
                    displayValue: true,
                    height: 80,
                    width: 3,
                    fontSize: 24
                });
            } else {
                const canvas = document.getElementById(canvasId);
                const ctx = canvas.getContext('2d');
                ctx.clearRect(0, 0, canvas.width, canvas.height);
            }
        }

        function displayInfo() {
            const display1 = document.getElementById('infoDisplayPage1');
            const display2 = document.getElementById('infoDisplayPage2');
            const text = (serialNumber ? `流水號: ${serialNumber}` : '') + (serialNumber && age ? '   ' : '') + (age ?
                `年齡: ${age}` : '');
            display1.textContent = text;
            display2.textContent = text;
            generateBarcode(barcodeValue, 'barcodePage1');
            generateBarcode(barcodeValue, 'barcodePage2');
        }

        function confirmInputs() {
            barcodeValue = document.getElementById('barcodeInput').value.trim();
            serialNumber = document.getElementById('serialInput').value.trim();
            age = document.getElementById('ageInput').value.trim();
            if (barcodeValue === '' && serialNumber === '' && age === '') {
                alert('請至少輸入一個欄位！');
                return;
            }
            if (age && (isNaN(age) || age < 0 || age > 150)) {
                alert('請輸入有效的年齡（0-150）！');
                return;
            }
            inputsLocked = true;
            document.getElementById('barcodeInput').disabled = true;
            document.getElementById('serialInput').disabled = true;
            document.getElementById('ageInput').disabled = true;
            displayInfo();
            updateURL();
        }

        function correctInputs() {
            const pwd = prompt('請輸入驗證碼');
            if (pwd && pwd.toLowerCase() === 'x') {
                inputsLocked = false;
                document.getElementById('barcodeInput').disabled = false;
                document.getElementById('serialInput').disabled = false;
                document.getElementById('ageInput').disabled = false;
            } else {
                alert('驗證碼錯誤，請輸入正確的驗證碼！');
            }
        }

        function confirmBasicCheckupInput() {
            const inputValue = document.getElementById('basicCheckupInput').value.trim();
            if (inputValue === '' || isNaN(inputValue) || inputValue < 0) {
                alert('請輸入有效的非負數值！');
                return;
            }
            basicCheckupValue = inputValue;
            document.getElementById('basicCheckupInput').disabled = true;
            updateURL();
        }

        function correctBasicCheckupInput() {
            const pwd = prompt('請輸入驗證碼');
            if (pwd && pwd.toUpperCase() === 'X') {
                document.getElementById('basicCheckupInput').disabled = false;
            } else {
                alert('驗證碼錯誤，請輸入正確的驗證碼！');
            }
        }

        function confirmC13Input() {
            const input = document.getElementById('c13Input');
            c13InputValue = input.value.trim();
            if (c13InputValue === '') {
                alert('請輸入C13相關資料！');
                return;
            }
            c13InputLocked = true;
            input.disabled = true;
            updateURL();
        }

        function correctC13Input() {
            const pwd = prompt('請輸入驗證碼');
            if (pwd && pwd.toUpperCase() === 'X') {
                c13InputLocked = false;
                document.getElementById('c13Input').disabled = false;
            } else {
                alert('驗證碼錯誤，請輸入正確的驗證碼！');
            }
        }

        function toggleSpecial(code) {
            const btn = document.getElementById(`spec${code}`);
            if (btn.classList.contains('selected')) {
                btn.classList.remove('selected');
                specialSelected = specialSelected.filter(c => c !== code);
            } else {
                btn.classList.add('selected');
                specialSelected.push(code);
            }
            updateURL();
        }

        function toggleAP() {
            isAP = !isAP;
            const apButton = document.querySelector('button[onclick="toggleAP()"]');
            if (isAP) {
                apButton.classList.add('ap-selected');
                document.body.style.background = '#FFC1E0';
                const xrayBtn = document.getElementById('xray');
                if (xrayBtn) {
                    xrayBtn.classList.add('rejected');
                    if (!xrayBtn.textContent.includes('🚫')) xrayBtn.textContent += ' 🚫';
                }
            } else {
                apButton.classList.remove('ap-selected');
                document.body.style.background = '#f9f9f9';
                const xrayBtn = document.getElementById('xray');
                if (xrayBtn) {
                    xrayBtn.classList.remove('rejected');
                    xrayBtn.textContent = xrayBtn.textContent.replace(' 🚫', '');
                }
            }
            updateURL();
        }

        function updateURL() {
            const state = {
                page: currentPage,
                selected: selected.join(','),
                selectedButtons: Object.keys(selectedButtons).join(','),
                packages: Object.keys(packageTotals).filter(k => packageTotals[k] > 0).join(','),
                pkgItems: [],
                singleItems: [],
                gift: selectedGift,
                addons: addonsVisible ? '1' : '0',
                done: [],
                rejected: [],
                recheck: [],
                barcode: barcodeValue,
                serial: serialNumber,
                age: age,
                basicCheckup: basicCheckupValue,
                inputsLocked: inputsLocked ? '1' : '0',
                c13Input: c13InputValue,
                c13InputLocked: c13InputLocked ? '1' : '0',
                special: specialSelected.join(','),
                ap: isAP ? '1' : '0',
                price: window.totalAmount || 0
            };

            document.querySelectorAll(".basic-checkup .btn").forEach(btn => {
                if (btn.classList.contains('done')) {
                    state.done.push(btn.id);
                } else if (btn.classList.contains('rejected')) {
                    state.rejected.push(btn.id);
                } else if (btn.classList.contains('recheck')) {
                    state.recheck.push(btn.id);
                }
            });

            document.querySelectorAll("#selectedOptions .btn").forEach(btn => {
                const id = btn.id;
                if (btn.classList.contains('done')) {
                    state.done.push('dynamic-' + id);
                } else if (btn.classList.contains('rejected')) {
                    state.rejected.push('dynamic-' + id);
                } else if (btn.classList.contains('recheck')) {
                    state.recheck.push('dynamic-' + id);
                }
            });

            document.querySelectorAll("#additionalItems .btn").forEach(btn => {
                const id = btn.id;
                if (btn.classList.contains('done')) {
                    state.done.push('dynamic-' + id);
                } else if (btn.classList.contains('rejected')) {
                    state.rejected.push('dynamic-' + id);
                } else if (btn.classList.contains('recheck')) {
                    state.recheck.push('dynamic-' + id);
                }
            });

            document.querySelectorAll(".special-operations .btn").forEach(btn => {
                const id = btn.id;
                if (btn.classList.contains('done')) {
                    state.done.push('dynamic-' + id);
                } else if (btn.classList.contains('rejected')) {
                    state.rejected.push('dynamic-' + id);
                } else if (btn.classList.contains('recheck')) {
                    state.recheck.push('dynamic-' + id);
                }
            });

            document.querySelectorAll(".pkg-item.selected").forEach(btn => {
                state.pkgItems.push(btn.id);
            });
            document.querySelectorAll(".single-item.selected").forEach(btn => {
                state.singleItems.push(btn.id);
            });

            const params = new URLSearchParams();
            Object.keys(state).forEach(key => {
                if (Array.isArray(state[key]) && state[key].length > 0) {
                    params.set(key, state[key].join(','));
                } else if (state[key] !== null && state[key] !== '' && state[key] !== undefined) {
                    params.set(key, state[key]);
                }
            });

            const newURL = window.location.pathname + '?' + params.toString();
            window.history.replaceState({}, '', newURL);
        }

        function loadFromURL() {
            const params = new URLSearchParams(window.location.search);
            currentPage = parseInt(params.get('page')) || 1;
            const selectedItems = params.get('selected');
            if (selectedItems) {
                selected = selectedItems.split(',').filter(s => s);
            }
            const selectedBtns = params.get('selectedButtons');
            if (selectedBtns) {
                selectedBtns.split(',').forEach(btnId => {
                    if (btnId) {
                        selectedButtons[btnId] = true;
                        const btn = document.getElementById(btnId);
                        if (btn) btn.classList.add('selected');
                    }
                });
            }
            const packages = params.get('packages');
            if (packages) {
                packages.split(',').forEach(pkg => {
                    if (pkg) {
                        packageTotals[pkg] = getPackagePrice(pkg);
                        const btn = document.getElementById('pkg' + pkg);
                        if (btn) btn.classList.add('selected');
                        if (pkg === 'A') {
                            document.querySelectorAll("#itemsA .pkg-item").forEach(innerBtn => {
                                innerBtn.classList.add("selected");
                            });
                            const pkgItems = params.get('pkgItems');
                            if (pkgItems && pkgItems.includes('pkgA6')) {
                                document.getElementById("pkgA5").classList.remove("selected");
                                document.getElementById("pkgA6").classList.add("selected");
                            } else {
                                document.getElementById("pkgA5").classList.add("selected");
                                document.getElementById("pkgA6").classList.remove("selected");
                            }
                        }
                    }
                });
            }
            const pkgItems = params.get('pkgItems');
            if (pkgItems) {
                pkgItems.split(',').forEach(itemId => {
                    if (itemId) {
                        const btn = document.getElementById(itemId);
                        if (btn) btn.classList.add('selected');
                    }
                });
            }
            const singleItems = params.get('singleItems');
            if (singleItems) {
                singleItems.split(',').forEach(itemId => {
                    if (itemId) {
                        const btn = document.getElementById(itemId);
                        if (btn) btn.classList.add('selected');
                    }
                });
            }
            const special = params.get('special');
            if (special) {
                specialSelected = special.split(',').filter(c => c);
                specialSelected.forEach(code => {
                    const btn = document.getElementById(`spec${code}`);
                    if (btn) btn.classList.add('selected');
                });
            }
            selectedGift = params.get('gift');
            if (selectedGift) {
                const giftBtn = document.getElementById(selectedGift);
                if (giftBtn) giftBtn.classList.add('selected');
            }
            addonsVisible = params.get('addons') === '1';
            if (addonsVisible) {
                document.getElementById('addons').classList.remove('hidden');
            }
            barcodeValue = params.get('barcode') || '';
            serialNumber = params.get('serial') || '';
            age = params.get('age') || '';
            basicCheckupValue = params.get('basicCheckup') || '';
            inputsLocked = params.get('inputsLocked') === '1';
            c13InputValue = params.get('c13Input') || '';
            c13InputLocked = params.get('c13InputLocked') === '1';
            isAP = params.get('ap') === '1';
            if (barcodeValue || serialNumber || age) {
                document.getElementById('barcodeInput').value = barcodeValue;
                document.getElementById('serialInput').value = serialNumber;
                document.getElementById('ageInput').value = age;
                if (inputsLocked) {
                    document.getElementById('barcodeInput').disabled = true;
                    document.getElementById('serialInput').disabled = true;
                    document.getElementById('ageInput').disabled = true;
                }
                displayInfo();
            }
            if (basicCheckupValue) {
                document.getElementById('basicCheckupInput').value = basicCheckupValue;
                document.getElementById('basicCheckupInput').disabled = true;
            }
            if (isAP) {
                document.body.style.background = '#FFC1E0';
                const xrayBtn = document.getElementById('xray');
                if (xrayBtn) {
                    xrayBtn.classList.add('rejected');
                    if (!xrayBtn.textContent.includes('🚫')) xrayBtn.textContent += ' 🚫';
                }
            }

            const doneItems = params.get('done') ? params.get('done').split(',') : [];
            const rejectedItems = params.get('rejected') ? params.get('rejected').split(',') : [];
            const recheckItems = params.get('recheck') ? params.get('recheck').split(',') : [];

            document.querySelectorAll(".basic-checkup .btn").forEach(btn => {
                if (doneItems.includes(btn.id)) {
                    btn.classList.add('done');
                    if (!btn.textContent.includes('✅')) btn.textContent += ' ✅';
                } else if (rejectedItems.includes(btn.id)) {
                    btn.classList.add('rejected');
                    if (!btn.textContent.includes('🚫')) btn.textContent += ' 🚫';
                } else if (recheckItems.includes(btn.id)) {
                    btn.classList.add('recheck');
                    if (!btn.textContent.includes('🔄')) btn.textContent += ' 🔄';
                }
            });

            if (currentPage === 2) {
                document.getElementById('page1').classList.add('hidden');
                document.getElementById('page2').classList.remove('hidden');
                renderSelectedOptions();
                setTimeout(() => {
                    document.querySelectorAll(
                        "#selectedOptions .btn, #additionalItems .btn, .special-operations .btn").forEach(
                        btn => {
                            const dynamicId = 'dynamic-' + btn.id;
                            if (doneItems.includes(dynamicId)) {
                                btn.classList.add('done');
                                if (!btn.textContent.includes('✅')) btn.textContent += ' ✅';
                            } else if (rejectedItems.includes(dynamicId)) {
                                btn.classList.add('rejected');
                                if (!btn.textContent.includes('🚫')) btn.textContent += ' 🚫';
                            } else if (recheckItems.includes(dynamicId)) {
                                btn.classList.add('recheck');
                                if (!btn.textContent.includes('🔄')) btn.textContent += ' 🔄';
                            }
                        });
                }, 100);
            }
            updateStatusIndicator();
            updateTotal();
        }

        function initSignature() {
            canvas = document.getElementById('signature');
            ctx = canvas.getContext('2d');
            ctx.strokeStyle = '#000000';
            ctx.lineWidth = 2;
            ctx.lineCap = 'round';
            canvas.addEventListener('mousedown', startDrawing);
            canvas.addEventListener('mousemove', draw);
            canvas.addEventListener('mouseup', stopDrawing);
            canvas.addEventListener('mouseout', stopDrawing);
            canvas.addEventListener('touchstart', handleTouch);
            canvas.addEventListener('touchmove', handleTouch);
            canvas.addEventListener('touchend', stopDrawing);
        }

        function startDrawing(e) {
            isDrawing = true;
            const rect = canvas.getBoundingClientRect();
            ctx.beginPath();
            ctx.moveTo(e.clientX - rect.left, e.clientY - rect.top);
        }

        function draw(e) {
            if (!isDrawing) return;
            const rect = canvas.getBoundingClientRect();
            ctx.lineTo(e.clientX - rect.left, e.clientY - rect.top);
            ctx.stroke();
        }

        function stopDrawing() {
            if (isDrawing) {
                isDrawing = false;
                saveSignature();
            }
        }

        function handleTouch(e) {
            e.preventDefault();
            const touch = e.touches[0];
            const rect = canvas.getBoundingClientRect();
            if (e.type === 'touchstart') {
                isDrawing = true;
                ctx.beginPath();
                ctx.moveTo(touch.clientX - rect.left, touch.clientY - rect.top);
            } else if (e.type === 'touchmove' && isDrawing) {
                ctx.lineTo(touch.clientX - rect.left, touch.clientY - rect.top);
                ctx.stroke();
            }
        }

        function clearSignature() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
        }

        function saveSignature() {
            const signatureData = canvas.toDataURL();
        }

        function updateStatusIndicator() {
            const indicator = document.getElementById('statusIndicator');
            indicator.textContent = `已選擇: ${selected.length}/2`;
            indicator.style.backgroundColor = selected.length === 2 ? '#28a745' : '#ffc107';
        }

        function selectGift(btn) {
            if (btn.disabled) return;
            if (btn.id === 'giftIGE') return;
            document.querySelectorAll('.gift-item:not(#giftIGE)').forEach(giftBtn => {
                giftBtn.classList.remove('selected');
            });
            btn.classList.add('selected');
            selectedGift = btn.id;
            updateStatusIndicator();
            updateURL();
        }

        function updateGiftAvailability(totalAmount) {
            const ultrasoundGifts = ['giftAbdominal', 'giftGynecology', 'giftBreast', 'giftThyroid', 'giftCarotid',
                'giftProstate'
            ];
            const igeGift = document.getElementById('giftIGE');
            ultrasoundGifts.forEach(giftId => {
                const giftBtn = document.getElementById(giftId);
                if (totalAmount >= 7500) {
                    giftBtn.disabled = false;
                    giftBtn.style.cursor = 'pointer';
                } else {
                    giftBtn.disabled = true;
                    giftBtn.classList.remove('selected');
                    giftBtn.style.cursor = 'not-allowed';
                }
            });
            if (totalAmount >= 9000) {
                igeGift.disabled = false;
                igeGift.classList.add('auto-selected');
                igeGift.classList.remove('selected');
            } else {
                igeGift.disabled = true;
                igeGift.classList.remove('auto-selected', 'selected');
            }
            if (totalAmount < 7500) selectedGift = null;
        }

        async function takeScreenshot() {
            try {
                const script = document.createElement('script');
                script.src = 'https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js';
                document.head.appendChild(script);
                script.onload = async function () {
                    try {
                        const loadingMsg = document.createElement('div');
                        loadingMsg.textContent = '正在生成截圖...';
                        loadingMsg.style.position = 'fixed';
                        loadingMsg.style.top = '50%';
                        loadingMsg.style.left = '50%';
                        loadingMsg.style.transform = 'translate(-50%, -50%)';
                        loadingMsg.style.background = '#007bff';
                        loadingMsg.style.color = 'white';
                        loadingMsg.style.padding = '20px';
                        loadingMsg.style.borderRadius = '10px';
                        loadingMsg.style.zIndex = '9999';
                        loadingMsg.style.fontSize = '18px';
                        document.body.appendChild(loadingMsg);
                        const screenshotCanvas = await html2canvas(document.body, {
                            useCORS: true,
                            allowTaint: true,
                            scale: 1,
                            backgroundColor: '#ffffff',
                            removeContainer: false,
                            foreignObjectRendering: true
                        });
                        document.body.removeChild(loadingMsg);
                        screenshotCanvas.toBlob(async function (blob) {
                            try {
                                if (navigator.clipboard && window.ClipboardItem) {
                                    await navigator.clipboard.write([new ClipboardItem({
                                        'image/png': blob
                                    })]);
                                    alert('完整頁面截圖已複製到剪貼簿！\n可以直接貼到其他應用程式中使用。');
                                } else {
                                    throw new Error('剪貼簿API不可用');
                                }
                            } catch (err) {
                                console.error('複製到剪貼簿失敗:', err);
                                const url = URL.createObjectURL(blob);
                                const a = document.createElement('a');
                                a.href = url;
                                a.download = '健檢流程-' + new Date().toISOString().slice(0, 10) + '.png';
                                a.click();
                                URL.revokeObjectURL(url);
                                alert('完整頁面截圖已下載到本機！');
                            }
                        }, 'image/png', 1.0);
                    } catch (error) {
                        console.error('截圖失敗:', error);
                        alert('截圖功能出現問題，請嘗試以下備用方案：\n1. 使用瀏覽器列印功能（Ctrl+P）\n2. 使用瀏覽器開發者工具的裝置模擬功能截圖');
                    }
                };
                script.onerror = function () {
                    alert('截圖功能載入失敗，請檢查網路連線或使用瀏覽器列印功能（Ctrl+P）');
                };
            } catch (error) {
                console.error('截圖功能初始化失敗:', error);
                alert('截圖功能暫不可用，建議使用瀏覽器列印功能（Ctrl+P）儲存頁面');
            }
        }

        function toggleOption(btn) {
            if (btn.classList.contains("selected")) {
                btn.classList.remove("selected");
                selected = selected.filter(x => x !== btn.textContent);
                delete selectedButtons[btn.id];
            } else {
                if (selected.length >= 2) {
                    alert("最多選2個");
                    return;
                }
                btn.classList.add("selected");
                selected.push(btn.textContent);
                selectedButtons[btn.id] = true;
            }
            updateStatusIndicator();
            updateURL();
        }

        function toggleAddons() {
            document.getElementById("addons").classList.toggle("hidden");
            addonsVisible = !document.getElementById("addons").classList.contains("hidden");
            updateURL();
        }

        function getPackagePrice(pkg) {
            const packagePrice = {
                A: 3000,
                B: 2200,
                C: 2500,
                D: 2000,
                E: 2000,
                F: 2000
            };
            return packagePrice[pkg];
        }

        function clearPackage(pkg) {
            const pkgBtn = document.getElementById("pkg" + pkg);
            pkgBtn.classList.remove("selected");
            packageTotals[pkg] = 0;
            document.querySelectorAll("#items" + pkg + " .pkg-item").forEach(innerBtn => {
                innerBtn.classList.remove("selected");
            });
        }

        function togglePackage(pkg) {
            let btn = document.getElementById("pkg" + pkg);
            if (pkg === "D") clearPackage("E");
            if (pkg === "E") clearPackage("D");
            btn.classList.toggle("selected");
            packageTotals[pkg] = btn.classList.contains("selected") ? getPackagePrice(pkg) : 0;
            document.querySelectorAll("#items" + pkg + " .pkg-item").forEach(innerBtn => {
                if (btn.classList.contains("selected")) {
                    innerBtn.classList.add("selected");
                    if (pkg === "A") {
                        if (innerBtn.id === "pkgA5") innerBtn.classList.add("selected");
                        else if (innerBtn.id === "pkgA6") innerBtn.classList.remove("selected");
                    }
                } else {
                    innerBtn.classList.remove("selected");
                }
            });
            updateTotal();
            updateURL();
        }

        document.querySelectorAll(".pkg-item").forEach(btn => {
            btn.addEventListener("click", function () {
                let pkg = btn.dataset.pkg;
                const isCarotid = btn.id === 'pkgA5';
                const isEye = btn.id === 'pkgA6';
                if (!document.getElementById("pkg" + pkg).classList.contains("selected")) {
                    if (pkg === "D") clearPackage("E");
                    if (pkg === "E") clearPackage("D");
                    if (isCarotid) clearAllEyeSelections(btn.id);
                    else if (isEye) clearAllCarotidSelections(btn.id);
                    btn.classList.toggle("selected");
                    updateTotal();
                    updateURL();
                } else {
                    if (pkg === "A" && (btn.id === "pkgA5" || btn.id === "pkgA6")) {
                        handlePackageAExclusive(btn);
                        updateTotal();
                        updateURL();
                        return;
                    }
                    btn.classList.toggle("selected");
                    updateTotal();
                    updateURL();
                }
            });
        });

        document.querySelectorAll(".single-item").forEach(btn => {
            btn.addEventListener("click", function () {
                btn.classList.toggle("selected");
                updateTotal();
                updateURL();
            });
        });

        function updateTotal() {
            let sum = 0;
            let selectedPackages = [];
            for (let k in packageTotals) {
                if (packageTotals[k] > 0) selectedPackages.push(k);
            }
            let comboApplied = false;
            let comboDescription = "";
            if (selectedPackages.includes('A') && selectedPackages.includes('B') && selectedPackages.includes('C') &&
                ((selectedPackages.includes('D') || selectedPackages.includes('E')) && selectedPackages.includes('F'))) {
                sum = 10000;
                comboApplied = true;
                comboDescription = " (套餐A+B+C+D/E+F優惠組合)";
            } else if (selectedPackages.includes('A') && selectedPackages.includes('B') &&
                selectedPackages.includes('C') && selectedPackages.includes('D')) {
                sum = 9000;
                comboApplied = true;
                comboDescription = " (套餐A+B+C+D優惠組合)";
            } else if (selectedPackages.includes('A') && selectedPackages.includes('B') &&
                selectedPackages.includes('C') && selectedPackages.includes('E')) {
                sum = 9000;
                comboApplied = true;
                comboDescription = " (套餐A+B+C+E優惠組合)";
            } else if (selectedPackages.includes('A') && selectedPackages.includes('B') &&
                selectedPackages.includes('C')) {
                sum = 7500;
                comboApplied = true;
                comboDescription = " (套餐A+B+C優惠組合)";
            } else {
                for (let k in packageTotals) sum += packageTotals[k];
            }
            if (!comboApplied) {
                document.querySelectorAll(".pkg-item.selected").forEach(btn => {
                    let pkg = btn.dataset.pkg;
                    if (!document.getElementById("pkg" + pkg).classList.contains("selected")) {
                        sum += parseInt(btn.dataset.price);
                    }
                });
            }
            document.querySelectorAll(".single-item.selected").forEach(btn => {
                sum += parseInt(btn.dataset.price);
            });
            window.totalAmount = sum;
            document.getElementById("total").textContent = "總金額: " + sum + " 元" + comboDescription;
            updateGiftAvailability(sum);
        }

        function confirmPage1() {
            let pwd = prompt("請洽諮詢人員輸入驗證碼");
            if (!pwd || pwd.toLowerCase() !== "2") {
                alert("驗證失敗");
                return;
            }
            if (selected.length !== 2) {
                alert("請選2個項目");
                return;
            }
            currentPage = 2;
            document.getElementById("page1").classList.add("hidden");
            document.getElementById("page2").classList.remove("hidden");
            renderSelectedOptions();
            updateURL();
        }

        function renderSelectedOptions() {
            let selectedBox = document.getElementById("selectedOptions");
            let additionalBox = document.getElementById("additionalItems");
            selectedBox.innerHTML = "";
            additionalBox.innerHTML = "";
            const params = new URLSearchParams(window.location.search);
            const doneItems = params.get('done') ? params.get('done').split(',') : [];
            const rejectedItems = params.get('rejected') ? params.get('rejected').split(',') : [];
            const recheckItems = params.get('recheck') ? params.get('recheck').split(',') : [];

            let ultrasoundItems = [];
            let otherItems = [];
            selected.forEach(name => {
                if (name.includes('超音波')) ultrasoundItems.push(name);
                else otherItems.push(name);
            });
            const sortedSelectedItems = [...ultrasoundItems, ...otherItems];
            sortedSelectedItems.forEach(name => {
                let btn = document.createElement("button");
                btn.className = "btn";
                btn.textContent = name;
                btn.id = name;
                btn.onclick = function () {
                    markDone(btn);
                };
                if (doneItems.includes('dynamic-' + name)) {
                    btn.classList.add('done');
                    if (!btn.textContent.includes('✅')) btn.textContent += ' ✅';
                } else if (rejectedItems.includes('dynamic-' + name)) {
                    btn.classList.add('rejected');
                    if (!btn.textContent.includes('🚫')) btn.textContent += ' 🚫';
                } else if (recheckItems.includes('dynamic-' + name)) {
                    btn.classList.add('recheck');
                    if (!btn.textContent.includes('🔄')) btn.textContent += ' 🔄';
                }
                selectedBox.appendChild(btn);
            });

            let hasAdditionalItems = false;
            let hasC13 = false;
            let additionalUltrasoundItems = [];
            let additionalOtherItems = [];
            ["A", "B", "C", "D", "E", "F"].forEach(pkg => {
                document.querySelectorAll("#items" + pkg + " .pkg-item").forEach(innerBtn => {
                    if (innerBtn.classList.contains("selected")) {
                        const itemText = innerBtn.textContent;
                        if (isAllowedItem(itemText)) {
                            hasAdditionalItems = true;
                            const itemName = extractItemName(itemText);
                            if (itemName === 'C13') hasC13 = true;
                            if (itemName.includes('超音波')) {
                                additionalUltrasoundItems.push({
                                    text: itemName,
                                    pkg: pkg
                                });
                            } else {
                                additionalOtherItems.push({
                                    text: itemName,
                                    pkg: pkg
                                });
                            }
                        }
                    }
                });
            });
            document.querySelectorAll(".single-item.selected").forEach(singleBtn => {
                const itemText = singleBtn.textContent;
                if (isAllowedItem(itemText)) {
                    hasAdditionalItems = true;
                    const itemName = extractItemName(itemText);
                    if (itemName === 'C13') hasC13 = true;
                    if (itemName.includes('超音波')) {
                        additionalUltrasoundItems.push({
                            text: itemName,
                            id: "single-" + itemName
                        });
                    } else {
                        additionalOtherItems.push({
                            text: itemName,
                            id: "single-" + itemName
                        });
                    }
                }
            });
            if (selectedGift) {
                let giftBtn = document.getElementById(selectedGift);
                const giftText = giftBtn.textContent;
                if (isAllowedItem(giftText)) {
                    hasAdditionalItems = true;
                    const giftName = extractItemName(giftText);
                    if (giftName === 'C13') hasC13 = true;
                    if (giftName.includes('超音波')) {
                        additionalUltrasoundItems.push({
                            text: "🎁 " + giftName,
                            id: "gift-" + selectedGift
                        });
                    } else {
                        additionalOtherItems.push({
                            text: "🎁 " + giftName,
                            id: "gift-" + selectedGift
                        });
                    }
                }
            }
            const igeGift = document.getElementById('giftIGE');
            if (igeGift.classList.contains('auto-selected')) {
                const igeText = igeGift.textContent;
                if (isAllowedItem(igeText)) {
                    hasAdditionalItems = true;
                    const igeName = extractItemName(igeText);
                    if (igeName === 'C13') hasC13 = true;
                    additionalOtherItems.push({
                        text: "🎁 " + igeName,
                        id: "gift-giftIGE"
                    });
                }
            }
            const c13InputContainer = document.getElementById('c13InputContainer');
            if (hasC13) {
                c13InputContainer.classList.remove('hidden');
                const c13Input = document.getElementById('c13Input');
                c13Input.value = c13InputValue;
                if (c13InputLocked) c13Input.disabled = true;
            } else {
                c13InputContainer.classList.add('hidden');
            }
            const sortedAdditionalItems = [...additionalUltrasoundItems, ...additionalOtherItems];
            sortedAdditionalItems.forEach(item => {
                let btn = document.createElement("button");
                btn.className = "btn";
                btn.textContent = item.text;
                btn.id = item.id || (item.pkg + "-" + item.text);
                btn.onclick = function () {
                    markDone(btn);
                };
                const btnIdentifier = item.id || (item.pkg + "-" + item.text);
                if (doneItems.includes('dynamic-' + btnIdentifier)) {
                    btn.classList.add('done');
                    if (!btn.textContent.includes('✅')) btn.textContent += ' ✅';
                } else if (rejectedItems.includes('dynamic-' + btnIdentifier)) {
                    btn.classList.add('rejected');
                    if (!btn.textContent.includes('🚫')) btn.textContent += ' 🚫';
                } else if (recheckItems.includes('dynamic-' + btnIdentifier)) {
                    btn.classList.add('recheck');
                    if (!btn.textContent.includes('🔄')) btn.textContent += ' 🔄';
                }
                additionalBox.appendChild(btn);
            });
            const additionalBlock = document.querySelector('.additional-items');
            if (!hasAdditionalItems) {
                additionalBlock.style.display = 'none';
            } else {
                additionalBlock.style.display = 'block';
            }

            let existingSpecial = document.querySelector('.special-operations');
            if (existingSpecial) {
                existingSpecial.remove();
            }
            if (specialSelected.length > 0) {
                let specialDiv = document.createElement('div');
                specialDiv.className = 'selected-options special-operations';
                specialDiv.innerHTML = '<h3>🔹 特殊作業</h3>';
                let specialBtnsDiv = document.createElement('div');
                specialSelected.forEach(code => {
                    let generated = [];
                    if (code === '03') {
                        generated = ['肺功能'];
                    } else if (code === '05') {
                        generated = ['血中鉛'];
                    } else if (code === '12') {
                        generated = [];
                    }
                    generated.forEach(name => {
                        let btn = document.createElement('button');
                        btn.className = 'btn';
                        btn.textContent = name;
                        btn.id = `special-${code}-${name}`;
                        btn.onclick = () => markDone(btn);
                        const identifier = `special-${code}-${name}`;
                        if (doneItems.includes('dynamic-' + identifier)) {
                            btn.classList.add('done');
                            if (!btn.textContent.includes('✅')) btn.textContent += ' ✅';
                        } else if (rejectedItems.includes('dynamic-' + identifier)) {
                            btn.classList.add('rejected');
                            if (!btn.textContent.includes('🚫')) btn.textContent += ' 🚫';
                        } else if (recheckItems.includes('dynamic-' + identifier)) {
                            btn.classList.add('recheck');
                            if (!btn.textContent.includes('🔄')) btn.textContent += ' 🔄';
                        }
                        specialBtnsDiv.appendChild(btn);
                    });
                });
                specialDiv.appendChild(specialBtnsDiv);
                const selectedOptionsDiv = document.querySelector('.selected-options');
                selectedOptionsDiv.parentNode.insertBefore(specialDiv, selectedOptionsDiv);
            }
        }

        function goBack() {
            let pw = prompt("請洽諮詢人員輸入驗證碼");
            if (pw && pw.toLowerCase() === "1") {
                currentPage = 1;
                document.getElementById("page2").classList.add("hidden");
                document.getElementById("page1").classList.remove("hidden");
                updateURL();
            }
        }

        async function uploadCurrentURL(buttonId, status) {
            if (!serialNumber) {
                console.warn('流水號未輸入，無法上傳URL');
                alert('請先輸入流水號！');
                return false;
            }
            const data = {
                full_url: window.location.href,
                barcode: barcodeValue,
                serial: serialNumber,
                button_id: buttonId,
                status: status,
                total_amount: window.totalAmount || 0,
                timestamp: new Date().toISOString()
            };
            console.log('上傳資料:', JSON.stringify(data, null, 2));
            try {
                const response = await fetch('https://your-server.com/upload', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                        'Accept': 'application/json'
                    },
                    body: JSON.stringify(data)
                });
                if (response.ok) {
                    console.log(`按鈕 ${buttonId} 狀態 (${status}) 上傳成功`);
                    return true;
                } else {
                    const errorText = await response.text();
                    console.error(`URL上傳失敗: ${response.status} - ${errorText}`);
                    alert(`上傳失敗: ${response.status} - ${errorText}`);
                    return false;
                }
            } catch (error) {
                console.error('URL上傳錯誤:', error);
                alert('上傳錯誤，請檢查網路連線或後端服務');
                return false;
            }
        }

        async function markDone(button) {
            let input = prompt("請由操作人員輸入");
            if (!input) return;
            input = input.toLowerCase();
            const isDynamic = button.closest('#selectedOptions') || button.closest('#additionalItems') || button.closest(
                '.special-operations');
            const buttonId = isDynamic ? 'dynamic-' + button.id : button.id;
            let status = '';

            if (input === "v") {
                button.classList.remove("rejected", "recheck");
                button.classList.add("done");
                button.textContent = button.textContent.replace(" 🚫", "").replace(" ✅", "").replace(" 🔄", "") + " ✅";
                status = 'done';
            } else if (input === "x") {
                button.classList.remove("done", "rejected", "recheck");
                button.textContent = button.textContent.replace(" ✅", "").replace(" 🚫", "").replace(" 🔄", "");
                status = 'cleared';
            } else if (input === "g" && (button.closest('.basic-checkup') || button.closest('#selectedOptions'))) {
                button.classList.remove("done", "recheck");
                button.classList.add("rejected");
                button.textContent = button.textContent.replace(" ✅", "").replace(" 🚫", "").replace(" 🔄", "") + " 🚫";
                status = 'rejected';
            } else if (input === "2") {
                button.classList.remove("done", "rejected");
                button.classList.add("recheck");
                button.textContent = button.textContent.replace(" ✅", "").replace(" 🚫", "").replace(" 🔄", "") + " 🔄";
                status = 'recheck';
            } else {
                alert("輸入錯誤，請重新操作！");
                return;
            }

            updateURL();
            if (currentPage === 2) {
                const success = await uploadCurrentURL(buttonId, status);
                if (!success) {
                    console.warn(`按鈕 ${buttonId} 狀態 (${status}) 上傳失敗`);
                }
            }
        }

        function uploadData() {
            const pwd = prompt('請洽諮詢人員輸入驗證碼');
            if (!pwd || pwd.toUpperCase() !== 'F') {
                alert('驗證碼錯誤，請輸入正確的驗證碼！');
                return;
            }
            if (!serialNumber) {
                alert('請先輸入流水號！');
                return;
            }
            const data = {
                full_url: window.location.href,
                barcode: barcodeValue,
                serial: serialNumber,
                total_amount: window.totalAmount || 0 // 新增：發送總金額，如果未計算則預設0
            };

            fetch('https://your-server.com/upload', { // 替換成您的實際後端URL
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(data)
            }).then(response => {
                if (response.ok) {
                    alert('資料上傳成功！');
                } else {
                    alert('上傳失敗');
                }
            }).catch(error => {
                console.error('Error:', error);
                alert('上傳錯誤');
            });
        }

        window.onload = function () {
            initSignature();
            loadFromURL();
            updateStatusIndicator();
            updateTotal();
        }
    </script>
</body>

