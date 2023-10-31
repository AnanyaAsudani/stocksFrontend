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
            font-size: 14px;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.2); /* Add a glow effect to text boxes */
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
            font-size: 14px !important; /* Make the text smaller */
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
            background: linear-gradient(-45deg, #e81cff 0%, #40c9ff 100%);
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
        /* Add or modify other styles as needed */
        /* ... */
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
<div class="button-container">
    <button class="button" onclick="window.location.href='{{site.baseurl}}/reader'">Learn More</button>
</div>

<!-- Stock Price Viewer Section -->
<p>
    <strong>Stock Price Viewer:</strong> Shows very concise data, only consisting of the high, low, close, and open of the user-inputted stock from the previous business day.
</p>
<div class="button-container">
    <button class="button" onclick="window.location.href='{{site.baseurl}}/stock_price'">Learn More</button>
</div>

<!-- Stock Analyzer Section -->
<p>
    <strong>Stock Analyzer:</strong> The user is prompted to input a stock, and using our highly skilled AI, patterns and trends from previous years, and various other useful pieces of data, it will give a prediction on whether you should buy the stock.
</p>
<div class="button-container">
    <button class="button" onclick="window.location.href='{{site.baseurl}}/stock_analyze'">Learn More</button>
</div>

<!-- Stock Optimizer Section -->
<p>
    <strong>Stock Optimizer:</strong> This page will prompt the user to input up to 8 stocks as a simulated investment portfolio. It will then give the most optimal weighting of each stock using data such as the correlation, volatility, Sharpe Ratio, and more.
</p>
<div class="button-container">
    <button class="button" onclick="window.location.href='{{site.baseurl}}/optimize'">Learn More</button>
</div>