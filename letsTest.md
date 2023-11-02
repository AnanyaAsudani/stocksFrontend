
<head>
    <style>
        body {
            text-align: center;
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
            background-color: #222; /* Dark background color */
            color: #fff; /* White text color */
        }
        h1 {
            background-color: #004d99; /* Darker blue for the h1 background */
            color: white;
            padding: 20px;
            margin: 0;
        }
        p {
            max-width: 800px;
            margin: 30px auto;
            padding: 10px;
            background-color: #333; /* Slightly darker background color for paragraphs */
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            color: #fff; /* White text color for paragraphs */
        }
        label {
            display: block;
            margin-top: 20px;
            color: #fff; /* White text color for labels */
        }
        input {
            font-size: 16px;
            padding: 10px;
            border: 2px solid #e81cff; /* Add a #e81cff border around the text boxes */
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.2); /* Add a glow effect to text boxes */
        }
        .button-container {
            display: flex;
            justify-content: center;
            margin-top: 35px;
            margin-bottom: 40px;
        }
        .button.add-button {
            margin-right: 50px; /* Add some right margin to create space between the buttons */
        }
        .button2-container {
            display: flex;
            justify-content: center;
            margin-top: 25px;
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
            font-size: 14px; /* Make the text smaller */
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
        #graph {
            max-width: 800px;
            margin: 20px auto;
            margin-bottom: 75px; 
        }
        #dictionaryContent {
            display: none;
        }
        .dictionaryPos { 
            margin-top: -50px;
        }
        .container2 { 
            margin-top: -20px;
        }
    </style>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
</head>

<body>
    <h1>Stock Portfolio Optimization</h1>
    <p>This is a portfolio simulator where you can put up to 8 stocks to find the most optimal weighting between them and see it on a graph. There are also other terms that will be defined below.</p>
    <p>Enter up to 8 stock tickers by adding one and then clicking the add stock button. When you are done, click calculate:</p>
    <div class="container2">
    <form id="stock-form">
        <label for="update-input">Input up to 8 stocks:</label>
        <input id="update-input" type="text" placeholder="Enter a ticker (e.g., AAPL)">
        <div class="button-container">
    <button type="button" class="button add-button" id="add-stock">Add stock</button>
    <button type="submit" class="button" id="calculate">Calculate</button>
