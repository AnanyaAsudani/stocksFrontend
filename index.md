<head>
    <style>
        body{
            align-items: center;
            text-align: center;
        }
        label {
            display: block;
            margin-bottom: 5px;
            margin-top: 30px;
            align-items: center;
            text-align: center;
        }
        select, button {
            font-size: 16px;
            padding: 10px;
            margin-bottom: 20px;
            color: white;
            align-items: center;
            text-align: center;
        }
        select {
            width: 50%;
            align-items: center;
            text-align: center;
        }
        button {
            cursor: pointer;
            align-items: center;
            text-align: center;
        }
        #graph {
            width: 100%;
        }
    </style>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
</head>
<body>
    <label for="stock-select">Select a stock:</label>
    <select id="stock-select" style = "border: 1px solid black; color: black; align-items: center; text-align: center;">
        <option value="AAPL">AAPL</option>
        <option value="GOOGL">GOOGL</option>
        <option value="AMZN">AMZN</option>
        <option value="META">META</option>
        <option value="TSLA">TSLA</option>
        <option value="SBUX">SBUX</option>
        <option value="ETSY">ETSY</option>
        <option value="EBAY">EBAY</option>
    </select>
    <button id="collect" style="background-color: #0073e6; border: none; border-radius: 5px; transition: background-color 0.3s;" onmouseover="this.style.backgroundColor='#005cbf'" onmouseout="this.style.backgroundColor='#0073e6'" onclick="getStockGraph()">Display Graph</button>
    <div id="graph"></div>
    <script>
        function getStockGraph() {
            const selectedStock = document.getElementById('stock-select').value;
            const apiUrl = 'http://localhost:8282/api/stocks/stock_graph/' + selectedStock;
            fetch(apiUrl)
                .then(response => response.json())
                .then(graphData => {
                    Plotly.newPlot('graph', graphData.data, graphData.layout);
                })
                .catch(error => console.error('Error:', error));
        }
    </script>
</body>
