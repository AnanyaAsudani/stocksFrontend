---
permalink: /stock_analyze
title: Analyze Price
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
        .latest-data {
            margin-top: 30px;
        }
        #recommendation {
            margin-top: 20px;
        }
    </style>
</head>

<body>
    <h1>AI Assistant</h1>
    <p> Put in a stock and the AI will use historical data and trends to analyze whether or not it is a good time to buy the stock</p>
    <label for="update-input">AI stock buy prediction:</label>
    <input id="update-input" type="text" style="border: 1px solid black;" placeholder="Enter ticker (e.g., AAPL)">
    <button id="get-latest-data">Should I Buy?</button>
    <div id="latest-data">
    </div>
    <div id="data" style="display: none;">
        <h2>What does all this mean?</h2>
        <button id="toggle-definitions">Show</button>
        <div id="definitions" style="display: none;">
            <p>Prediction</p>
        </div>
    </div>
    <div id="recommendation">
        <h3>Recommendation:</h3>
        <p id="recommendation-text"></p>
    </div>
    <script>
        const getLatestDataButton = document.getElementById('get-latest-data');
        const latestDataDiv = document.getElementById('latest-data');
        const dataDiv = document.getElementById('data');
        const recommendationDiv = document.getElementById('recommendation');
        const recommendationText = document.getElementById('recommendation-text');
        getLatestDataButton.addEventListener('click', () => {
            const stockTicker = document.getElementById('update-input').value;
            getLatestDataButton.textContent = 'Loading...';
            // get the recommendation
            fetch(`http://localhost:8282/api/stocks/analyze/${stockTicker}`)
                .then(response => response.json())
                .then(recommendationData => {
                    const recommendation = recommendationData.recommendation;
                    const reason = recommendationData.reason;
                    recommendationText.textContent = `Recommendation: ${recommendation} (${reason})`;
                    recommendationDiv.style.display = 'block';
                    getLatestDataButton.textContent = 'Should I Buy?';
                })
                .catch(error => {
                    console.error(error);
                    getLatestDataButton.textContent = 'Should I Buy?';
                });
        });
    </script>
</body>
