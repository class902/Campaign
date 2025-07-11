<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EarnWala - Get Cashback & Rewards</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 10px;
        }
        
        .container {
            max-width: 400px;
            margin: 0 auto;
            background: #f5f5f5;
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }
        
        .header {
            background: linear-gradient(135deg, #4f46e5, #7c3aed);
            padding: 15px;
            text-align: center;
            color: white;
        }
        
        .header h1 {
            font-size: 24px;
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        .header p {
            font-size: 14px;
            opacity: 0.9;
        }
        
        .upi-section {
            background: linear-gradient(135deg, #10b981, #059669);
            margin: 15px;
            border-radius: 12px;
            padding: 15px;
            color: white;
            box-shadow: 0 4px 15px rgba(16, 185, 129, 0.3);
        }
        
        .upi-title {
            font-size: 16px;
            font-weight: bold;
            margin-bottom: 10px;
            text-align: center;
        }
        
        .upi-form {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        
        .upi-input {
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-size: 14px;
            outline: none;
        }
        
        .upi-submit {
            background: #ffffff;
            color: #10b981;
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-weight: bold;
            font-size: 14px;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .upi-submit:hover {
            background: #f0fdf4;
            transform: translateY(-2px);
        }
        
        .cashback-display {
            text-align: center;
            font-size: 18px;
            font-weight: bold;
            margin: 10px 0;
            padding: 10px;
            border-radius: 8px;
            background: rgba(255,255,255,0.2);
            animation: colorChange 2s infinite;
        }
        
        @keyframes colorChange {
            0% { color: #ffffff; }
            25% { color: #fbbf24; }
            50% { color: #34d399; }
            75% { color: #f87171; }
            100% { color: #ffffff; }
        }
        
        .telegram-header {
            background: linear-gradient(135deg, #0088cc, #0077b3);
            margin: 15px;
            border-radius: 12px;
            padding: 15px;
            display: flex;
            align-items: center;
            color: white;
            box-shadow: 0 4px 15px rgba(0, 136, 204, 0.3);
        }
        
        .telegram-icon {
            width: 50px;
            height: 50px;
            margin-right: 15px;
            background: rgba(255,255,255,0.2);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .telegram-icon img {
            width: 30px;
            height: 30px;
        }
        
        .telegram-text {
            flex: 1;
        }
        
        .telegram-text div:first-child {
            font-size: 16px;
            font-weight: bold;
            margin-bottom: 3px;
        }
        
        .telegram-subtitle {
            font-size: 12px;
            opacity: 0.9;
        }
        
        .telegram-btn {
            background: #ffffff;
            color: #0088cc;
            padding: 10px 20px;
            border: none;
            border-radius: 20px;
            font-weight: bold;
            font-size: 14px;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .telegram-btn:hover {
            background: #f0f9ff;
            transform: translateY(-2px);
        }
        
        .offers-section {
            padding: 0 15px 15px;
        }
        
        .section-title {
            text-align: center;
            font-size: 20px;
            font-weight: bold;
            color: #4f46e5;
            margin-bottom: 20px;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        
        .offer-card {
            background: white;
            border-radius: 12px;
            margin-bottom: 12px;
            padding: 15px;
            display: flex;
            align-items: center;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            border-left: 4px solid #4f46e5;
            transition: all 0.3s ease;
            cursor: pointer;
        }
        
        .offer-card:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 20px rgba(0,0,0,0.15);
        }
        
        .offer-icon {
            width: 50px;
            height: 50px;
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 15px;
            font-size: 24px;
            font-weight: bold;
            color: white;
            flex-shrink: 0;
        }
        
        .offer-content {
            flex: 1;
        }
        
        .offer-title {
            font-size: 16px;
            font-weight: bold;
            color: #1f2937;
            margin-bottom: 4px;
        }
        
        .offer-desc {
            font-size: 12px;
            color: #6b7280;
            line-height: 1.4;
        }
        
        .earn-btn {
            background: linear-gradient(135deg, #7c3aed, #4f46e5);
            color: white;
            padding: 12px 18px;
            border: none;
            border-radius: 20px;
            font-weight: bold;
            font-size: 14px;
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: center;
            min-width: 80px;
            animation: cashbackColor 2s infinite;
        }
        
        @keyframes cashbackColor {
            0% { background: linear-gradient(135deg, #7c3aed, #4f46e5); }
            25% { background: linear-gradient(135deg, #dc2626, #b91c1c); }
            50% { background: linear-gradient(135deg, #059669, #047857); }
            75% { background: linear-gradient(135deg, #f97316, #ea580c); }
            100% { background: linear-gradient(135deg, #7c3aed, #4f46e5); }
        }
        
        .earn-btn:hover {
            transform: scale(1.05);
            box-shadow: 0 5px 15px rgba(79, 70, 229, 0.4);
        }
        
        .footer {
            background: #1f2937;
            padding: 15px;
            text-align: center;
            color: white;
            margin-top: 20px;
        }
        
        .footer-icon {
            background: #0ea5e9;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 10px;
            font-size: 20px;
        }
        
        .admin-btn {
            position: fixed;
            bottom: 20px;
            left: 20px;
            background: #dc2626;
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 50px;
            font-weight: bold;
            font-size: 12px;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(220, 38, 38, 0.3);
            z-index: 1000;
        }
        
        .admin-btn:hover {
            background: #b91c1c;
        }
        
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 2000;
        }
        
        .modal-content {
            background: white;
            margin: 5% auto;
            padding: 20px;
            border-radius: 10px;
            width: 90%;
            max-width: 500px;
            max-height: 80vh;
            overflow-y: auto;
        }
        
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 1px solid #e5e7eb;
        }
        
        .close {
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
            color: #6b7280;
        }
        
        .close:hover {
            color: #374151;
        }
        
        .form-group {
            margin-bottom: 15px;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #374151;
        }
        
        .form-group input,
        .form-group textarea,
        .form-group select {
            width: 100%;
            padding: 10px;
            border: 1px solid #d1d5db;
            border-radius: 6px;
            font-size: 14px;
        }
        
        .form-group textarea {
            resize: vertical;
            min-height: 80px;
        }
        
        .btn {
            background: #4f46e5;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 6px;
            font-weight: bold;
            cursor: pointer;
            margin-right: 10px;
            margin-bottom: 10px;
        }
        
        .btn:hover {
            background: #4338ca;
        }
        
        .btn-secondary {
            background: #6b7280;
        }
        
        .btn-secondary:hover {
            background: #4b5563;
        }
        
        .btn-danger {
            background: #dc2626;
        }
        
        .btn-danger:hover {
            background: #b91c1c;
        }
        
        .kotak { background: linear-gradient(135deg, #dc2626, #b91c1c); }
        .airtel { background: linear-gradient(135deg, #dc2626, #991b1b); }
        .groww { background: linear-gradient(135deg, #059669, #047857); }
        .digibank { background: linear-gradient(135deg, #1f2937, #374151); }
        .jupiter { background: linear-gradient(135deg, #f97316, #ea580c); }
        .smallcase { background: linear-gradient(135deg, #3b82f6, #2563eb); }
        .upstox { background: linear-gradient(135deg, #7c3aed, #6d28d9); }
        .navi { background: linear-gradient(135deg, #10b981, #059669); }
        .axis { background: linear-gradient(135deg, #be185d, #a21caf); }
        .paytm { background: linear-gradient(135deg, #0ea5e9, #0284c7); }
        .indusind { background: linear-gradient(135deg, #dc2626, #b91c1c); }
        
        .pulse {
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        
        .offer-details {
            background: white;
            padding: 20px;
            border-radius: 10px;
            margin: 20px;
        }
        
        .steps-list {
            margin: 15px 0;
        }
        
        .steps-list li {
            margin: 8px 0;
            padding: 8px;
            background: #f3f4f6;
            border-radius: 5px;
            border-left: 3px solid #4f46e5;
        }
        
        .hidden {
            display: none;
        }
        
        .download-btn {
            position: fixed;
            top: 20px;
            right: 20px;
            background: #10b981;
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 50px;
            font-weight: bold;
            font-size: 12px;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(16, 185, 129, 0.3);
            z-index: 1000;
        }
        
        .download-btn:hover {
            background: #059669;
        }
        
        .file-upload {
            position: relative;
            display: inline-block;
            cursor: pointer;
        }
        
        .file-upload input[type=file] {
            position: absolute;
            left: -9999px;
        }
        
        .file-upload-btn {
            background: #6b7280;
            color: white;
            padding: 8px 15px;
            border: none;
            border-radius: 5px;
            font-size: 12px;
            cursor: pointer;
        }
        
        .file-upload-btn:hover {
            background: #4b5563;
        }
    </style>
</head>
<body>
    <div class="container" id="mainContainer">
        <div class="header">
            <h1>💰 EARNING HUB</h1>
            <p>Your Ultimate Cashback Destination</p>
        </div>
        
        <div class="upi-section">
            <div class="upi-title">💳 Enter Your UPI ID</div>
            <div class="cashback-display">Total Cashback: ₹2,840</div>
            <div class="upi-form">
                <input type="text" class="upi-input" placeholder="Enter your UPI ID (e.g., yourname@paytm)" id="upiInput">
                <button class="upi-submit" onclick="submitUPI()">Submit UPI ID</button>
            </div>
        </div>
        
        <div class="telegram-header">
            <div class="telegram-icon">
                <img src="https://upload.wikimedia.org/wikipedia/commons/8/82/Telegram_logo.svg" alt="Telegram">
            </div>
            <div class="telegram-text">
                <div>Join Our Telegram Channel</div>
                <div class="telegram-subtitle">Get Exclusive Offers & Updates</div>
            </div>
            <button class="telegram-btn" onclick="window.open('https://t.me/Earncashback_com', '_blank')">Join</button>
        </div>
        
        <div class="offers-section">
            <div class="section-title">💎 Premium Offers</div>
            
            <div class="offer-card pulse" onclick="showOfferDetails('Kotak 811', '₹200', 'kotak')">
                <div class="offer-icon kotak">K</div>
                <div class="offer-content">
                    <div class="offer-title">Kotak 811</div>
                    <div class="offer-desc">Open Bank Account and Get Rs.200 Within 24hrs...</div>
                </div>
                <button class="earn-btn">Earn<br>₹200</button>
            </div>
            
            <div class="offer-card" onclick="showOfferDetails('Airtel Payment Bank', '₹100', 'airtel')">
                <div class="offer-icon airtel">A</div>
                <div class="offer-content">
                    <div class="offer-title">Airtel Payment Bank</div>
                    <div class="offer-desc">Open 0 balance Account and Get Rs.100 UPI Cas...</div>
                </div>
                <button class="earn-btn">Earn<br>₹100</button>
            </div>
            
            <div class="offer-card" onclick="showOfferDetails('Groww', '₹100', 'groww')">
                <div class="offer-icon groww">G</div>
                <div class="offer-content">
                    <div class="offer-title">Groww</div>
                    <div class="offer-desc">Open Trading Account & Get Rs.100 UPI...</div>
                </div>
                <button class="earn-btn">Earn<br>₹100</button>
            </div>
            
            <div class="offer-card" onclick="showOfferDetails('Digibank', '₹120', 'digibank')">
                <div class="offer-icon digibank">D</div>
                <div class="offer-content">
                    <div class="offer-title">Digibank</div>
                    <div class="offer-desc">Install & Open Account and get Rs.120 UPI Cas...</div>
                </div>
                <button class="earn-btn">Earn<br>₹120</button>
            </div>
            
            <div class="offer-card" onclick="showOfferDetails('Jupiter', '₹70', 'jupiter')">
                <div class="offer-icon jupiter">J</div>
                <div class="offer-content">
                    <div class="offer-title">Jupiter</div>
                    <div class="offer-desc">Open Bank Account & Get Rs.70 UPI Cashback in...</div>
                </div>
                <button class="earn-btn">Earn<br>₹70</button>
            </div>
            
            <div class="offer-card" onclick="showOfferDetails('Smallcase', '₹550', 'smallcase')">
                <div class="offer-icon smallcase">S</div>
                <div class="offer-content">
                    <div class="offer-title">Smallcase</div>
                    <div class="offer-desc">Deposit Rs.300 & Get Rs.550 UPI Cashback With...</div>
                </div>
                <button class="earn-btn">Earn<br>₹550</button>
            </div>
            
            <div class="offer-card" onclick="showOfferDetails('Upstox', '₹200', 'upstox')">
                <div class="offer-icon upstox">U</div>
                <div class="offer-content">
                    <div class="offer-title">Upstox</div>
                    <div class="offer-desc">Open Demat Account and get Rs.200 UPI Cashbac...</div>
                </div>
                <button class="earn-btn">Earn<br>₹200</button>
            </div>
            
            <div class="offer-card" onclick="showOfferDetails('Navi UPI', '₹10', 'navi')">
                <div class="offer-icon navi">N</div>
                <div class="offer-content">
                    <div class="offer-title">Navi UPI</div>
                    <div class="offer-desc">Do First UPI Transfer & Get Rs.10 Cashback in...</div>
                </div>
                <button class="earn-btn">Earn<br>₹10</button>
            </div>
            
            <div class="offer-card" onclick="showOfferDetails('AxisBank', '₹1000', 'axis')">
                <div class="offer-icon axis">A</div>
                <div class="offer-content">
                    <div class="offer-title">AxisBank</div>
                    <div class="offer-desc">Install & Open Bank Account and Get Rs.1000 U...</div>
                </div>
                <button class="earn-btn">Earn<br>₹1000</button>
            </div>
            
            <div class="offer-card" onclick="showOfferDetails('IndusInd Savings', '₹400', 'indusind')">
                <div class="offer-icon indusind">I</div>
                <div class="offer-content">
                    <div class="offer-title">IndusInd Savings</div>
                    <div class="offer-desc">Install & Open Bank Account and Get Rs.400 UP...</div>
                </div>
                <button class="earn-btn">Earn<br>₹400</button>
            </div>
        </div>
        
        <div class="footer">
            <div class="footer-icon">📞</div>
            <div style="font-size: 14px; margin-bottom: 5px;">Need Help? Contact Support</div>
            <div style="font-size: 12px; opacity: 0.8;">support@earninghub.com</div>
        </div>
    </div>
    
    <!-- Admin Button -->
    <button class="admin-btn" onclick="showAdminLogin()">Admin</button>
    
    <!-- Download Button (Hidden by default) -->
    <button class="download-btn hidden" id="downloadBtn" onclick="downloadLeads()">Download Leads</button>
    
    <!-- Admin Login Modal -->
    <div id="adminLoginModal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2>Admin Login</h2>
             
