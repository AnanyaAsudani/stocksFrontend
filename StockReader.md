---
permalink: /reader
title: Reader
---

<head>
    <style>
        body {
            text-align: center;
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
            background-color: #222; 
            color: #fff; 
        }
        h1 {
            background-color: #004d99; 
            color: white;
            padding: 20px;
            margin: 0;
        }
        p {
            max-width: 800px;
            margin: 30px auto;
            padding: 10px;
            background-color: #333; 
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            color: #fff; 
        }
        label {
            display: block;
            margin-top: 20px;
            color: #fff; 
        }
        input {
            font-size: 16px;
            padding: 10px;
            border: 3px solid #e81cff; 
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
        }
        .button-container {
            display: flex;
            justify-content: center;
            margin-top: 30px;
            margin-bottom: 12px;
        }
        .button {
            position: relative;
            width: 120px;
            height: 40px;
            background-color: #000;
            display: flex;
            align-items: center;
            color: white;
            flex-direction: column;
            justify-content: center;
            border: none;
            padding: 12px;
            gap: 12px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 14px; 
        }
        .button::before {
            content: '';
            position: absolute;
            inset: 0;
            left: -4px;
            top: -1px;
            margin: auto;
            width: 128px;
            height: 48px;
            border-radius: 10px;
            background: linear-gradient(-45deg, #e81cff 0%, #004d99 100%);
            z-index: -10;
            pointer-events: none;
            transition: all 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
        }
        .button::after {
            content: "";
            z-index: -1;
            position: absolute;
            inset: 0;
            background: linear-gradient(-45deg, #fc00ff 0%, #00dbde 100%);
            transform: translate3d(0, 0, 0) scale(0.95);
            filter: blur(20px);
        }
        .button:hover::after {
            filter: blur(30px);
        }
        .button:hover::before {
            transform: rotate(-180deg);
        }
        .button:active::before {
            scale: 0.7;
        }
        .Example{
            margin-top: 0px;
        }
        .image-container {
            display: flex;
            justify-content: center;
            gap: 20px; 
            margin-top: 20px;
        }
        .image-box img {
            border: 5px solid #004d99;
            border-radius: 8px;
        }
        .container{
            margin-top: -40px;
        }
        #graph {
            max-width: 800px;
            margin: 20px auto;
        }
    </style>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
</head>

<body>
    <h1>Stock Viewer</h1>
    <p>Welcome to our Stock Viewer app where you can view the graphs and prices of different stocks using their stock ticker (e.g., AAPL, AMZN, etc.). Enter the desired stock ticker in all caps to generate an interactive graph showing data from the last 12 years.</p>
    <div class="container">
        <label for="stock-input">Enter a stock symbol:</label>
        <input id="stock-input" type="text">
        <div class="button-container">
            <button class="button" id="collect" onclick="getStockGraph()">Display Graph</button>
        </div>
    </div>
    <div id="graph-container">
        <div id="graph"></div>
    </div>

<h3 class="Example" >Image Examples:</h3>
   <div class="image-container">
        <div class="image-box">
            <img src="https://i.ibb.co/3vqVkLW/SCR-20231025-rrut.png" alt="Image 1">
        </div>
        <div class="image-box">
            <img src="https://i.ibb.co/7zG578m/SCR-20231025-rngg.png" alt="Image 2">
        </div>
    </div>
    <script>
        function getStockGraph() {
            const selectedStock = document.getElementById('stock-input').value;
            const button = document.getElementById('collect');
            const apiUrl = 'http://localhost:8282/api/stocks/stock_graph/' + selectedStock;
            button.textContent = 'Loading...';
            fetch(apiUrl)
                .then(response => response.json())
                .then(graphData => {
                    Plotly.newPlot('graph', graphData.data, graphData.layout);
                    button.textContent = 'Display Graph';
                })
                .catch(error => {
                    console.error('Error:', error);
                    button.textContent = 'Display Graph';
                });
        }
        const stockInput = document.getElementById('stock-input');
        const collectButton = document.getElementById('collect');
        stockInput.addEventListener("keydown", function (event) {
            if (event.key === 'Enter') {
                getStockGraph();
            }
        });
        collectButton.addEventListener("click", getStockGraph);
    </script>
</body>

