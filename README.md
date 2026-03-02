<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Beauty Battle</title>
<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #1e1e2f, #2c2c54);
    color: white;
    text-align: center;
}

h1 {
    padding-top: 20px;
}

.container {
    display: flex;
    justify-content: center;
    gap: 50px;
    margin-top: 40px;
}

.card {
    cursor: pointer;
    transition: 0.3s;
}

.card:hover {
    transform: scale(1.05);
}

img {
    width: 260px;
    height: 360px;
    object-fit: cover;
    border-radius: 20px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.5);
}

.name {
    margin-top: 10px;
    font-size: 20px;
    font-weight: bold;
}

.rating {
    font-size: 14px;
    opacity: 0.8;
}

.leaderboard {
    margin-top: 60px;
}

table {
    margin: 0 auto;
    border-collapse: collapse;
    width: 50%;
}

th, td {
    padding: 10px;
}

th {
    border-bottom: 2px solid white;
}
</style>
</head>
<body>

<h1>🔥 Кто более привлекательная?</h1>

<div class="container">
    <div class="card" onclick="vote(left)">
        <img id="leftImg">
        <div class="name" id="leftName"></div>
        <div class="rating" id="leftRating"></div>
    </div>

    <div class="card" onclick="vote(right)">
        <img id="rightImg">
        <div class="name" id="rightName"></div>
        <div class="rating" id="rightRating"></div>
    </div>
</div>

<div class="leaderboard">
    <h2>🏆 Рейтинг</h2>
    <table id="rankingTable"></table>
</div>

<script>
const K = 32;

let girls = [
    {name: "Zendaya", rating: 1200, img: "https://via.placeholder.com/300x400"},
    {name: "Taylor Swift", rating: 1200, img: "https://via.placeholder.com/300x400"},
    {name: "Margot Robbie", rating: 1200, img: "https://via.placeholder.com/300x400"},
    {name: "Dua Lipa", rating: 1200, img: "https://via.placeholder.com/300x400"},
    {name: "Ana de Armas", rating: 1200, img: "https://via.placeholder.com/300x400"}
];

let left, right;

function expectedScore(rA, rB) {
    return 1 / (1 + Math.pow(10, (rB - rA) / 400));
}

function updateElo(winner, loser) {
    let expectedWin = expectedScore(winner.rating, loser.rating);
    let expectedLose = expectedScore(loser.rating, winner.rating);

    winner.rating = Math.round(winner.rating + K * (1 - expectedWin));
    loser.rating = Math.round(loser.rating + K * (0 - expectedLose));
}

function newPair() {
    left = girls[Math.floor(Math.random() * girls.length)];
    right = girls[Math.floor(Math.random() * girls.length)];

    while (left === right) {
        right = girls[Math.floor(Math.random() * girls.length)];
    }

    document.getElementById("leftImg").src = left.img;
    document.getElementById("leftName").innerText = left.name;
    document.getElementById("leftRating").innerText = "Рейтинг: " + left.rating;

    document.getElementById("rightImg").src = right.img;
    document.getElementById("rightName").innerText = right.name;
    document.getElementById("rightRating").innerText = "Рейтинг: " + right.rating;

    updateLeaderboard();
}

function vote(choice) {
    let winner = choice;
    let loser = (choice === left) ? right : left;

    updateElo(winner, loser);
    newPair();
}

function updateLeaderboard() {
    let sorted = [...girls].sort((a, b) => b.rating - a.rating);
    let table = "<tr><th>Место</th><th>Имя</th><th>Рейтинг</th></tr>";

    sorted.forEach((g, i) => {
        table += `<tr><td>${i+1}</td><td>${g.name}</td><td>${g.rating}</td></tr>`;
    });

    document.getElementById("rankingTable").innerHTML = table;
}

newPair();
</script>

</body>
</html>
