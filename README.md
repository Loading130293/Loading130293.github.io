
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
            max-width: 700px;
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

        /* Base styles for both inputs and selects */
        .form-group input,
        .form-group select {
            padding: 10px 12px;
            border: 1px solid #ccc;
            border-radius: 5px;
            font-size: 16px;
            font-family: inherit;
            background-color: #fff;
            width: 100%;
            box-sizing: border-box;
            -webkit-appearance: none;
            appearance: none;
        }

        /* Add a custom dropdown arrow to SELECT elements */
        .form-group select {
            background-image: url('data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%22292.4%22%20height%3D%22292.4%22%3E%3Cpath%20fill%3D%22%23666666%22%20d%3D%22M287%2069.4a17.6%2017.6%200%200%200-13-5.4H18.4c-5%200-9.3%201.8-13%205.4A17.6%2017.6%200%200%200%200%2082.2c0%205%201.8%209.3%205.4%2013l128%20127.9c3.6%203.6%207.8%205.4%2013%205.4s9.4-1.8%2013-5.4L287%2095c3.5-3.5%205.4-7.8%205.4-13%200-5-1.9-9.2-5.4-12.6z%22%2F%3E%3C%2Fsvg%3E');
            background-repeat: no-repeat;
            background-position: right 12px center;
            background-size: 10px;
            padding-right: 35px;
        }

        /* Remove spinners from number inputs */
        .form-group input[type="number"]::-webkit-inner-spin-button,
        .form-group input[type="number"]::-webkit-outer-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
        .form-group input[type="number"] {
            -moz-appearance: textfield;
        }

        .full-width {
            grid-column: 1 / -1;
        }
        
        .clear-button {
            padding: 10px;
            font-size: 15px;
            font-weight: 600;
            color: #333;
            background-color: #f0f0f0;
            border: 1px solid #ccc;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.2s;
            width: 100%;
        }
        .clear-button:hover {
            background-color: #e0e0e0;
        }

        .results {
            background-color: #f9f9f4;
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
        .tooltip {
            font-size: 12px;
            color: #666;
            margin-top: 4px;
        }
        
        .result-explainer {
            font-size: 13px;
            color: #666;
            margin-top: 8px;
            text-align: right;
            border-bottom: none;
            padding: 0;
        }

        @media (max-width: 600px) {
            .calculator-body {
                grid-template-columns: 1fr;
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
                <input type="number" id="cashPrice">
            </div>
            <div class="form-group">
                <label for="taxFee">Tax & Fees</label>
                <input type="number" id="taxFee">
            </div>

            <div class="form-group">
                <label for="brandMultiplier">Brand Earning Rate</label>
                <select id="brandMultiplier">
                    <option value="10" selected>Standard Brands (10X)</option>
                    <option value="5">Residence Inn, TownePlace, Element (5X)</option>
                    <option value="4">StudioRes (4X)</option>
                    <option value="2.5">Marriott Exec. Apts (2.5X)</option>
                </select>
            </div>
            <div class="form-group">
                <label for="statusMultiplier">Status Level</label>
                <select id="statusMultiplier">
                    <option value="0" selected>Member (0% Bonus)</option>
                    <option value="10">Silver (10% Bonus)</option>
                    <option value="25">Gold (25% Bonus)</option>
                    <option value="50">Platinum (50% Bonus)</option>
                    <option value="75" >Titanium (75% Bonus)</option>
                    <option value="75">Ambassador (75% Bonus)</option>
                </select>
            </div>
            
            <div class="form-group">
                <label for="ccMultiplier">Credit Card</label>
                <select id="ccMultiplier">
                    <option value="0" selected>No Marriott Card (0X)</option>
                    <option value="3">Chase Marriott Bold (3X)</option>
                    <option value="6">All Other Marriott Cards (6X)</option>
                </select>
            </div>
            <div class="form-group">
                <label for="pointValue">Your Point Value (e.g., 0.007)</label>
                <input type="number" id="pointValue" step="0.001" value="0.005">
            </div>
            
            <div class="form-group full-width">
                <label for="welcomePoints">Welcome Gift: Stay at select brand?</label>
                <select id="welcomePoints">
                    <option value="500">Yes (500 Points)</option>
                    <option value="1000" selected>No (1000 Points)</option>
                </select>
                <div class="tooltip">Select 'Yes' for: Courtyard, AC, Moxy, Fairfield, Residence Inn, Four Points, SpringHill, TownePlace, Aloft.</div>
            </div>
            <div class="form-group full-width">
                <label for="promoPoints">Other Promotion Points</label>
                <input type="number" id="promoPoints" value="2025">
            </div>

            <div class="form-group full-width" style="border-top: 1px solid #eee; padding-top: 20px;">
                <label for="altPrice">Paid with Discounted Gift Card? Enter total price here:</label>
                <input type="number" id="altPrice" placeholder="e.g., 200">
                <div class="tooltip">If filled, this overrides 'Total Cash Price' in results. Points are still based on the 'Base Cash Price'.</div>
            </div>

            <div class="form-group full-width" style="margin-top: 10px;">
                <button id="clearButton" class="clear-button">Clear All Inputs</button>
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
            <div class="result-item">
                <strong>Welcome Points:</strong>
                <span id="resWelcomePoints">0</span>
            </div>
            <div class="result-item">
                <strong>Promotion Points:</strong>
                <span id="resPromoPoints">0</span>
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
            
            <div class="result-explainer">
                Points needed to match the net cost of this stay.
            </div>

        </div>
    </div>

    <script>
        function calculatePoints() {
            // 1. Get all input values.
            const cashPrice = parseFloat(document.getElementById('cashPrice').value) || 0;
            const taxFee = parseFloat(document.getElementById('taxFee').value) || 0;
            const brandMultiplier = parseFloat(document.getElementById('brandMultiplier').value) || 0;
            const statusMultiplier = parseFloat(document.getElementById('statusMultiplier').value) || 0;
            const ccMultiplier = parseFloat(document.getElementById('ccMultiplier').value) || 0; 
            const pointValue = parseFloat(document.getElementById('pointValue').value) || 0.007; // Fallback to 0.007
            const welcomePoints = parseFloat(document.getElementById('welcomePoints').value) || 0;
            const promoPoints = parseFloat(document.getElementById('promoPoints').value) || 0;
            const altPrice = parseFloat(document.getElementById('altPrice').value) || 0; 

            // 2. Perform Calculations
            const basePoints = cashPrice * brandMultiplier;
            const statusBonus = basePoints * (statusMultiplier / 100); 
            const ccBonus = cashPrice * ccMultiplier; 
            const totalPoints = basePoints + statusBonus + ccBonus + welcomePoints + promoPoints;

            const totalCashOriginal = cashPrice + taxFee;
            const totalCash = (altPrice > 0) ? altPrice : totalCashOriginal;

            const pointsValueCash = totalPoints * pointValue;
            const netCost = totalCash - pointsValueCash;
            const breakevenPoints = (pointValue > 0) ? (netCost / pointValue) : 0;

            // 3. Display Results
            const formatNum = (num) => num.toLocaleString(undefined, { maximumFractionDigits: 0 });
            const formatCurrency = (num) => num.toLocaleString('en-US', { style: 'currency', currency: 'USD' });

            document.getElementById('resBasePoints').innerText = formatNum(basePoints);
            document.getElementById('resStatusBonus').innerText = formatNum(statusBonus);
            document.getElementById('resCcBonus').innerText = formatNum(ccBonus);
            document.getElementById('resWelcomePoints').innerText = formatNum(welcomePoints);
            document.getElementById('resPromoPoints').innerText = formatNum(promoPoints);
            document.getElementById('resTotalPoints').innerText = formatNum(totalPoints);

            document.getElementById('resTotalCash').innerText = formatCurrency(totalCash);
            document.getElementById('resPointsValue').innerText = formatCurrency(pointsValueCash);
            document.getElementById('resNetCost').innerText = formatCurrency(netCost);
            document.getElementById('resBreakeven').innerText = `${formatNum(breakevenPoints)} points`;
        }

        // --- UPDATED CLEAR FUNCTION ---
        function clearInputs() {
            // Set all number inputs to empty
            document.getElementById('cashPrice').value = "";
            document.getElementById('taxFee').value = "";
            document.getElementById('promoPoints').value = "";
            document.getElementById('altPrice').value = "";

            // Reset point value to its default
            document.getElementById('pointValue').value = "0.007";

            // Reset dropdowns to their first option (index 0)
            document.getElementById('brandMultiplier').selectedIndex = 0; // "Standard Brands (10X)"
            document.getElementById('statusMultiplier').selectedIndex = 0; // "Member (0% Bonus)"
            document.getElementById('ccMultiplier').selectedIndex = 0; // "No Marriott Card (0X)"
            document.getElementById_('welcomePoints').selectedIndex = 0; // "Yes (500 Points)"

            // After clearing, re-run the calculation
            calculatePoints();
        }
        // --- END NEW FUNCTION ---

        // Add event listeners to all input/select fields to auto-calculate
        document.getElementById('cashPrice').addEventListener('input', calculatePoints);
        document.getElementById('taxFee').addEventListener('input', calculatePoints);
        document.getElementById('brandMultiplier').addEventListener('change', calculatePoints);
        document.getElementById('statusMultiplier').addEventListener('change', calculatePoints);
        document.getElementById('ccMultiplier').addEventListener('change', calculatePoints);
        document.getElementById('pointValue').addEventListener('input', calculatePoints);
        document.getElementById('welcomePoints').addEventListener('change', calculatePoints);
        document.getElementById('promoPoints').addEventListener('input', calculatePoints);
        document.getElementById('altPrice').addEventListener('input', calculatePoints);
        
        document.getElementById('clearButton').addEventListener('click', clearInputs);
        
        // Run the calculation on page load with default values
        window.onload = calculatePoints;
    </script>

</body>
</html>
