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
        .completed-box {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
            padding: 20px;
            border-radius: 8px;
        }
        .completed-box h3 { margin-top: 0; }
    </style>
</head>
<body>

<div class="container">

    <!-- VIEW 1: FIRST-TIME REGISTRATION -->
    <div id="register-section" class="hidden">
        <h2>First-Time Registration</h2>
        <p>Please enter your information once to register your device.</p>
        
        <form id="info-form">
            <div class="form-group">
                <label for="fullName">Full Name</label>
                <input type="text" id="fullName" placeholder="e.g. Maria Santos" required>
            </div>
            <div class="form-group">
                <label for="idNumber">ID / Member Number</label>
                <input type="text" id="idNumber" placeholder="e.g. 2026-8899" required>
            </div>
            <button type="submit">Save & Proceed to Attendance</button>
        </form>
    </div>

    <!-- VIEW 2: ONE-TIME ATTENDANCE SUBMISSION -->
    <div id="attendance-section" class="hidden">
        <div class="user-card">
            <h3 id="welcome-name">Welcome!</h3>
            <p id="welcome-id" style="margin:0; font-size:13px; color:#555;"></p>
        </div>

        <h2>Attendance Check-In</h2>
        <p>Tap below to log your attendance for today.</p>

        <button id="mark-attendance-btn" onclick="submitAttendance()">Mark Me Present</button>
    </div>

    <!-- VIEW 3: LOCKED / COMPLETED STATE -->
    <div id="completed-section" class="hidden">
        <div class="completed-box">
            <h3>✓ Attendance Already Submitted</h3>
            <p id="completed-details"></p>
            <p>You have already checked in. Multiple submissions are not allowed on this device.</p>
        </div>
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
        const submitted = localStorage.getItem("attendance_submitted");

        // IF ALREADY SUBMITTED: Hide forms and show locked screen
        if (submitted === "true") {
            document.getElementById("register-section").classList.add("hidden");
            document.getElementById("attendance-section").classList.add("hidden");
            document.getElementById("completed-section").classList.remove("hidden");
            document.getElementById("completed-details").innerText = "Attendee: " + savedName + " (" + savedID + ")";
            return;
        }

        // IF REGISTERED BUT NOT YET SUBMITTED: Show Mark Present screen
        if (savedName && savedID) {
            document.getElementById("register-section").classList.add("hidden");
            document.getElementById("attendance-section").classList.remove("hidden");
            document.getElementById("completed-section").classList.add("hidden");
            
            document.getElementById("welcome-name").innerText = "Welcome, " + savedName + "!";
            document.getElementById("welcome-id").innerText = "ID: " + savedID;
        } else {
            // NEW USER: Show Registration Form
            document.getElementById("register-section").classList.remove("hidden");
            document.getElementById("attendance-section").classList.add("hidden");
            document.getElementById("completed-section").classList.add("hidden");
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
        // Lock this device permanently
        localStorage.setItem("attendance_submitted", "true");
        
        // Redirect to Google Form or update view
        window.location.href = GOOGLE_FORM_URL;
    }
</script>

</body>
</html>
