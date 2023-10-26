<head>
    <style>
        body {
            text-align: center;
            margin: 0;
            padding: 0;
        }
        label {
            display: block;
            margin-top: 40px;
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
        /* #graph-container {
            display: flex;
            justify-content: center;
            align-items: center;
        } */
        #image-container{
            display: flex;
            justify-content: center;
            align-items: center;
        }
        #graph {
            width: 100%;
        }
        p {
            margin-top: 30px; 
            text-align: center; 
            max-width: 800px; 
            margin-left: auto; 
            margin-right: auto; 
        }
        h3{
            margin-top: 100px;
        }
        .definitions-container {
            margin-top: 30px;
        }
        .definitions {
            display: none;
        }
    </style>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
</head>
<body>
<h1>Stock Viewer</h1>
    <p>Welcome to our Stock Viewer app where you can view the graphs and prices of different stocks using just their stock ticker (e.g., AAPL, AMZN, etc.). Start your journey by entering the desired stock ticker in all caps to generate a fully interactive graph. The graph shows data from the last 12 or so yeears and can show trends in the stock price so you know if you should buy now or not.</p>
    <label for="stock-input">Enter a stock symbol:</label>
    <input id="stock-input" type="text" style="border: 1px solid black;">
    <button id="collect" onclick="getStockGraph()">Display Graph</button>
    <div id="graph-container">
        <div id="graph"></div>
    </div>
    <h3 style="margin-top:100px;"> Image Examples: </h3>
    <div id="image-container">
    <img src="https://i.ibb.co/3vqVkLW/SCR-20231025-rrut.png" alt="Image 1" style="margin-right:70px; margin-top:20px">
        <img src="https://i.ibb.co/7zG578m/SCR-20231025-rngg.png" alt="Image 2" style="margin-right:10px; margin-top:20px;">
        </div>
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