</div>
    </form>
    </div>
    <div id="result" class="latest-data"></div>
    <div id="graph" class="latest-data"></div>
    <div id="dictionary" class="dictionaryPos"></div>
        <h2>Dictionary</h2>
        <div class="button2-container">
            <button class="button" id="toggleDictionary">Show</button>
        </div>
        <div id="dictionaryContent">
            <p><strong>Volatility:</strong> Volatility measures how much a financial asset's price changes over time, indicating the level of risk in a portfolio. Higher volatility means greater price fluctuations.</p>
            <p><strong>Return:</strong> Return is the profit or loss from an investment compared to the initial amount invested. It's usually expressed as a percentage of the initial investment.</p>
            <p><strong>Optimal Weights:</strong> Optimal weights are the allocation of capital to different assets in a portfolio to achieve a desired risk-return balance. They are the weightings for each asset that maximize the Sharpe Ratio, showing the best trade-off between return and risk.</p>
            <p><strong>Sharpe Ratio:</strong> The Sharpe Ratio is a measure that evaluates the risk-adjusted return of a portfolio. It indicates how well an investment has performed given its level of risk. A higher Sharpe Ratio suggests a more favorable balance between risk and return, making it a valuable tool for assessing and comparing investment performance.</p>
            <p><strong>What is a good Sharpe Ratio?</strong> A good Sharpe Ratio typically falls in the range of 0.5 to 1.0 or higher. The higher the ratio, the better, as it indicates a higher return for the level of risk taken. However, the definition of "good" may vary based on individual risk preferences and the specific investment objectives.</p>
            <p><strong>Graph Explanation:</strong> The graph shows a scatter plot with portfolio return on the y-axis and volatility (risk) on the x-axis. Each point on the graph represents a unique portfolio with different asset weightings. The color of each point indicates the Sharpe Ratio, with darker colors showing a higher ratio.</p>
            <p><strong>Red Dot (Most Optimal):</strong> The red dot on the graph represents the most optimal portfolio, which has the highest Sharpe Ratio, indicating the best risk-return trade-off. Investors often aim to construct a portfolio that closely resembles this point for optimal performance.</p>
            <p><strong>Most Optimal:</strong> "Most optimal" refers to the portfolio that offers the best risk-adjusted return, typically determined by the Sharpe Ratio. It represents the ideal balance between risk and return and is the goal for portfolio optimization.</p>
        </div>
    <script>
    const stockForm = document.getElementById("stock-form");
    const updateInput = document.getElementById("update-input");
    const addStockButton = document.getElementById("add-stock");
    const calculateButton = document.getElementById("calculate");
    const resultDiv = document.getElementById("result");
    const graphDiv = document.getElementById("graph");
    let stocks = [];
    addStockButton.addEventListener("click", function () {
        const stockTicker = updateInput.value.trim().toUpperCase();
        if (stockTicker && stocks.length < 8 && !stocks.includes(stockTicker)) {
            stocks.push(stockTicker);
            updateInput.value = "";
            updateInput.placeholder = "Enter another ticker";
            resultDiv.innerHTML = `Selected stocks: ${stocks.join(', ')}`;
        }
    });
    stockForm.addEventListener("submit", function (e) {
        e.preventDefault();
        calculateButton.textContent = "Loading...";
        const stockData = { stocks };
        fetch('http://localhost:8282/api/stocks/optimal_weights', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(stockData)
        })
        .then(response => response.json())
        .then(data => {
            const optimalWeights = data.max_sharpe_weights.map(weight => weight.toFixed(2));
            //display the results
            resultDiv.innerHTML = `
            <p>Maximum Sharpe Ratio Portfolio:</p>
            <p>Expected Return: ${data.max_sharpe_ret.toFixed(2)}</p>
            <p>Volatility: ${data.max_sharpe_vol.toFixed(2)}</p>
            <p>Sharpe Ratio: ${data.max_sharpe.toFixed(2)}</p>
            <p>Optimal Weights: ${optimalWeights.join(', ')}</p>
        `;
            // plotly scatter plot for portfolio optimization
            const scatterData = [{
                x: data.volatility,
                y: data.return,
                mode: 'markers',
                type: 'scatter',
                marker: { 
                    size: 12, 
                    color: data.sharpe,
                    colorscale: 'Viridis', 
                    opacity: 0.7,
                    colorbar: { title: 'Sharpe Ratio' }  //colorbar for clarity
                },
                text: stocks
            }];
            // crreate a separate trace for the red dot (most optimal point)
            const redDotData = [{
                x: [data.max_sharpe_vol.toFixed(4)],
                y: [data.max_sharpe_ret.toFixed(4)],
                mode: 'markers',
                type: 'scatter',
                marker: {
                    size: 12,
                    color: 'red'
                },
                text: 'Most Optimal'
            }];
            // combine the red dot trace and the scatter plot data
            scatterData.push(...redDotData);
            const scatterLayout = {
                title: 'Portfolio Optimization',
                xaxis: { title: 'Volatility' },
                yaxis: { title: 'Return' },
                hovermode: 'closest'
            };
            Plotly.newPlot(graphDiv, scatterData, scatterLayout);
            calculateButton.textContent = "Calculate";
        })
        .catch(error => {
            console.error('Error:', error);
            calculateButton.textContent = "Calculate";
        });
    });
    const toggleDictionaryButton = document.getElementById("toggleDictionary");
    const dictionaryContent = document.getElementById("dictionaryContent");
    // toggle dict
    toggleDictionaryButton.addEventListener("click", function () {
        if (dictionaryContent.style.display === "none" || dictionaryContent.style.display === "") {
            dictionaryContent.style.display = "block";
            toggleDictionaryButton.innerText = "Hide";
        } else {
            dictionaryContent.style.display = "none";
            toggleDictionaryButton.innerText = "Show";
        }
    });


</script>

</body>
