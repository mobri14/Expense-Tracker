# Expense-Tracker
A simple and modern Expense Tracker built with HTML, CSS, and JavaScript.
## Features

- Add income and expenses
- Calculate total balance
- View transaction history
- Delete transactions
- Save data using Local Storage
- Responsive design

## Technologies Used

- HTML5
- CSS3
- JavaScript (Vanilla JS)
- Local Storage

## Installation

1. Clone the repository
2. Open index.html in your browser

## License

MIT


<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Expense Tracker</title>

<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Arial,sans-serif;
}

body{
background:#f5f7fa;
display:flex;
justify-content:center;
padding:30px;
}

.container{
width:100%;
max-width:500px;
background:white;
padding:20px;
border-radius:15px;
box-shadow:0 4px 15px rgba(0,0,0,.1);
}

h1{
text-align:center;
margin-bottom:20px;
}

.balance{
text-align:center;
margin-bottom:20px;
}

.balance h2{
color:#2e7d32;
}

form{
display:flex;
flex-direction:column;
gap:10px;
margin-bottom:20px;
}

input{
padding:10px;
border:1px solid #ccc;
border-radius:8px;
}

button{
padding:12px;
border:none;
background:#1976d2;
color:white;
border-radius:8px;
cursor:pointer;
}

button:hover{
background:#1565c0;
}

.list{
list-style:none;
}

.list li{
display:flex;
justify-content:space-between;
align-items:center;
padding:10px;
margin:8px 0;
border-radius:8px;
background:#f0f0f0;
}

.income{
border-left:5px solid green;
}

.expense{
border-left:5px solid red;
}

.delete{
background:red;
padding:5px 10px;
border-radius:5px;
color:white;
cursor:pointer;
}
</style>
</head>
<body>

<div class="container">

<h1>Expense Tracker</h1>

<div class="balance">
<h3>Current Balance</h3>
<h2 id="balance">$0</h2>
</div>

<form id="form">
<input type="text" id="text" placeholder="Description" required>
<input type="number" id="amount" placeholder="Amount (+ income, - expense)" required>
<button type="submit">Add Transaction</button>
</form>

<ul id="list" class="list"></ul>

</div>

<script>

let transactions =
JSON.parse(localStorage.getItem("transactions")) || [];

const balanceEl = document.getElementById("balance");
const listEl = document.getElementById("list");

function saveData(){
localStorage.setItem(
"transactions",
JSON.stringify(transactions)
);
}

function updateUI(){

listEl.innerHTML = "";

let balance = 0;

transactions.forEach((t,index)=>{

balance += Number(t.amount);

const li = document.createElement("li");

li.className =
Number(t.amount) > 0
? "income"
: "expense";

li.innerHTML = `
<span>${t.text} (${t.amount}$)</span>
<span class="delete" onclick="removeTransaction(${index})">
X
</span>
`;

listEl.appendChild(li);

});

balanceEl.innerText = "$" + balance.toFixed(2);

saveData();
}

function removeTransaction(index){
transactions.splice(index,1);
updateUI();
}

document
.getElementById("form")
.addEventListener("submit",(e)=>{

e.preventDefault();

const text =
document.getElementById("text").value;

const amount =
document.getElementById("amount").value;

transactions.push({
text,
amount
});

e.target.reset();

updateUI();

});

updateUI();

</script>

</body>
</html>
