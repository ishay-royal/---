<!DOCTYPE html>
<html lang="he">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ניהול הזמנות - רויאל רסטורנט</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f8f9fa;
        }
        header {
            background-color: #343a40;
            color: white;
            padding: 1rem;
            text-align: center;
        }
        main {
            padding: 2rem;
        }
        .order-table {
            width: 100%;
            border-collapse: collapse;
            margin: 2rem 0;
            direction: rtl;
        }
        .order-table th, .order-table td {
            border: 1px solid #ddd;
            padding: 0.8rem;
            text-align: right;
        }
        .order-table th {
            background-color: #007bff;
            color: white;
        }
        .menu-table {
            width: 100%;
            border-collapse: collapse;
            margin: 2rem 0;
            direction: rtl;
        }
        .menu-table th, .menu-table td {
            border: 1px solid #ddd;
            padding: 0.8rem;
            text-align: right;
        }
        .menu-table th {
            background-color: #6c757d;
            color: white;
        }
        .add-order {
            margin: 1rem 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
            direction: rtl;
        }
        .add-order button {
            background-color: #28a745;
            color: white;
            border: none;
            padding: 0.5rem 1rem;
            cursor: pointer;
        }
        .add-order button:hover {
            background-color: #218838;
        }
        .export-buttons {
            margin-top: 1rem;
            direction: rtl;
        }
        .export-buttons button {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 0.5rem 1rem;
            cursor: pointer;
            margin-right: 1rem;
        }
        .export-buttons button:hover {
            background-color: #0056b3;
        }
        .language-select {
            margin: 1rem 0;
            text-align: center;
        }
        .language-select button {
            margin: 0 0.5rem;
            background-color: #6c757d;
            color: white;
            border: none;
            padding: 0.5rem 1rem;
            cursor: pointer;
        }
        .language-select button:hover {
            background-color: #5a6268;
        }
        .menu-select {
            margin: 1rem 0;
            display: flex;
            justify-content: space-between;
            direction: rtl;
        }
        .menu-select select {
            padding: 0.5rem;
        }
        .menu-select input {
            padding: 0.5rem;
            margin-right: 1rem;
        }
        .new-order {
            margin: 2rem 0;
            padding: 1rem;
            border: 1px solid #ddd;
            border-radius: 5px;
            background-color: #ffffff;
            direction: rtl;
        }
        .new-order label {
            margin-right: 1rem;
        }
        .new-order input {
            margin: 0.5rem 0;
            padding: 0.5rem;
            width: calc(100% - 1rem);
        }
        .new-order button {
            margin-top: 1rem;
            background-color: #007bff;
            color: white;
            border: none;
            padding: 0.5rem 1rem;
            cursor: pointer;
        }
        .new-order button:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
    <header>
        <h1 id="title">ניהול הזמנות - רויאל רסטורנט</h1>
    </header>
    <div class="language-select">
        <button onclick="changeLanguage('he')">עברית</button>
        <button onclick="changeLanguage('en')">English</button>
        <button onclick="changeLanguage('uk')">Українська</button>
    </div>
    <main>
        <div class="new-order">
            <h2>צור הזמנה חדשה</h2>
            <label for="tableNumberInput">מספר שולחן:</label>
            <input type="number" id="tableNumberInput" placeholder="הכנס מספר שולחן">
            <label for="numPeopleInput">מספר אנשים:</label>
            <input type="number" id="numPeopleInput" placeholder="הכנס מספר אנשים">
            <button onclick="createNewOrder()">צור הזמנה</button>
        </div>
        <div class="add-order">
            <h2 id="currentOrders">הזמנות נוכחיות</h2>
            <button id="addOrderBtn" onclick="addOrder()">הוסף פריט להזמנה</button>
        </div>
        <div class="menu-select">
            <label for="menu">בחר קטגוריה:</label>
            <select id="menu" onchange="updateMenu()">
                <option value="breakfast">בוקר</option>
                <option value="appetizers">ראשונות בשרי</option>
                <option value="main">עיקריות בשרי</option>
                <option value="drinks">שתייה</option>
            </select>
            <select id="menuItems">
                <!-- Items will be dynamically loaded here -->
            </select>
        </div>
        <div class="menu-select">
            <input type="text" id="newItemName" placeholder="שם פריט חדש">
            <input type="number" id="newItemPrice" placeholder="מחיר">
            <button onclick="addNewItem()">הוסף פריט חדש</button>
        </div>
        <table class="menu-table">
            <thead>
                <tr>
                    <th>קטגוריה</th>
                    <th>שם פריט</th>
                    <th>מחיר</th>
                </tr>
            </thead>
            <tbody id="menuTableBody">
                <!-- תפריט ייטען כאן -->
            </tbody>
        </table>
        <table class="order-table">
            <thead>
                <tr>
                    <th id="orderId">מספר הזמנה</th>
                    <th id="tableNumber">מספר שולחן</th>
                    <th id="numPeople">מספר אנשים</th>
                    <th id="orderDetails">פרטי הזמנה</th>
                    <th id="price">מחיר</th>
                    <th id="status">סטטוס</th>
                    <th id="actions">פעולות</th>
                </tr>
            </thead>
            <tbody id="orderTableBody">
                <!-- הזמנות יתווספו כאן באופן דינמי -->
            </tbody>
        </table>
        <div class="export-buttons">
            <button id="exportKitchen" onclick="exportToKitchen()">יצוא למטבח</button>
            <button id="generateBill" onclick="generateBill()">הפקת חשבון</button>
        </div>
    </main>
    <script>
        const menu = {
            breakfast: [
                { name: "ביצים לבחירה", price: 20 },
                { name: "שקשוקה", price: 20 },
                { name: "רויאל טוסט", price: 20 }
            ],
            appetizers: [
                { name: "אדממה", price: 10 },
                { name: "לחם מטבלים", price: 10 },
                { name: "מרק היום", price: 10 },
                { name: "פטריה מטוגנת", price: 10 },
                { name: "סיגר מרוקאי", price: 15 },
                { name: "מקדינה", price: 15 }
            ],
            main: [
                { name: "רויאל בורגר", price: 25 },
                { name: "פרגית", price: 25 },
                { name: "החריימה של דניאל", price: 25 },
                { name: "דג סלמון", price: 25 }
            ],
            drinks: [
                { name: "קוקה קולה", price: 2 },
                { name: "זירו", price: 2 },
                { name: "ספרייט", price: 2 },
                { name: "סודה", price: 1 },
                { name: "מים", price: 1 },
                { name: "בירה קורונה", price: 5 }
            ]
        };

        function updateMenu() {
            const category = document.getElementById('menu').value;
            const menuItems = document.getElementById('menuItems');
            menuItems.innerHTML = '';

            menu[category].forEach(item => {
                const option = document.createElement('option');
                option.value = item.name;
                option.textContent = `${item.name} - ₪${item.price}`;
                menuItems.appendChild(option);
            });
            updateMenuTable();
        }

        function updateMenuTable() {
            const menuTableBody = document.getElementById('menuTableBody');
            menuTableBody.innerHTML = '';

            Object.keys(menu).forEach(category => {
                menu[category].forEach(item => {
                    const row = document.createElement('tr');
                    row.innerHTML = `
                        <td>${category}</td>
                        <td>${item.name}</td>
                        <td>₪${item.price.toFixed(2)}</td>
                    `;
                    menuTableBody.appendChild(row);
                });
            });
        }

        updateMenu(); // Initialize menu on load
        updateMenuTable();

        function createNewOrder() {
            const tableNumber = document.getElementById('tableNumberInput').value;
            const numPeople = document.getElementById('numPeopleInput').value;

            if (!tableNumber || !numPeople) {
                alert('אנא מלא את מספר השולחן ומספר האנשים');
                return;
            }

            const tableBody = document.getElementById('orderTableBody');
            const newRow = document.createElement('tr');
            newRow.innerHTML = `
                <td>${tableBody.rows.length + 1}</td>
                <td>${tableNumber}</td>
                <td>${numPeople}</td>
                <td>הזמנה חדשה</td>
                <td>₪0.00</td>
                <td><select>
                        <option value="Pending">ממתין</option>
                        <option value="In Progress">בתהליך</option>
                        <option value="Completed">הושלם</option>
                    </select></td>
                <td><button onclick="removeOrder(this)">הסר</button></td>
            `;

            tableBody.appendChild(newRow);

            // Clear inputs
            document.getElementById('tableNumberInput').value = '';
            document.getElementById('numPeopleInput').value = '';

            alert('הזמנה חדשה נוצרה בהצלחה');
        }

        function addOrder() {
            const tableBody
