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
            background-color: #f3f3f3;
            font-family: Arial, sans-serif;
        }

        h1 {
            background-color: #0073e6;
            color: white;
            padding: 20px;
            margin: 0;
        }

        .container {
            background-color: white;
            border-radius: 10px;
            padding: 20px;
            margin: 20px auto; 
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
            max-width: 800px; 
        }

        p {
            margin: 10px 0;
        }

        label {
            display: block;
            font-weight: bold;
            margin-top: 20px;
        }

        input {
            font-size: 16px;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            max-width: 800px;
        }

        button {
            background-color: #0073e6;
            color: white;
            border: none;
            border-radius: 5px;
            padding: 10px 20px;
            cursor: pointer;
            transition: background-color 0.3s;
            margin-top: 30px;
        }

        button:hover {
            background-color: #005cbf;
        }

        .latest-data {
            margin-top: 30px;
        }

        h2 {
            color: #0073e6;
        }

        #toggle-definitions {
            background-color: #0073e6;
            color: white;
            border: none;
            border-radius: 5px;
            padding: 10px 20px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        #toggle-definitions:hover {
            background-color: #005cbf;
        }
    </style>
</head>
<body>
    <h1>Stock Price Viewer</h1>
    <div class="container">
        <p>Check the latest price for any stock ticker</p>
        <label for="update-input">Get Latest Stock Data:</label>
        <input id="update-input" type="text" placeholder="Enter ticker (e.g., AAPL)">
        <button id="get-latest-data">Get Latest Data</button>
        <div id="latest-data" class="latest-data"></div>
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

