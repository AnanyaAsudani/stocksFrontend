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
        }
        label {
            display: block;
            margin-top: 20px;
        }
        input {
            font-size: 16px;
            padding: 10px;
        }
        button {
            background-color: #0073e6;
            color: white;
            border: none;
            border-radius: 5px;
            padding: 10px 20px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #005cbf;
        }
        .latest-data{
            margin-top: 30px;
        }
    </style>
</head>
<body>
    <h1>Stock Price Viewer</h1>
    <p>Check the latest price for any stock ticker</p>
    <label for="update-input">Get Latest Stock Data:</label>
    <input id="update-input" type="text" style="border: 1px solid black;" placeholder="Enter ticker (e.g., AAPL)">
    <button id="get-latest-data">Get Latest Data</button>
    <div id="latest-data">
    </div>
    <div id="data" style="display: none;">
    <h2>What does all this mean?</h2>
    <button id="toggle-definitions">Show</button>
    <div id="definitions" style="display: none;">
        <p><strong>Open:</strong> The opening price of a stock for the current trading day.</p>
        <p><strong>High:</strong> The highest price the stock reached during the current trading day.</p>
        <p><strong>Low:</strong> The lowest price the stock reached during the current trading day.</p>
        <p><strong>Close:</strong> The closing price of the stock for the current trading day.</p>
    </div>
</div>
    <script>
        function getLatestStockData() {
            // get the stock ticker entered by the user
            const stockTicker = document.getElementById("update-input").value;
            // make a API request to get the latest stok data
            fetch("http://localhost:8282/api/stocks/latest_data/" + stockTicker)
                .then(response => response.json())
                .then(data => {
                    // display the latest stock data on the page
                    const latestDataElement = document.getElementById("latest-data");
                    latestDataElement.innerHTML = `<h2>Latest Stock Data for ${stockTicker} In The Last Day:</h2>`;
                    latestDataElement.innerHTML += `<p>Open: $${data.open}</p>`;
                    latestDataElement.innerHTML += `<p>High: $${data.high}</p>`;
                    latestDataElement.innerHTML += `<p>Low: $${data.low}</p>`;
                    latestDataElement.innerHTML += `<p>Close: $${data.close}</p>`
                    const dataDiv = document.getElementById("data");
            dataDiv.style.display = "block";
                })
                .catch(error => {
                    console.error("Error:", error);
                });
        }
        document.getElementById("get-latest-data").addEventListener("click", getLatestStockData);
        const toggleButton = document.getElementById("toggle-definitions");
    const definitions = document.getElementById("definitions")
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

