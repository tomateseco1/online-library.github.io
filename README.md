# online-library.github.io
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Online Library</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #fff;
            color: #000;
            margin: 0;
            padding: 0;
        }
        header {
            background-color: #000;
            color: #fff;
            padding: 10px 0;
            text-align: center;
        }
        header h1 {
            margin: 0;
        }
        nav {
            display: flex;
            justify-content: center;
            background-color: #fff;
            border-bottom: 1px solid #000;
        }
        nav a {
            padding: 15px 20px;
            text-decoration: none;
            color: #000;
            font-weight: bold;
        }
        nav a:hover {
            background-color: #000;
            color: #fff;
        }
        section {
            padding: 20px;
        }
        .genres {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            justify-content: center;
        }
        .genre {
            flex: 1 1 200px;
            border: 1px solid #000;
            padding: 10px;
            text-align: center;
        }
        footer {
            background-color: #000;
            color: #fff;
            text-align: center;
            padding: 10px 0;
            position: fixed;
            width: 100%;
            bottom: 0;
        }
        .form-container {
            max-width: 400px;
            margin: 0 auto;
            padding: 20px;
            border: 1px solid #000;
        }
        .form-container input {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #000;
        }
        .form-container button {
            background-color: #000;
            color: #fff;
            padding: 10px 20px;
            border: none;
            cursor: pointer;
        }
        .form-container button:hover {
            background-color: #555;
        }
    </style>
    <!-- Add Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/8.6.8/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.6.8/firebase-auth.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.6.8/firebase-firestore.js"></script>
    <!-- Add PayPal SDK -->
    <script src="https://www.paypal.com/sdk/js?client-id=EMLtollSduZWfQp27IXmqkTamEP5A7-uwt04yUn1rmpSzFy6nRZzdShU84XO5whVAJliyuxyCUsanc6p&currency=USD"></script>
    <script>
        // Your Firebase configuration
        var firebaseConfig = {
            apiKey: "AIzaSyB4UDUSq_Kk2i5XeiDshSxqNy0XMWsiJp8",
            authDomain: "online-library-d2f12.firebaseapp.com",
            projectId: "online-library-d2f12"
        };
        // Initialize Firebase
        firebase.initializeApp(firebaseConfig);
        var auth = firebase.auth();
        var db = firebase.firestore();

        // Register function
        function register() {
            var email = document.getElementById('registerEmail').value;
            var password = document.getElementById('registerPassword').value;
            auth.createUserWithEmailAndPassword(email, password)
                .then((userCredential) => {
                    var user = userCredential.user;
                    alert('Successfully registered: ' + user.email);
                    db.collection("users").doc(user.uid).set({
                        email: user.email
                    })
                    .then(() => {
                        console.log("User registered in Firestore.");
                    })
                    .catch((error) => {
                        console.error("Error saving to Firestore: ", error);
                    });
                })
                .catch((error) => {
                    var errorCode = error.code;
                    var errorMessage = error.message;
                    alert('Error: ' + errorMessage);
                });
        }

        // Login function
        function login() {
            var email = document.getElementById('email').value;
            var password = document.getElementById('password').value;
            auth.signInWithEmailAndPassword(email, password)
                .then((userCredential) => {
                    var user = userCredential.user;
                    alert('Successfully logged in: ' + user.email);
                })
                .catch((error) => {
                    var errorCode = error.code;
                    var errorMessage = error.message;
                    alert('Error: ' + errorMessage);
                });
        }

        // PayPal button rendering function
        function renderPaypalButton() {
            paypal.Buttons({
                createOrder: function(data, actions) {
                    return actions.order.create({
                        purchase_units: [{
                            amount: {
                                value: '5.00'
                            }
                        }]
                    });
                },
                onApprove: function(data, actions) {
                    return actions.order.capture().then(function(details) {
                        alert('Transaction completed by ' + details.payer.name.given_name);
                        // Here you can add additional logic to save the transaction in your database
                    });
                }
            }).render('#paypal-button-container');
        }

        document.addEventListener('DOMContentLoaded', function() {
            renderPaypalButton();
        });
    </script>
</head>
<body>
    <header>
        <h1>Online Library</h1>
    </header>
    <nav>
        <a href="#action">Action</a>
        <a href="#fantasy">Fantasy</a>
        <a href="#romance">Romance</a>
        <a href="#nonfiction">Non-Fiction</a>
        <!-- Add more genres as needed -->
    </nav>
    <section id="action">
        <h2>Action</h2>
        <div class="genres">
            <div class="genre">Book 1</div>
            <div class="genre">Book 2</div>
            <!-- Add more books as needed -->
        </div>
    </section>
    <section id="fantasy">
        <h2>Fantasy</h2>
        <div class="genres">
            <div class="genre">Book 1</div>
            <div class="genre">Book 2</div>
            <!-- Add more books as needed -->
        </div>
    </section>
    <section id="romance">
        <h2>Romance</h2>
        <div class="genres">
            <div class="genre">Book 1</div>
            <div class="genre">Book 2</div>
            <!-- Add more books as needed -->
        </div>
    </section>
    <section id="nonfiction">
        <h2>Non-Fiction</h2>
        <div class="genres">
            <div class="genre">Book 1</div>
            <div class="genre">Book 2</div>
            <!-- Add more books as needed -->
        </div>
    </section>
    <section>
        <h2>Login</h2>
        <div class="form-container">
            <form id="loginForm" onsubmit="event.preventDefault(); login();">
                <input type="email" id="email" placeholder="Email" required>
                <input type="password" id="password" placeholder="Password" required>
                <button type="submit">Login</button>
            </form>
        </div>
    </section>
    <section>
        <h2>Register</h2>
        <div class="form-container">
            <form id="registerForm" onsubmit="event.preventDefault(); register();">
                <input type="email" id="registerEmail" placeholder="Email" required>
                <input type="password" id="registerPassword" placeholder="Password" required>
                <button type="submit">Register</button>
            </form>
        </div>
    </section>
    <section>
        <h2>Monthly Plan</h2>
        <p>Subscribe for just $5 per month to access all books.</p>
        <div id="paypal-button-container"></div>
    </section>
    <footer>
        <p>&copy; 2024 Online Library. All rights reserved.</p>
    </footer>
</body>
</html>
