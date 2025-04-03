<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gowdham Tech - Earn Rewards</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>🎉 Join Gowdham Tech and Earn Rewards! 🎉</h1>
    </header>

    <section>
        <p>🚀 Register now, the all-in-one payment app for recharging mobile, and more!</p>
        <h3>🔗 <a href="http://gowdhamtech.site/signup?referralID=GT1234" target="_blank">Sign Up Now</a></h3>
        <p>🔢 Use my referral code: <strong>GT1234</strong></p>
    </section>

    <footer>
        <p>&copy; 2025 Gowdham Tech. All rights reserved.</p>
    </footer>
</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sign Up - Gowdham Tech</title>
</head>
<body>
    <h2>Sign Up</h2>
    <form id="signupForm">
        <label>Phone Number:</label>
        <input type="tel" id="phone" required>
        <label>Email:</label>
        <input type="email" id="email" required>
        <label>Referral Code:</label>
        <input type="text" id="referralCode" value="GT1234" readonly>
        <button type="submit">Register</button>
    </form>

    <script>
        document.getElementById('signupForm').addEventListener('submit', function(e) {
            e.preventDefault();
            let phone = document.getElementById('phone').value;
            let email = document.getElementById('email').value;
            let referralCode = document.getElementById('referralCode').value;

            console.log("User registered with:", { phone, email, referralCode });
            alert("Registration Successful!");
        });
    </script>
</body>
</html>

<button onclick="makePayment()">Pay Now</button>

<script src="https://checkout.razorpay.com/v1/checkout.js"></script>
<script>
function makePayment() {
    var options = {
        "key": "YOUR_RAZORPAY_KEY",
        "amount": 50000,  // ₹500
        "currency": "INR",
        "name": "Gowdham Tech",
        "description": "Wallet Recharge",
        "handler": function (response) {
            alert("Payment Successful! Payment ID: " + response.razorpay_payment_id);
        },
        "prefill": {
            "email": "user@example.com",
            "contact": "9999999999"
        }
    };
    var rzp = new Razorpay(options);
    rzp.open();
}
</script>

const express = require('express');
const admin = require('firebase-admin');
const cors = require('cors');

const app = express();
app.use(cors());
app.use(express.json());

admin.initializeApp({
    credential: admin.credential.cert(require("./firebase.json")),
    databaseURL: "https://YOUR_FIREBASE_DATABASE.firebaseio.com"
});

const db = admin.firestore();

// API for registering users
app.post('/register', async (req, res) => {
    const { phone, email, referralCode } = req.body;
    
    await db.collection('users').add({ phone, email, referralCode });
    res.json({ message: "User registered successfully!" });
});

// API to get referral count
app.get('/referrals/:code', async (req, res) => {
    const code = req.params.code;
    const users = await db.collection('users').where("referralCode", "==", code).get();
    res.json({ referrals: users.size });
});

app.listen(3000, () => console.log("Server running on port 3000"));
