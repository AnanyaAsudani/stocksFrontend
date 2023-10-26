---
permalink: /optimizer
title: Optimizer
---

<head>
    <title>Stock Portfolio Optimization</title>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
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
        #graph{
            width: 100%;
            height: 100%;
        }
        p {
            margin-top: 30px; 
            text-align: center; 
            max-width: 800px; 
            margin-left: auto; 
            margin-right: auto; 
        }
    </style>
</head>
<body>
    <h1>Stock Portfolio Optimization</h1>
    <p>This is a portfolio simulator where you can put up to 8 stocks to find the most optimal weighting between them and see it on a graph. There are also other terms that will be defined below.</p>
    <form id="stock-form">
        <label for="update-input">Enter up to 8 stock tickers by adding one and then clicking the add stock button. When you are done, click calculate:</label>
        <input id="update-input" type="text" style="border: 1px solid black;" placeholder="Enter a ticker(EX: AAPL)">
        <button type="button" id="add-stock">Add stock</button>
        <button type="submit" id="calculate">Calculate</button>
    </form>
    <div id="result" class="latest-data">
    </div>
    <div id="graph" class="latest-data">
    </div>
<div id="dictionary">
    <h2>Dictionary</h2>
    <button id="toggleDictionary">Show</button>
    <div id="dictionaryContent" style="display: none;">
<p><strong>Volatility:</strong> Volatility measures how much a financial asset's price changes over time, indicating the level of risk in a portfolio. Higher volatility means greater price fluctuations.</p>

<p><strong>Return:</strong> Return is the profit or loss from an investment compared to the initial amount invested. It's usually expressed as a percentage of the initial investment.</p>

<p><strong>Optimal Weights:</strong> Optimal weights are the allocation of capital to different assets in a portfolio to achieve a desired risk-return balance. They are the weightings for each asset that maximize the Sharpe Ratio, showing the best trade-off between return and risk.</p>

<p><strong>Sharpe Ratio:</strong> The Sharpe Ratio is a measure that evaluates the risk-adjusted return of a portfolio. It indicates how well an investment has performed given its level of risk. A higher Sharpe Ratio suggests a more favorable balance between risk and return, making it a valuable tool for assessing and comparing investment performance.</p>

<p><strong>What is a good Sharpe Ratio?</strong> A good Sharpe Ratio typically falls in the range of 0.5 to 1.0 or higher. The higher the ratio, the better, as it indicates a higher return for the level of risk taken. However, the definition of "good" may vary based on individual risk preferences and the specific investment objectives.</p>

<p><strong>Graph Explanation:</strong> The graph shows a scatter plot with portfolio return on the y-axis and volatility (risk) on the x-axis. Each point on the graph represents a unique portfolio with different asset weightings. The color of each point indicates the Sharpe Ratio, with darker colors showing a higher ratio.</p>

<p><strong>Red Dot (Most Optimal):</strong> The red dot on the graph represents the most optimal portfolio, which has the highest Sharpe Ratio, indicating the best risk-return trade-off. Investors often aim to construct a portfolio that closely resembles this point for optimal performance.</p>

<p><strong>Most Optimal:</strong> "Most optimal" refers to the portfolio that offers the best risk-adjusted return, typically determined by the Sharpe Ratio. It represents the ideal balance between risk and return and is the goal for portfolio optimization.</p>
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
        })
        .catch(error => {
            console.error('Error:', error);
        });
    });
    const toggleDictionaryButton = document.getElementById("toggleDictionary");
    const dictionaryContent = document.getElementById("dictionaryContent");
    // Toggle dictionary content
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
