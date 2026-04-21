# 92-cafe-loyalty-system-<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>92 Cafe Loyalty System</title>
<style>
body {
    font-family: Arial;
    background: #f5f5f5;
    text-align: center;
}
.container {
    background: white;
    padding: 20px;
    margin: 30px auto;
    width: 350px;
    border-radius: 10px;
    box-shadow: 0 0 10px gray;
}
input, button {
    padding: 10px;
    margin: 5px;
    width: 90%;
}
button {
    background: black;
    color: white;
    border: none;
    cursor: pointer;
}
.card {
    background: #222;
    color: white;
    padding: 15px;
    margin-top: 15px;
    border-radius: 10px;
}
</style>
</head>
<body>

<div class="container">
    <h2>☕ 92 Cafe Loyalty</h2>

    <input type="text" id="phone" placeholder="Enter Phone Number">

    <button onclick="searchCustomer()">Search / Add Customer</button>
    <button onclick="addVisit()">+1 Visit</button>
    <button onclick="resetCustomer()">Reset</button>

    <div class="card" id="result">
        No customer selected
    </div>
</div>

<script>
let currentCustomer = null;

// Load database
let db = JSON.parse(localStorage.getItem("loyaltyDB")) || {};

function saveDB() {
    localStorage.setItem("loyaltyDB", JSON.stringify(db));
}

function searchCustomer() {
    let phone = document.getElementById("phone").value;

    if (!phone) {
        alert("Enter phone number");
        return;
    }

    if (!db[phone]) {
        db[phone] = { visits: 0 };
    }

    currentCustomer = phone;
    updateUI();
}

function addVisit() {
    if (!currentCustomer) {
        alert("Search customer first");
        return;
    }

    db[currentCustomer].visits += 1;

    // Reward system
    if (db[currentCustomer].visits >= 10) {
        alert("🎉 Reward! Free Coffee!");
        db[currentCustomer].visits = 0;
    }

    saveDB();
    updateUI();
}

function resetCustomer() {
    if (!currentCustomer) return;

    db[currentCustomer].visits = 0;
    saveDB();
    updateUI();
}

function updateUI() {
    let visits = db[currentCustomer].visits;

    document.getElementById("result").innerHTML = `
        📞 Phone: ${currentCustomer} <br>
        ☕ Visits: ${visits}/10 <br>
        🎁 Reward: ${10 - visits} visits left
    `;
}
</script>

</body>
</html>
