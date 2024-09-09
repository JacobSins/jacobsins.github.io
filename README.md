<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login Panel</title>
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
</div>

<!-- Key Generation Container -->
<div class="container hidden" id="keygen-container">
    <h2>Generate Key</h2>
    <label for="key-duration">Select Key Duration</label>
    <select id="key-duration">
        <option value="1d">1 Day</option>
        <option value="1w">1 Week</option>
        <option value="1m">1 Month</option>
        <option value="lifetime">Lifetime</option>
    </select>
    <button onclick="generateKey()">Generate Key</button>
    <p id="generated-key"></p>
</div>

<!-- Post-login Container -->
<div class="container hidden" id="admin-container">
    <h2>Welcome!</h2>
    <div id="admin-section">
        <h2>Admin Panel</h2>
        <button onclick="performAction()">Perform Action</button>
        <button onclick="showKeygen()">Generate Key</button>
    </div>
</div>

<script>
    // Simulated in-memory storage for credentials and keys
    let userCredentials = { "admin": "password123" };  // Example credentials
    let validKeys = {};

    function login() {
        const username = document.getElementById('login-username').value;
        const password = document.getElementById('login-password').value;

        if (userCredentials[username] && userCredentials[username] === password) {
            alert("Login Successful!");
            showAdminPanel();
            sendNotification("Login Successful", "You have successfully logged in.");
        } else {
            alert("Incorrect Username or Password!");
        }
    }

    // Show admin panel after successful login
    function showAdminPanel() {
        document.getElementById('login-container').classList.add('hidden');
        document.getElementById('admin-container').classList.remove('hidden');
    }

    // Show key generation panel
    function showKeygen() {
        document.getElementById('admin-container').classList.add('hidden');
        document.getElementById('keygen-container').classList.remove('hidden');
    }

    // Generate a key based on selected duration
    function generateKey() {
        const duration = document.getElementById('key-duration').value;
        const key = generateRandomKey();
        const expiryDate = calculateExpiryDate(duration);
        validKeys[key] = expiryDate;

        document.getElementById('generated-key').textContent = `Generated Key: ${key} (Expires: ${expiryDate.toDateString()})`;
        sendNotification("Key Generated", `Generated Key: ${key} (Expires: ${expiryDate.toDateString()})`);
    }

    // Generate a random key
    function generateRandomKey() {
        return 'KEY-' + Math.random().toString(36).substr(2, 9).toUpperCase();
    }

    // Calculate expiry date based on duration
    function calculateExpiryDate(duration) {
        const now = new Date();
        switch (duration) {
            case '1d':
                now.setDate(now.getDate() + 1);
                break;
            case '1w':
                now.setDate(now.getDate() + 7);
                break;
            case '1m':
                now.setMonth(now.getMonth() + 1);
                break;
            case 'lifetime':
                // Lifetime doesn't have an expiry date
                return 'Never';
            default:
                return now;
        }
        return now;
    }

    // Action for admin button
    function performAction() {
        alert("Action performed!");
        sendNotification("Action Performed", "The action was successfully performed.");
    }

    // Function to send browser notifications
    function sendNotification(title, body) {
        if (Notification.permission === 'granted') {
            new Notification(title, { body });
        } else if (Notification.permission !== 'denied') {
            Notification.requestPermission().then(permission => {
                if (permission === 'granted') {
                    new Notification(title, { body });
                }
            });
        }
    }

    // Request permission for notifications on page load
    window.onload = () => {
        if (Notification.permission === 'default') {
            Notification.requestPermission();
        }
    };
</script>

</body>
</html>
