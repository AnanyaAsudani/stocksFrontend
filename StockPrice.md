---
permalink: /stock_price
title: Stock Price
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
            margin-top: 35px;
            margin-bottom: 40px;
        }
        .button2-container {
            display: flex;
            justify-content: center;
            margin-top: 40px;
            margin-bottom: 40px; 
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
    </style>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
</head>
<body>
    <h1>Stock Price Viewer</h1>
    <div class="container">
        <p>Check the latest price for any stock ticker</p>
        <label for="update-input">Get Latest Stock Data:</label>
        <input id="update-input" type="text" placeholder="Enter ticker (e.g., AAPL)">
        <div class="button-container">
            <button id="get-latest-data" class="button">Latest Data</button>
        </div>
        <div id="latest-data" class="latest-data"></div>
        <div id="data" style="display: none;">
            <h2>What does all this mean?</h2>
             <div class="button2-container">
                 <button id="toggle-definitions" class="button">Show</button>
            </div>
            <div id="definitions" style="display: none;">
                <p><strong>Open:</strong> The opening price of a stock for the current trading day.</p>
                <p><strong>High:</strong> The highest price the stock reached during the current trading day.</p>
                <p><strong>Low:</strong> The lowest price the stock reached during the current trading day.</p>
                <p><strong>Close:</strong> The closing price of the stock for the current trading day.</p>
            </div>
        </div>
    </div>
    <script>
    const getLatestDataButton = document.getElementById('get-latest-data');
    const updateInput = document.getElementById('update-input');
    function getLatestStockData() {
        // get the stock ticker entered by the user
        const stockTicker = updateInput.value;
        getLatestDataButton.textContent = 'Loading...';
        // make an API request to get the latest stock data
        fetch("http://localhost:8282/api/stocks/latest_data/" + stockTicker)
            .then(response => response.json())
            .then(data => {
                // display the latest stock data on the page
                const latestDataElement = document.getElementById("latest-data");
                latestDataElement.innerHTML = `<h2>Latest Stock Data for ${stockTicker.toUpperCase()} In The Last Day:</h2>`;
                latestDataElement.innerHTML += `<p>Open: $${data.open}</p>`;
                latestDataElement.innerHTML += `<p>High: $${data.high}</p>`;
                latestDataElement.innerHTML += `<p>Low: $${data.low}</p>`;
                latestDataElement.innerHTML += `<p>Close/Last Price: $${data.close}</p>`
                const dataDiv = document.getElementById("data");
                dataDiv.style.display = "block";
                getLatestDataButton.textContent = 'Get Latest Data';
            })
            .catch(error => {
                console.error("Error:", error);
                getLatestDataButton.textContent = 'Get Latest Data';
            });
    }
    document.getElementById("get-latest-data").addEventListener("click", getLatestStockData);
    updateInput.addEventListener("keydown", function (event) {
        if (event.key === 'Enter') {
            getLatestStockData();
        }
    });
    const toggleButton = document.getElementById("toggle-definitions");
    const definitions = document.getElementById("definitions");
    toggleButton.addEventListener("click", function () {
        if (definitions.style.display === "none") {
            definitions.style.display = "block";
            toggleButton.textContent = "Hide";
        } else {
            definitions.style.display = "none";
            toggleButton.textContent = "Show";
        }
    });
    </script>
</body>
