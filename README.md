
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Electricity Bill Calculator</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f9f9f9;
            color: #333;
        }
        header, footer {
            background-color: #004aad;
            color: white;
            text-align: center;
            padding: 1rem 0;
        }
        main {
            max-width: 800px;
            margin: 20px auto;
            padding: 20px;
            background: white;
            border-radius: 8px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
        }
        label {
            display: block;
            margin-top: 15px;
        }
        input {
            width: 100%;
            padding: 10px;
            margin-top: 5px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        button {
            background-color: #004aad;
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
        }
        button:hover {
            background-color: #00308f;
        }
        .result {
            margin-top: 20px;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            background: #f1f1f1;
        }
        .user-section {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <header>
        <h1>Electricity Bill Calculator</h1>
    </header>
    <main>
        <form id="billForm">
            <label for="totalBill">Total Bill Amount (₹):</label>
            <input type="number" id="totalBill" placeholder="Enter total bill amount" required>

            <label for="totalUnits">Total Units Consumed (KWH):</label>
            <input type="number" id="totalUnits" placeholder="Enter total units consumed" required>

            <label for="numUsers">Number of Users:</label>
            <input type="number" id="numUsers" placeholder="Enter number of users" required>

            <div id="usersSection" class="user-section"></div>

            <button type="button" id="addUsersBtn">Add User Inputs</button>
            <button type="button" id="calculateBtn">Calculate</button>
        </form>

        <div id="results" class="result" style="display: none;">
            <h3>Results</h3>
            <p id="costPerUnit"></p>
            <div id="userBills"></div>
            <p id="meterBill"></p>
        </div>
    </main>
    <footer>
        <p>&copy; 2025 Electricity Calculator</p>
    </footer>

    <script>
        const addUsersBtn = document.getElementById('addUsersBtn');
        const calculateBtn = document.getElementById('calculateBtn');
        const usersSection = document.getElementById('usersSection');
        const results = document.getElementById('results');
        const userBills = document.getElementById('userBills');
        const costPerUnitDisplay = document.getElementById('costPerUnit');
        const meterBillDisplay = document.getElementById('meterBill');

        addUsersBtn.addEventListener('click', () => {
            const numUsers = document.getElementById('numUsers').value;
            usersSection.innerHTML = '';
            for (let i = 0; i < numUsers; i++) {
                usersSection.innerHTML += `
                    <h3>User ${i + 1}</h3>
                    <label>Previous Reading:</label>
                    <input type="number" id="reading1_${i}" placeholder="Enter previous month's reading" required>
                    <label>Current Reading:</label>
                    <input type="number" id="reading2_${i}" placeholder="Enter current month's reading" required>
                `;
            }
        });

        calculateBtn.addEventListener('click', () => {
            const totalBill = parseFloat(document.getElementById('totalBill').value);
            const totalUnits = parseFloat(document.getElementById('totalUnits').value);
            const numUsers = parseInt(document.getElementById('numUsers').value);

            if (!totalBill || !totalUnits || !numUsers) {
                alert('Please fill out all required fields.');
                return;
            }

            const costPerUnit = totalBill / totalUnits;
            costPerUnitDisplay.innerHTML = `Cost per Unit: ₹${costPerUnit.toFixed(2)}`;

            let totalSubmeterUnits = 0;
            userBills.innerHTML = '<h4>Submeter Bills:</h4>';

            for (let i = 0; i < numUsers; i++) {
                const reading1 = parseFloat(document.getElementById(`reading1_${i}`).value);
                const reading2 = parseFloat(document.getElementById(`reading2_${i}`).value);

                if (reading2 < reading1) {
                    alert(`Invalid readings for User ${i + 1}.`);
                    return;
                }

                const diff = reading2 - reading1;
                const submeterBill = diff * costPerUnit;

                totalSubmeterUnits += diff;

                userBills.innerHTML += `<p>User ${i + 1}: ${diff} units, Submeter Bill: ₹${submeterBill.toFixed(2)}</p>`;
            }

            const meterBill = totalBill - (totalSubmeterUnits * costPerUnit);
            meterBillDisplay.innerHTML = `Meter Bill (e.g., common area usage): ₹${meterBill.toFixed(2)}`;

            results.style.display = 'block';
        });
    </script>
</body>
</html>