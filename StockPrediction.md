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
            margin-top: 20px;
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
        .container { 
             margin-top: -20px
        }
        .button-container2 {
            display: flex;
            justify-content: center;
            margin-top: 30px;
            margin-bottom: 30px;
        }
        .recommnedation { 
            margin-top: 0px; 
            margin-bottom: -1000px;
        }
        .reccomendatText {
            margin-top: 0px; 
        }
    </style>
</head>
<body>
    <h1>AI Assistant</h1>
    <p>Put in a stock, and the AI will use historical data and trends to analyze whether or not it is a good time to buy the stock.</p>
    <div class="container">
        <label for="update-input">AI stock buy prediction:</label>
        <input id="update-input" type="text">
        <div class="button-container2">
            <button class="button" id="get-latest-data" >Should I buy?</button>
        </div>
    </div>
    <div id="latest-data" class="latest-data"></div>
    <div id="data" style="display: none;">
        <h2>What does all this mean?</h2>
        <button id="toggle-definitions">Show</button>
        <div id="definitions" style="display: none;">
            <p>Prediction</p>
        </div>
    </div>
    <div class="recommnedation">
        <div id="recommendation">
            <h3>Recommendation:</h3>
            <p id="recommendation-text" class="reccomendatText"></p>
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
