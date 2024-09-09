<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login & Register</title>
    <style>
        body {
            background-color: #272930;
            color: #c2c6dc;
            font-family: Arial, sans-serif;
        }

        .container {
            width: 400px;
            margin: 50px auto;
            padding: 20px;
            background-color: #313645;
            border-radius: 10px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.5);
        }

        h2 {
            text-align: center;
        }

        label {
            display: block;
            margin-top: 10px;
            margin-bottom: 5px;
        }

        input {
            width: 100%;
            padding: 10px;
            margin-bottom: 10px;
            background-color: #313645;
            border: 1px solid #454b5e;
            color: #c2c6dc;
            border-radius: 5px;
        }

        input[type="checkbox"] {
            width: auto;
        }

        button {
            background-color: #3c3e4f;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 10px;
        }

        button:hover {
            background-color: #4e5366;
        }

        .credits {
            text-align: center;
            font-size: 0.8em;
            margin-top: 20px;
        }

        .hidden {
            display: none;
        }

    </style>
</head>
<body>

<!-- Login Container -->
<div class="container" id="login-container">
    <h2>Login</h2>
    <label for="login-username">Username</label>
    <input type="text" id="login-username" placeholder="Enter your username">

    <label for="login-password">Password</label>
    <input type="password" id="login-password" placeholder="Enter your password">

    <label>
        <input type="checkbox" id="remember-me"> Remember Me
    </label>

    <button onclick="login()">Login</button>
    <button onclick="showRegister()">Register</button>

    <div class="credits">
        Owner: Jacob Sins (Developer) <br>
        Special Thanks: Roberto Denaro
    </div>
</div>

<!-- Register Container -->
<div class="container hidden" id="register-container">
    <h2>Register</h2>

    <label for="reg-username">Username</label>
    <input type="text" id="reg-username" placeholder="Enter your username">

    <label for="reg-password">Password</label>
    <input type="password" id="reg-password" placeholder="Enter your password">

    <label for="reg-repeat-password">Repeat Password</label>
    <input type="password" id="reg-repeat-password" placeholder="Repeat your password">

    <label for="reg-email">Email</label>
    <input type="email" id="reg-email" placeholder="Enter your email">

    <button onclick="register()">Register</button>
    <button onclick="showLogin()">Cancel</button>
</div>

<!-- Post-login Container -->
<div class="container hidden" id="inject-container">
    <h2>Welcome!</h2>

    <!-- License Key Input -->
    <label for="license-key">License Key</label>
    <input type="text" id="license-key" placeholder="Enter your license key">

    <button onclick="validateLicenseKey()">Validate License Key</button>

    <div id="inject-section" class="hidden">
        <h2>Inject Section</h2>
        <button onclick="inject()">Inject</button>
    </div>
</div>

<script>
    // Simulated in-memory storage for credentials
    let userCredentials = {};
    const validLicenseKey = "ABC123XYZ";  // Predefined valid license key

    function showRegister() {
        document.getElementById('login-container').classList.add('hidden');
        document.getElementById('register-container').classList.remove('hidden');
    }

    function showLogin() {
        document.getElementById('register-container').classList.add('hidden');
        document.getElementById('login-container').classList.remove('hidden');
    }

    function isValidEmail(email) {
        const pattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return pattern.test(email);
    }

    function register() {
        const username = document.getElementById('reg-username').value;
        const password = document.getElementById('reg-password').value;
        const repeatPassword = document.getElementById('reg-repeat-password').value;
        const email = document.getElementById('reg-email').value;

        if (password !== repeatPassword) {
            alert("Passwords do not match!");
            return;
        }

        if (!isValidEmail(email)) {
            alert("Invalid email format!");
            return;
        }

        // Save the credentials
        userCredentials[username] = password;
        alert("Registration Successful!");

        // Clear inputs
        document.getElementById('reg-username').value = '';
        document.getElementById('reg-password').value = '';
        document.getElementById('reg-repeat-password').value = '';
        document.getElementById('reg-email').value = '';

        // Show login form
        showLogin();
    }

    function login() {
        const username = document.getElementById('login-username').value;
        const password = document.getElementById('login-password').value;

        if (userCredentials[username] && userCredentials[username] === password) {
            alert("Login Successful!");
            showInjectPage();
        } else {
            alert("Incorrect Username or Password!");
        }
    }

    // Show inject page after successful login
    function showInjectPage() {
        document.getElementById('login-container').classList.add('hidden');
        document.getElementById('inject-container').classList.remove('hidden');
    }

    // Validate the entered license key
    function validateLicenseKey() {
        const enteredKey = document.getElementById('license-key').value;

        if (enteredKey === validLicenseKey) {
            alert("License Key Validated Successfully!");
            document.getElementById('inject-section').classList.remove('hidden');
        } else {
            alert("Invalid License Key!");
        }
    }

    // Action for inject button
    function inject() {
        alert("Inject button clicked!");
    }
</script>

</body>
</html>
