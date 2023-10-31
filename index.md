
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

<h1>Home Page</h1>
<p>Welcome to our Stock Viewer app where you can view the graphs and prices of different stocks using their stock ticker (e.g., AAPL, AMZN, etc.). Enter the desired stock ticker in all caps to generate an interactive graph showing data from the last 12 years.</p>

<!-- Data Reader Section -->
<p>
    <strong>Data Reader:</strong> This allows you to view the graph of the stock, zoom in and out; shows you when you times when it would've been best to invest or when to sell, and much more.
</p>
<button onclick="window.location.href='{{site.baseurl}}/reader'">Learn More</button>

<!-- Stock Price Viewer Section -->
<p>
    <strong>Stock Price Viewer:</strong> Shows very concise data, only consisting of the high, low, close, and open of the user-inputted stock from the previous business day.
</p>
<button onclick="window.location.href='{{site.baseurl}}/stock_price'">Learn More</button>

<!-- Stock Analyzer Section -->
<p>
    <strong>Stock Analyzer:</strong> The user is prompted to input a stock, and using our highly skilled AI, patterns and trends from previous years, and various other useful pieces of data, it will give a prediction on whether you should buy the stock.
</p>
<button onclick="window.location.href='{{site.baseurl}}/stock_analyze'">Learn More</button>

<!-- Stock Optimizer Section -->
<p>
    <strong>Stock Optimizer:</strong> This page will prompt the user to input up to 8 stocks as a simulated investment portfolio. It will then give the most optimal weighting of each stock using data such as the correlation, volatility, Sharpe Ratio, and more.
</p>
<button onclick="window.location.href='{{site.baseurl}}/optimizer'">Learn More</button>
</body>