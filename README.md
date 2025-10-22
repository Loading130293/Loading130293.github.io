<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Marriott Points Calculator</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #f4f4f4;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
        }
        .container {
            background-color: #ffffff;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
            width: 100%;
            max-width: 600px;
            overflow: hidden;
        }
        .header {
            background-color: #00487C; /* Marriott-like blue */
            color: white;
            padding: 20px 30px;
        }
        .header h1 {
            margin: 0;
            font-size: 24px;
        }
        .calculator-body {
            padding: 30px;
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }
        .form-group {
            display: flex;
            flex-direction: column;
        }
        .form-group label {
            font-weight: 600;
            margin-bottom: 6px;
            font-size: 14px;
            color: #333;
        }
        .form-group input {
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            font-size: 16px;
        }
        .full-width {
            grid-column: 1 / -1;
        }
        .button-container {
            grid-column: 1 / -1;
            margin-top: 10px;
        }
        .button-container button {
            width: 100%;
            padding: 12px;
            font-size: 16px;
            font-weight: 700;
            color: white;
            background-color: #d32f2f; /* Marriott-like red */
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.2s;
        }
        .button-container button:hover {
            background-color: #b71c1c;
        }
        .results {
            background-color: #f9f9f9;
            padding: 30px;
            border-top: 1px solid #eee;
        }
        .results h2 {
            margin-top: 0;
            border-bottom: 2px solid #eee;
            padding-bottom: 10px;
        }
        .result-item {
            display: flex;
            justify-content: space-between;
            font-size: 16px;
            padding: 12px 0;
            border-bottom: 1px solid #eee;
        }
        .result-item strong {
            font-weight: 600;
            color: #000;
        }
        .result-item span {
            font-weight: 500;
            color: #d32f2f;
        }

        /* Responsive design for smaller screens */
        @media (max-width: 600px) {
            .calculator-body {
                grid-template-columns: 1fr;
            }
            .full-width {
                grid-column: 1 / -1; /* This is redundant but ensures consistency */
            }
        }
    </style>
</head>
<body>

    <div class="container">
        <div class="header">
            <h1>Marriott Points Earning Calculator</h1>
        </div>

        <div class="calculator-body">
            <div class="form-group">
                <label for="cashPrice">Base Cash Price</label>
                <input type="number" id="cashPrice" value="180">
            </div>
            <div class="form-group">
                <label for="taxFee">Tax & Fees</label>
                <input type="number" id="taxFee" value="33">
            </div>

            <div class="form-group">
                <label for="brandMultiplier">Brand Multiplier (e.g., 10)</label>
                <input type="number" id="brandMultiplier" value="10">
            </div>
            <div class="form-group">
                <label for="statusMultiplier">Status Bonus (%)</label>
                <input type="number" id="statusMultiplier" value="75">
            </div>
            <div class="form-group">
                <label for="ccMultiplier">Credit Card Multiplier (e.g., 0)</label>
                <input type="number" id="ccMultiplier" value="0">
            </div>
            <div class="form-group">
                <label for="pointValue">Your Point Value (e.g., 0.007)</label>
                <input type="number" id="pointValue" step="0.001" value="0.007">
            </div>
            
            <div class="form-group full-width">
                <label for="welcomePoints">Welcome Gift Points</label>
                <input type="number" id="welcomePoints" value="500">
            </div>
            <div class="form-group full-width">
                <label for="promoPoints">Other Promotion Points</label>
                <input type="number" id="promoPoints" value="3500">
            </div>

            <div class="button-container">
                <button onclick="calculatePoints()">Calculate</button>
            </div>
        </div>

        <div class="results">
            <h2>Calculation Results</h2>
            <div class="result-item">
                <strong>Base Points:</strong>
                <span id="resBasePoints">0</span>
            </div>
            <div class="result-item">
                <strong>Status Bonus:</strong>
                <span id="resStatusBonus">0</span>
            </div>
            <div class="result-item">
                <strong>CC Bonus:</strong>
                <span id="resCcBonus">0</span>
            </div>
            <div class="result-item" style="border-bottom: 2px solid #ccc;">
                <strong>Total Points Earned:</strong>
                <span id="resTotalPoints" style="font-weight: 700; font-size: 18px;">0</span>
            </div>
            <div class="result-item">
                <strong>Total Cash Price (incl. tax):</strong>
                <span id="resTotalCash">$0.00</span>
            </div>
            <div class="result-item">
                <strong>Value of Points Earned (Rebate):</strong>
                <span id="resPointsValue">$0.00</span>
            </div>
            <div class="result-item" style="border-bottom: 2px solid #ccc;">
                <strong>Net Cost (After Rebate):</strong>
                <span id="resNetCost" style="font-weight: 700; font-size: 18px;">$0.00</span>
            </div>
            <div class="result-item" style="border-bottom: none;">
                <strong>Breakeven Points Price:</strong>
                <span id="resBreakeven">0 points</span>
            </div>
        </div>
    </div>

    <script>
        function calculatePoints() {
            // 1. Get all input values
            const cashPrice = parseFloat(document.getElementById('cashPrice').value) || 0;
            const taxFee = parseFloat(document.getElementById('taxFee').value) || 0;
            const brandMultiplier = parseFloat(document.getElementById('brandMultiplier').value) || 0;
            const statusMultiplier = parseFloat(document.getElementById('statusMultiplier').value) || 0;
            const ccMultiplier = parseFloat(document.getElementById('ccMultiplier').value) || 0;
            const pointValue = parseFloat(document.getElementById('pointValue').value) || 0.007;
            const welcomePoints = parseFloat(document.getElementById('welcomePoints').value) || 0;
            const promoPoints = parseFloat(document.getElementById('promoPoints').value) || 0;

            // 2. Perform Calculations
            const basePoints = cashPrice * brandMultiplier;
            const statusBonus = basePoints * (statusMultiplier / 100);
            const ccBonus = cashPrice * ccMultiplier;
            const totalPoints = basePoints + statusBonus + ccBonus + welcomePoints + promoPoints;

            const totalCash = cashPrice + taxFee;
            const pointsValueCash = totalPoints * pointValue;
            const netCost = totalCash - pointsValueCash;
            const breakevenPoints = (pointValue > 0) ? (totalCash / pointValue) : 0;

            // 3. Display Results
            // Helper function to format numbers
            const formatNum = (num) => num.toLocaleString(undefined, { maximumFractionDigits: 0 });
            const formatCurrency = (num) => num.toLocaleString('en-US', { style: 'currency', currency: 'USD' });

            // Update points breakdown
            document.getElementById('resBasePoints').innerText = formatNum(basePoints);
            document.getElementById('resStatusBonus').innerText = formatNum(statusBonus);
            document.getElementById('resCcBonus').innerText = formatNum(ccBonus);
            document.getElementById('resTotalPoints').innerText = formatNum(totalPoints);

            // Update cash and value breakdown
            document.getElementById('resTotalCash').innerText = formatCurrency(totalCash);
            document.getElementById('resPointsValue').innerText = formatCurrency(pointsValueCash);
            document.getElementById('resNetCost').innerText = formatCurrency(netCost);
            document.getElementById('resBreakeven').innerText = `${formatNum(breakevenPoints)} points`;
        }

        // Run the calculation on page load with default values
        window.onload = calculatePoints;
    </script>

</body>
</html>
