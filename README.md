<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>IT Helpdesk Support</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      font-family: Arial, sans-serif;
      background: linear-gradient(to bottom, black, #222);
      color: gold;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
    }

    header, footer {
      text-align: center;
      padding: 20px;
    }

    main {
      width: 90%;
      max-width: 600px;
      background: rgba(255, 255, 255, 0.1);
      backdrop-filter: blur(10px);
      border-radius: 15px;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.5);
      padding: 20px;
      margin: 20px 0;
    }

    h1 {
      margin: 0 0 20px;
      color: gold;
      text-align: center;
    }

    .ticket-info {
      background: rgba(0, 0, 0, 0.3);
      padding: 15px;
      border-radius: 10px;
      margin-bottom: 20px;
      text-align: center;
      border: 1px solid gold;
    }

    .ticket-note {
      font-size: 0.9em;
      margin-top: 10px;
      padding-top: 10px;
      border-top: 1px solid rgba(255, 215, 0, 0.3);
    }

    label {
      display: block;
      margin-bottom: 10px;
      font-weight: bold;
    }

    input, select, textarea, button {
      width: 100%;
      padding: 12px;
      margin-bottom: 15px;
      border: none;
      border-radius: 10px;
      outline: none;
      font-size: 16px;
      background: rgba(255, 255, 255, 0.2);
      color: gold;
    }

    textarea {
      min-height: 100px;
      resize: vertical;
    }

    select option {
      background: #222;
      color: gold;
    }

    button {
      background: linear-gradient(to bottom, gold, #cc9900);
      color: black;
      font-weight: bold;
      cursor: pointer;
      box-shadow: 0 5px #995c00;
      transition: transform 0.2s, box-shadow 0.2s;
    }

    button:hover {
      background: linear-gradient(to bottom, #cc9900, gold);
    }

    button:active {
      transform: translateY(2px);
      box-shadow: 0 3px #995c00;
    }

    .severity-high {
      background: rgba(255, 0, 0, 0.2);
    }

    .severity-medium {
      background: rgba(255, 165, 0, 0.2);
    }

    .severity-low {
      background: rgba(0, 255, 0, 0.2);
    }

    .confirmation-popup {
      display: none;
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 80%;
      max-width: 400px;
      background: rgba(0, 0, 0, 0.9);
      border-radius: 15px;
      padding: 20px;
      box-shadow: 0 5px 20px rgba(0, 0, 0, 0.8);
      color: gold;
      text-align: center;
      z-index: 1000;
    }

    .popup-buttons {
      display: flex;
      justify-content: space-around;
      margin-top: 20px;
    }

    .popup-buttons button {
      width: 45%;
      padding: 10px;
    }

    .popup-yes {
      background: gold;
      color: black;
    }

    .popup-no {
      background: black;
      color: gold;
      border: 2px solid gold;
    }

    .technician-info {
      font-size: 0.9em;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <header>
    <h1>IT Helpdesk Support</h1>
  </header>

  <main>
    <div class="ticket-info">
      <div>Ticket #: <span id="ticketId">Loading...</span></div>
      <div class="ticket-note">Please note: Support requests remain open for 3 days</div>
    </div>

    <form id="helpDeskForm">
      <label for="name">Full Name:</label>
      <input type="text" id="name" name="name" placeholder="Enter your full name" required>

      <label for="email">Email:</label>
      <input type="email" id="email" name="email" placeholder="Enter your email" required>

      <label for="department">Department:</label>
      <select id="department" name="department" required>
        <option value="" disabled selected>Select Department</option>
        <option value="Finance">Finance</option>
        <option value="HR">Human Resources</option>
        <option value="IT">Information Technology</option>
        <option value="Operations">Operations</option>
        <option value="Sales">Sales</option>
      </select>

      <label for="severity">Severity Level:</label>
      <select id="severity" name="severity" required onchange="updateSeverityStyle()">
        <option value="" disabled selected>Select Severity</option>
        <option value="High">High - System Down/Critical Issue</option>
        <option value="Medium">Medium - Significant Impact</option>
        <option value="Low">Low - Minor Issue</option>
      </select>

      <label for="issue">Describe Your Issue:</label>
      <textarea id="issue" name="issue" placeholder="Please provide details about your issue..." required></textarea>

      <button type="button" onclick="confirmSubmission()">Submit Ticket</button>
    </form>
  </main>

  <div class="confirmation-popup" id="confirmationPopup">
    <p>Are you sure you want to submit this ticket?</p>
    <div class="popup-buttons">
      <button class="popup-yes" onclick="submitForm()">Yes</button>
      <button class="popup-no" onclick="closePopup()">No</button>
    </div>
  </div>

  <footer>
    <p>© 2025 IT Helpdesk Support. All rights reserved.</p>
    <div class="technician-info">
      IT Technician & Software Developer: Melato Tatu
    </div>
  </footer>

  <script>
    // Generate Ticket Number
    function generateTicketNumber() {
      const date = new Date();
      const year = date.getFullYear().toString().slice(-2);
      const month = (date.getMonth() + 1).toString().padStart(2, '0');
      const day = date.getDate().toString().padStart(2, '0');
      const random = Math.floor(Math.random() * 1000).toString().padStart(3, '0');
      return `IT${year}${month}${day}-${random}`;
    }

    // Set initial ticket number
    document.getElementById('ticketId').textContent = generateTicketNumber();

    // Update severity style
    function updateSeverityStyle() {
      const severity = document.getElementById('severity').value;
      const textarea = document.getElementById('issue');
      
      textarea.classList.remove('severity-high', 'severity-medium', 'severity-low');
      
      if (severity === 'High') {
        textarea.classList.add('severity-high');
      } else if (severity === 'Medium') {
        textarea.classList.add('severity-medium');
      } else if (severity === 'Low') {
        textarea.classList.add('severity-low');
      }
    }

    // Confirmation Popup
    function confirmSubmission() {
      document.getElementById("confirmationPopup").style.display = "block";
    }

    function closePopup() {
      document.getElementById("confirmationPopup").style.display = "none";
    }

    function submitForm() {
      const ticketId = document.getElementById('ticketId').textContent;
      const name = document.getElementById('name').value;
      const email = document.getElementById('email').value;
      const department = document.getElementById('department').value;
      const severity = document.getElementById('severity').value;
      const issue = document.getElementById('issue').value;

      const message = `New IT Support Ticket\n
Ticket #: ${ticketId}
Name: ${name}
Email: ${email}
Department: ${department}
Severity: ${severity}
Issue: ${issue}\n
This ticket has been logged and will be attended to according to severity level.
Note: Support requests remain open for 3 days.`;

      // Redirect to WhatsApp
      window.location.href = `https://wa.me/27732296540?text=${encodeURIComponent(message)}`;
      
      closePopup();
    }
  </script>
</body>
</html>
