<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Attendance Check-In</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f6f8;
            margin: 0;
            padding: 30px 15px;
            text-align: center;
        }
        .container {
            max-width: 500px;
            margin: auto;
            background: white;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
        h2 { color: #111; margin-top: 0; }
        p { color: #555; font-size: 14px; }
        .hidden { display: none; }
        .form-group {
            text-align: left;
            margin-bottom: 15px;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            font-size: 14px;
        }
        input {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 6px;
            box-sizing: border-box;
            font-size: 15px;
        }
        button {
            width: 100%;
            background-color: #0066cc;
            color: white;
            border: none;
            padding: 12px;
            font-size: 16px;
            border-radius: 6px;
            cursor: pointer;
            margin-top: 10px;
            font-weight: bold;
        }
        button:hover { background-color: #0052a3; }
        .user-card {
            background: #eef6ff;
            border: 1px solid #b6d4fe;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
        }
        .user-card h3 { margin: 0 0 5px 0; color: #004085; }
        .reset-btn {
            background: transparent;
            color: #d9534f;
            border: none;
            text-decoration: underline;
            font-size: 12px;
            cursor: pointer;
            margin-top: 15px;
        }
    </style>
</head>
<body>

<div class="container">

    <!-- FIRST-TIME USERS FORM -->
    <div id="register-section" class="hidden">
        <h2>First-Time Registration</h2>
        <p>Please enter your information once. Next time you scan, this step will be skipped automatically.</p>
        
        <form id="info-form">
            <div class="form-group">
                <label for="fullName">Full Name</label>
                <input type="text" id="fullName" placeholder="e.g. Juan Dela Cruz" required>
            </div>
            <div class="form-group">
                <label for="idNumber">ID / Member Number</label>
                <input type="text" id="idNumber" placeholder="e.g. 2026-1002" required>
            </div>
            <button type="submit">Save & Proceed to Attendance</button>
        </form>
    </div>

    <!-- RETURNING USERS ATTENDANCE BUTTON -->
    <div id="attendance-section" class="hidden">
        <div class="user-card">
            <h3 id="welcome-name">Welcome Back!</h3>
            <p id="welcome-id" style="margin:0; font-size:13px; color:#555;"></p>
        </div>

        <h2>Attendance Check-In</h2>
        <p>Your details are verified. Tap below to log your attendance.</p>

        <button id="mark-attendance-btn" onclick="submitAttendance()">Mark Me Present</button>

        <button class="reset-btn" onclick="resetUser()">Not you? Change registration info</button>
    </div>

</div>

<script>
    const GOOGLE_FORM_URL = "https://docs.google.com/forms/d/e/1FAIpQLSdOz8BN1tiqOR7IBF5nSDYXTkPkLc07U5NSLJrxuhQMBF2IQg/viewform";

    window.onload = function() {
        checkUserStatus();
    };

    function checkUserStatus() {
        const savedName = localStorage.getItem("attendee_name");
        const savedID = localStorage.getItem("attendee_id");

        if (savedName && savedID) {
            document.getElementById("register-section").classList.add("hidden");
            document.getElementById("attendance-section").classList.remove("hidden");
            
            document.getElementById("welcome-name").innerText = "Welcome back, " + savedName + "!";
            document.getElementById("welcome-id").innerText = "ID: " + savedID;
        } else {
            document.getElementById("register-section").classList.remove("hidden");
            document.getElementById("attendance-section").classList.add("hidden");
        }
    }

    document.getElementById("info-form").addEventListener("submit", function(e) {
        e.preventDefault();
        
        const name = document.getElementById("fullName").value;
        const id = document.getElementById("idNumber").value;

        localStorage.setItem("attendee_name", name);
        localStorage.setItem("attendee_id", id);

        checkUserStatus();
    });

    function submitAttendance() {
        window.location.href = GOOGLE_FORM_URL;
    }

    function resetUser() {
        if (confirm("Are you sure you want to clear saved information on this device?")) {
            localStorage.removeItem("attendee_name");
            localStorage.removeItem("attendee_id");
            checkUserStatus();
        }
    }
</script>

</body>
</html>
