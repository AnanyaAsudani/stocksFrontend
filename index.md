<!DOCTYPE html>
<html>
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

        #graph {
            width: 100%;
        }
    </style>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
</head>
<body>
    <label for="stock-input">Enter a stock symbol:</label>
    <input id="stock-input" type="text" style="border: 1px solid black;">
    <button id="collect" onclick="getStockGraph()">Display Graph</button>
    <div id="graph"></div>
    <script>
        function getStockGraph() {
            const selectedStock = document.getElementById('stock-input').value;
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
</html>
