
<head>
    <style>
        body {
            text-align: center;
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
            background-color: #f2f2f2;
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
            margin-top: 20px;
        }
        input {
            font-size: 16px;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
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
        #graph-container {
            margin-top: 20px;
            background-color: #fff;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            max-width: 800px;
            margin: 30px auto;
        }
        #graph {
            max-width: 800px;
            margin: 30px auto;
        }
        h3 {
            margin-top: 30px;
        }
        #image-container {
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 20px 0;
        }
        .image-box {
            margin: 0 20px;
            text-align: center;
        }
        img {
            max-width: 100%;
            border: 1px solid #0073e6;
            border-radius: 5px;
        }
    </style>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
</head>
<body>
<h1>Stock Viewer</h1>
<p>Welcome to our Stock Viewer app where you can view the graphs and prices of different stocks using their stock ticker (e.g., AAPL, AMZN, etc.). Enter the desired stock ticker in all caps to generate an interactive graph showing data from the last 12 years.</p>
<label for="stock-input">Enter a stock symbol:</label>
<input id="stock-input" type="text">
<button id="collect" onclick="getStockGraph()">Display Graph</button>
<div id="graph-container">
    <div id="graph"></div>
</div>
<h3>Image Examples:</h3>
<div id="image-container">
    <div class="image-box">
        <img src="https://i.ibb.co/3vqVkLW/SCR-20231025-rrut.png" alt="Image 1">
    </div>
    <div class="image-box">
        <img src="https://i.ibb.co/7zG578m/SCR-20231025-rngg.png" alt="Image 2">
    </div>
</div>
<script>
    function getStockGraph() {
        const selectedStock = document.getElementById('stock-input').value;
        const button = document.getElementById('collect');
        const apiUrl = 'http://localhost:8282/api/stocks/stock_graph/' + selectedStock;
        button.textContent = 'Loading...';
        fetch(apiUrl)
            .then(response => response.json())
            .then(graphData => {
                Plotly.newPlot('graph', graphData.data, graphData.layout);
                button.textContent = 'Display Graph';
            })
            .catch(error => {
                console.error('Error:', error);
                button.textContent = 'Display Graph';
            });
    }
    const stockInput = document.getElementById('stock-input');
    const collectButton = document.getElementById('collect');
    stockInput.addEventListener("keydown", function (event) {
        if (event.key === 'Enter') {
            getStockGraph();
        }
    });
    collectButton.addEventListener("click", getStockGraph);
</script>

</body>
