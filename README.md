<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-M-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Financial Calculator Suite</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f2f5;
        }
        /* Styles for main category tabs */
        .main-tab-button {
            padding: 12px 20px;
            font-weight: 700;
            border-bottom: 4px solid transparent;
            transition: all 0.2s ease-in-out;
            cursor: pointer;
            border-radius: 8px 8px 0 0;
            color: #4b5563; /* Gray-700 */
            font-size: 1.1rem;
            flex-grow: 1; /* Allow buttons to grow and fill space */
            text-align: center;
            white-space: nowrap; /* Prevent wrapping */
        }
        .main-tab-button.active {
            border-color: #3b82f6; /* Blue-500 */
            color: #1f2937; /* Gray-900 */
            background-color: #e5e7eb; /* Gray-200 */
        }
        .main-tab-button:hover:not(.active) {
            background-color: #e5e7eb; /* Gray-100 */
        }
        /* Styles for loan sub-category tabs */
        .loan-tab-button {
            padding: 8px 12px;
            font-weight: 600;
            border-bottom: 2px solid transparent;
            transition: all 0.2s ease-in-out;
            cursor: pointer;
            border-radius: 6px 6px 0 0;
            color: #6b7280; /* Gray-500 */
            font-size: 0.9rem;
        }
        .loan-tab-button.active {
            border-color: #2563eb; /* Blue-600 (darker for sub-tab) */
            color: #1f2937; /* Gray-900 */
            background-color: #eff6ff; /* Blue-50 */
        }
        .loan-tab-button:hover:not(.active) {
            background-color: #f3f4f6; /* Gray-50 */
        }
        /* Custom styles for cost breakdown circles */
        .cost-circle {
            width: 100px;
            height: 100px;
            border-radius: 50%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: 600;
            text-align: center;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            margin: 10px;
            padding: 5px; /* Added padding to ensure text fits */
            font-size: 0.75rem; /* Adjusted font size for better fit */
        }
        .cost-circle .amount {
            font-size: 0.95rem; /* Larger font for amount */
            font-weight: 700;
        }
        .cost-circle .percentage {
            font-size: 0.7rem; /* Smaller font for percentage */
            font-weight: 500;
            opacity: 0.9;
        }
        .cost-circle-principal { background-color: #3b82f6; } /* Blue */
        .cost-circle-interest { background-color: #ef4444; } /* Red */
        .cost-circle-processing { background-color: #f59e0b; } /* Amber */
        .cost-circle-gst-interest { background-color: #6d28d9; } /* Violet */
        .cost-circle-maturity { background-color: #10b981; } /* Green-500 */
        .cost-circle-total-deposit { background-color: #0d9488; } /* Teal-600 */
        .cost-circle-interest-earned { background-color: #f97316; } /* Orange-500 */
        .cost-circle-subsidized { background-color: #8b5cf6; } /* Violet-500 */
        .cost-circle-card-discount { background-color: #d946ef; } /* Fuchsia-500 */
        /* Telegram Link Styles and Animation */
        .telegram-link {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
            padding: 10px 24px;
            background-color: #0088cc; /* Telegram brand color */
            border-radius: 9999px;
            font-weight: 600;
            text-decoration: none;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
            width: 90%; /* Increased width */
            max-width: 600px; /* Max width for larger screens */
            margin-left: auto;
            margin-right: auto;
            animation: pulse 2s infinite cubic-bezier(0.4, 0, 0.6, 1);
        }
        .telegram-link:hover {
            transform: translateY(-2px) scale(1.02);
            box-shadow: 0 6px 10px rgba(0, 0, 0, 0.15);
            animation: none;
        }
        .telegram-link::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            background: rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            transform: translate(-50%, -50%);
            transition: width 0.4s ease, height 0.4s ease, opacity 0.4s ease;
            opacity: 0;
            z-index: 0;
        }
        .telegram-link:hover::before {
            width: 120%;
            height: 120%;
            opacity: 1;
        }
        .telegram-link span, .telegram-link svg {
            position: relative;
            z-index: 1;
        }
        @keyframes pulse {
            0%, 100% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.01);
                box-shadow: 0 0 15px rgba(0, 136, 204, 0.4);
            }
        }
        /* Dynamic Text Color - using CSS variables for transition */
        .telegram-link-text {
            transition: color 0.5s ease-in-out; /* Smooth color transition */
        }
        /* Modal styles */
        .modal {
            display: none; /* Hidden by default */
            position: fixed; /* Stay in place */
            z-index: 1000; /* Sit on top */
            left: 0;
            top: 0;
            width: 100%; /* Full width */
            height: 100%; /* Full height */
            overflow: auto; /* Enable scroll if needed */
            background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .modal-content {
            background-color: #fefefe;
            margin: auto;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
            position: relative;
            max-width: 500px;
            width: 90%;
            text-align: center;
            transform: translateY(-50px); /* Initial position for animation */
            opacity: 0; /* Initial opacity for animation */
            animation: modalAppear 0.3s ease-out forwards;
        }

        .modal.hidden .modal-content {
            animation: modalDisappear 0.3s ease-in forwards;
        }

        .close-button {
            color: #aaa;
            position: absolute;
            top: 10px;
            right: 15px;
            font-size: 28px;
            font-weight: bold;
            background: none;
            border: none;
            cursor: pointer;
        }

        .close-button:hover,
        .close-button:focus {
            color: black;
            text-decoration: none;
            cursor: pointer;
        }

        .loader {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #3b82f6;
            border-radius: 50%;
            width: 30px;
            height: 30px;
            animation: spin 1s linear infinite;
            margin: 0 auto 15px auto;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        @keyframes modalAppear {
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }

        @keyframes modalDisappear {
            from {
                transform: translateY(0);
                opacity: 1;
            }
            to {
                transform: translateY(-50px);
                opacity: 0;
            }
        }

        /* Styles for table headings matching colors */
        .table-header-principal { color: #3b82f6; }
        .table-header-interest { color: #ef4444; }
        .table-header-gst-interest { color: #6d28d9; }
        .table-header-total-emi { color: #1e40af; }
        .table-header-deposit { color: #0d9488; }
        .table-header-balance { color: #10b981; }

        /* Style for remaining balance in EMI schedule */
        .text-red-bold {
            color: #ef4444; /* red-600 */
            font-weight: bold;
        }

        .calculator-section-group {
            border: 1px solid #e0e0e0;
            border-radius: 0.5rem;
            padding: 1.5rem;
            margin-top: 1rem;
            background-color: #fdfdfd;
        }
        
        /* Styles for the hidden downloadable content */
        #downloadable-content-wrapper {
            position: absolute;
            left: -9999px; /* Move it off-screen */
            top: auto;
            width: 800px; /* A fixed width for consistent PDF/JPG output */
            background: white;
            padding: 20px;
        }


    </style>
</head>
<body class="p-4 sm:p-6 md:p-8 flex flex-col items-center justify-center min-h-screen">

    <a href="https://t.me/Earncashback_com" target="_blank" rel="noopener noreferrer" class="telegram-link mb-6">
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-send">
            <line x1="22" y1="2" x2="11" y2="13"></line>
            <polygon points="22 2 15 22 11 13 2 9 22 2"></polygon>
        </svg>
        <span class="font-extrabold telegram-link-text" style="color: red;">Join Now for Latest Offers & Earnings 💵</span>
    </a>

    <div id="calculator-main-card" class="bg-white p-6 sm:p-8 md:p-10 rounded-2xl shadow-xl w-full max-w-2xl border border-gray-200">
        <h1 class="text-3xl font-bold text-center text-gray-800 mb-6">Financial Calculator Suite</h1>

        <div class="flex flex-wrap justify-center gap-1 sm:gap-2 mb-8" id="mainCategoryTabs">
            <button class="main-tab-button active" data-main-type="loan">Loan Calculator</button>
            <button class="main-tab-button" data-main-type="fd">FD Calculator</button>
            <button class="main-tab-button" data-main-type="rd">RD Calculator</button>
        </div>

        <div id="calculatorSectionsContainer">
            <div id="loanCalculatorSection" class="calculator-section-group">
                <div class="flex flex-wrap justify-center gap-2 sm:gap-4 mb-8" id="loanSubCategoryTabs">
                    <button class="loan-tab-button active" data-loan-type="reducing_interest">Reducing Interest Rate</button>
                    <button class="loan-tab-button" data-loan-type="no_cost_emi">No-Cost EMI</button>
                    <button class="loan-tab-button" data-loan-type="flat_interest">Flat Interest Rate</button>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
                    <div>
                        <label for="loanAmount" id="loanAmountLabel" class="block text-gray-700 text-sm font-medium mb-2">Loan Amount (₹)</label>
                        <input type="number" id="loanAmount" value="20000" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 shadow-sm" step="0.01">
                        <p id="loanAmountError" class="text-red-500 text-xs mt-1 hidden">Please enter a valid amount.</p>
                    </div>
                    <div id="cardDiscountDiv" class="hidden">
                        <label for="cardDiscount" class="block text-gray-700 text-sm font-medium mb-2">Card Discount (₹)</label>
                        <input type="number" id="cardDiscount" value="0" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 shadow-sm" step="0.01">
                        <p id="cardDiscountError" class="text-red-500 text-xs mt-1 hidden">Please enter a valid discount.</p>
                    </div>
                    <div id="annualInterestRateDiv">
                        <label for="annualInterestRate" class="block text-gray-700 text-sm font-medium mb-2">Annual Interest Rate (%)</label>
                        <input type="number" id="annualInterestRate" value="16" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 shadow-sm" step="0.01">
                        <p id="annualInterestRateError" class="text-red-500 text-xs mt-1 hidden">Please enter a valid interest rate.</p>
                    </div>
                    <div>
                        <label for="tenure" class="block text-gray-700 text-sm font-medium mb-2">Tenure (Months)</label>
                        <select id="tenure" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 shadow-sm">
                            <option value="3">3 Months</option>
                            <option value="6" selected>6 Months</option>
                            <option value="9">9 Months</option>
                            <option value="12">12 Months</option>
                            <option value="18">18 Months</option>
                            <option value="24">24 Months</option>
                            <option value="36">36 Months</option>
                            <option value="custom">Custom</option>
                        </select>
                        <p id="tenureError" class="text-red-500 text-xs mt-1 hidden">Please select a tenure.</p>
                    </div>
                    <div>
                        <label for="startDate" class="block text-gray-700 text-sm font-medium mb-2">Starting Date (Optional)</label>
                        <input type="date" id="startDate" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 shadow-sm">
                        <p id="startDateError" class="text-red-500 text-xs mt-1 hidden">Please select a starting date.</p>
                    </div>
                    <div id="customTenureInputDiv" class="mt-4 hidden">
                        <label for="customTenure" class="block text-gray-700 text-sm font-medium mb-2">Custom Tenure (Months)</label>
                        <input type="number" id="customTenure" value="1" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 shadow-sm" min="1">
                        <p id="customTenureError" class="text-red-500 text-xs mt-1 hidden">Please enter a valid tenure (minimum 1 month).</p>
                    </div>
                </div>

                <div id="processingFeeSection" class="mb-6 border border-gray-200 p-4 rounded-lg bg-gray-50">
                    <label for="processingFeeType" class="block text-gray-700 text-sm font-medium mb-2">Select Processing Fee</label>
                    <select id="processingFeeType" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 shadow-sm">
                        <option value="standard-16percent">1% Processing + 18% GST</option>
                        <option value="standard-18flat" selected>₹199 Flat + 18% GST</option>
                        <option value="custom-percentage">Custom Percentage Fee (%) + 18% GST</option>
                        <option value="custom-flat">Custom Flat Fee (₹) + 18% GST</option>
                    </select>
                    <p id="processingFeeTypeError" class="text-red-500 text-xs mt-1 hidden">Please select a processing fee type.</p>

                    <div id="customPercentageInputDiv" class="mt-4 hidden">
                        <label for="customPercentageFee" class="block text-gray-700 text-sm font-medium mb-2">Percentage Fee (e.g., 0.5 for 0.5%)</label>
                        <input type="number" id="customPercentageFee" value="0" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 shadow-sm" step="0.01">
                        <p id="customPercentageFeeError" class="text-red-500 text-xs mt-1 hidden">Please enter a valid percentage fee.</p>
                    </div>

                    <div id="customFlatInputDiv" class="mt-4 hidden">
                        <label for="customFlatFee" class="block text-gray-700 text-sm font-medium mb-2">Flat Fee (₹)</label>
                        <input type="number" id="customFlatFee" value="199" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 shadow-sm" step="0.01">
                        <p id="customFlatFeeError" class="text-red-500 text-xs mt-1 hidden">Please enter a valid flat fee.</p>
                    </div>
                </div>
            </div> <div id="fdCalculatorSection" class="calculator-section-group hidden">
                <h2 class="text-2xl font-bold text-center text-gray-800 mb-6">Fixed Deposit (FD) Calculator</h2>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
                    <div>
                        <label for="fdPrincipal" class="block text-gray-700 text-sm font-medium mb-2">Deposit Amount (₹)</label>
                        <input type="number" id="fdPrincipal" value="100000" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 shadow-sm" step="0.01">
                        <p id="fdPrincipalError" class="text-red-500 text-xs mt-1 hidden">Please enter a valid deposit amount.</p>
                    </div>
                    <div>
                        <label for="fdAnnualInterestRate" class="block text-gray-700 text-sm font-medium mb-2">Annual Interest Rate (%)</label>
                        <input type="number" id="fdAnnualInterestRate" value="7" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 shadow-sm" step="0.01">
                        <p id="fdAnnualInterestRateError" class="text-red-500 text-xs mt-1 hidden">Please enter a valid interest rate.</p>
                    </div>
                    <div>
                        <label for="fdTenureYears" class="block text-gray-700 text-sm font-medium mb-2">Tenure (Years)</label>
                        <input type="number" id="fdTenureYears" value="0.5" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 shadow-sm" min="0" step="0.5">
                        <p id="fdTenureYearsError" class="text-red-500 text-xs mt-1 hidden">Please enter a valid number of years.</p>
                    </div>
                    <div class="hidden"> <label for="fdCompoundingFrequency" class="block text-gray-700 text-sm font-medium mb-2">Compounding Frequency</label>
                        <select id="fdCompoundingFrequency" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 shadow-sm">
                            <option value="4">Quarterly</option>
                        </select>
                    </div>
                     <div class="md:col-span-2">
                        <label for="fdStartDate" class="block text-gray-700 text-sm font-medium mb-2">Starting Date (Optional)</label>
                        <input type="date" id="fdStartDate" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 shadow-sm">
                    </div>
                </div>
            </div> <div id=
