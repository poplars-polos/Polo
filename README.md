<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Poplar's Polos</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #1a2e19; /* Dark green */
            color: #f2e6d4; /* Light gold */
        }
        
        header {
            background-color: #1a2e19;
            padding: 10px;
            position: relative;
            text-align: center;
        }

        header h1 {
            color: #f2e6d4;
            font-size: 30px;
            font-family: 'Georgia', serif;
            letter-spacing: 2px;
            margin: 0;
        }

        #menuToggle {
            display: block;
            background-color: #f2e6d4;
            width: 30px;
            height: 25px;
            position: absolute;
            top: 20px;
            left: 15px;
            cursor: pointer;
        }

        #menuToggle span {
            display: block;
            width: 100%;
            height: 4px;
            background-color: #1a2e19;
            margin: 5px 0;
        }

        #menu {
            position: fixed;
            top: 0;
            left: -250px;
            width: 250px;
            height: 100%;
            background-color: #1a2e19;
            color: #f2e6d4;
            padding-top: 60px;
            transition: left 0.3s ease;
        }

        #menu a {
            display: block;
            padding: 20px;
            color: #f2e6d4;
            text-decoration: none;
            font-size: 18px;
            transition: background-color 0.3s;
        }

        #menu a:hover {
            background-color: #3b5c46;
        }

        .menu-open #menu {
            left: 0;
        }

        .product-card {
            display: inline-block;
            width: 100%;
            max-width: 600px;
            padding: 20px;
            background-color: #1a2e19;
            color: #f2e6d4;
            text-align: center;
            margin-bottom: 20px;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }

        .product-card img {
            width: 100%;
            max-width: 300px;
            margin-bottom: 15px;
        }

        .product-card h3 {
            font-size: 24px;
            margin-bottom: 10px;
        }

        .product-card button {
            background-color: #f2e6d4;
            color: #1a2e19;
            padding: 10px 20px;
            border: none;
            cursor: pointer;
            font-size: 18px;
        }

        .product-card button:hover {
            background-color: #3b5c46;
        }

        .basket-container {
            display: none;
            position: fixed;
            top: 0;
            right: 0;
            width: 300px;
            height: 100%;
            background-color: #1a2e19;
            color: #f2e6d4;
            padding: 20px;
            overflow-y: auto;
            box-shadow: -2px 0px 10px rgba(0, 0, 0, 0.3);
        }

        .basket-container.active {
            display: block;
        }

        .basket-item {
            padding: 10px;
            margin-bottom: 15px;
            border-bottom: 1px solid #3b5c46;
        }

        .checkout-button {
            background-color: #f2e6d4;
            color: #1a2e19;
            padding: 15px;
            font-size: 18px;
            cursor: pointer;
            text-align: center;
            width: 100%;
        }

        .sign-in-container, .return-container {
            padding: 20px;
            background-color: #1a2e19;
            color: #f2e6d4;
        }

        .sign-in-container input, .return-container input {
            width: 100%;
            padding: 10px;
            margin-bottom: 15px;
            background-color: #3b5c46;
            border: none;
            color: #f2e6d4;
        }

        .sign-in-container button, .return-container button {
            background-color: #f2e6d4;
            color: #1a2e19;
            padding: 10px 20px;
            border: none;
            cursor: pointer;
            font-size: 18px;
        }

        .sign-in-container button:hover, .return-container button:hover {
            background-color: #3b5c46;
        }
    </style>
</head>
<body>

    <header>
        <h1>Poplar's Polos</h1>
        <div id="menuToggle">
            <span></span>
            <span></span>
            <span></span>
        </div>
    </header>

    <div id="menu">
        <a href="#">Polo Shirts</a>
        <a href="#" id="viewBasketBtn">View Basket</a>
        <a href="#">Contact Us</a>
        <a href="#">Our Story</a>
        <a href="sign-in.html">Sign In</a>
        <a href="return.html">Return</a>
    </div>

    <!-- Products -->
    <div class="product-card">
        <img src="images/polo1.jpg" alt="No. 1 Green Polo">
        <h3>No. 1 Green Polo</h3>
        <p>£50</p>
        <button onclick="addToBasket('No. 1 Green Polo', 50)">Add to Basket</button>
    </div>

    <!-- Basket Section -->
    <div class="basket-container" id="basketContainer">
        <h2>Basket</h2>
        <div id="basketItems"></div>
        <div class="checkout-button" onclick="window.location.href='checkout.html'">Proceed to Checkout</div>
    </div>

    <script>
        let basket = [];

        // Toggle the menu
        document.getElementById('menuToggle').addEventListener('click', function() {
            document.body.classList.toggle('menu-open');
        });

        // Toggle the basket
        document.getElementById('viewBasketBtn').addEventListener('click', function() {
            document.getElementById('basketContainer').classList.toggle('active');
            displayBasket();
        });

        function addToBasket(poloName, price) {
            let item = basket.find(i => i.name === poloName);
            if (item) {
                item.quantity++;
            } else {
                basket.push({ name: poloName, price: price, quantity: 1 });
            }
            displayBasket();
        }

        function displayBasket() {
            let basketItems = document.getElementById('basketItems');
            basketItems.innerHTML = '';

            basket.forEach(item => {
                let basketItem = document.createElement('div');
                basketItem.classList.add('basket-item');
                basketItem.innerHTML = `${item.name} - £${item.price} x ${item.quantity} <button onclick="removeItem('${item.name}')">Remove</button>`;
                basketItems.appendChild(basketItem);
            });
        }

        function removeItem(itemName) {
            basket = basket.filter(item => item.name !== itemName);
            displayBasket();
        }
    </script>

</body>
</html>
