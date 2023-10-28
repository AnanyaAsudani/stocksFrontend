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
            font-family: Arial, sans-serif;
            background-color: #f3f3f3;
        }

        h1 {
            background-color: #0073e6;
            color: white;
            padding: 20px;
            margin: 0;
        }

        p {
            max-width: 800px;
            margin: 30px auto;
            padding: 10px;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
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
            max-width: 400px;
        }
        .container {
            background-color: white;
            border-radius: 10px;
            padding: 20px;
            margin: 20px auto; 
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
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
            margin-top: 20px;
        }

        button:hover {
            background-color: #005cbf;
        }

        .latest-data {
            margin-top: 30px;
            max-width: 800px;
        }

        #recommendation {
            margin-top: 20px;
        }

        h3 {
            margin-top: 10px;
            color: #0073e6;
        }

        #recommendation-text {
            font-size: 18px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <h1>AI Assistant</h1>
    <p>Put in a stock, and the AI will use historical data and trends to analyze whether or not it is a good time to buy the stock.</p>
    <div class="container">
    <label for="update-input">AI stock buy prediction:</label>
    <input id="update-input" type="text">
    <button id="get-latest-data">Should I Buy?</button>
    </div>
    <div id="latest-data" class="latest-data"></div>
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
    const updateInput = document.getElementById('update-input');
    function getRecommendation() {
        const stockTicker = updateInput.value;
        getLatestDataButton.textContent = 'Loading...';
        // get the recommendation
        fetch(`http://localhost:8282/api/stocks/analyze/${stockTicker}`)
            .then(response => response.json())
            .then(recommendationData => {
                const recommendation = recommendationData.recommendation;
                const reason = recommendationData.reason;
                recommendationText.textContent = `${recommendation} (${reason})`;
                recommendationDiv.style.display = 'block';
                getLatestDataButton.textContent = 'Should I Buy?';
            })
            .catch(error => {
                console.error(error);
                getLatestDataButton.textContent = 'Should I Buy?';
            });
    }
    getLatestDataButton.addEventListener('click', getRecommendation);
    // Add event listener for the Enter key press in the input field
    updateInput.addEventListener("keydown", function (event) {
        if (event.key === 'Enter') {
            getRecommendation();
        }
    });
</script>

</body>
