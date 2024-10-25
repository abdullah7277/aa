<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shift Scheduler</title>
    <style>
        /* Background gradient for the whole page */
        body {
            font-family: 'Poppins', sans-serif; /* Use a modern font */
            background: gainsboro; /* Cool gradient */
            text-align: center;
            margin: 0;
            padding: 0;
            color: #333; /* Darker text color */
            direction: ltr; /* Set direction to left-to-right */
            display: flex;
            justify-content: center;
            align-items: flex-start;
        }

        .container {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            width: 80%;
            margin: 20px auto;
        }

        .header {
            margin: 20px;
            font-size: 2em;
            font-weight: bold;
            color: white;
        }

        .calendar-container {
            flex: 3;
            margin-left: 20px;
        }

        .shift-selection-container {
            flex: 1;
            background-color: #fff;
            padding: 10px;
            box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
            border-radius: 10px;
            margin-right: 20px;
            display: grid;
            grid-template-columns: repeat(3, 1fr); /* 3 columns for shift boxes */
            gap: 10px; /* Space between shift boxes */
        }

        .shift-selection-box {
            background-color: #f0f0f0;
            padding: 10px;
            height: 70px; /* Set height to match calendar cells */
            border-radius: 10px;
            box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
            cursor: pointer;
            transition: background-color 0.3s ease;
            font-size: 0.9em;
            display: flex; /* Use flexbox to center the content */
            justify-content: center; /* Center horizontally */
            align-items: center; /* Center vertically */
        }

        .shift-selection-box:hover {
            background-color: #e0e0e0;
        }

        .calendar-navigation {
            display: flex;
            justify-content: center;
            align-items: center;
            margin-bottom: 20px;
        }

        .calendar-navigation button {
            padding: 10px;
            margin: 5px;
            cursor: pointer;
            background-color: #3498db; /* Blue buttons */
            color: white;
            border: none;
            border-radius: 8px;
            transition: background-color 0.3s ease;
            box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1); /* Button shadow */
        }

        .calendar-navigation button:hover {
            background-color: #2980b9; /* Darker blue on hover */
        }

        .calendar-navigation span {
            color: white;
            font-size: 1.2em;
            font-weight: bold;
            margin: 0 10px;
            cursor: pointer;
        }

        /* Enhanced calendar grid styling */
        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr); /* 7 columns */
            gap: 10px;
            justify-content: center;
            margin-top: 10px;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
        }

        /* Styling for day and date cells */
        .calendar-cell, .day-cell {
            width: 70px;
            height: 70px;
            text-align: center;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background-color: white;
            color: #2c3e50; /* Dark text for readability */
            border-radius: 10px; /* Rounded corners */
            box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1); /* Subtle shadow */
            transition: transform 0.2s ease;
        }

        /* Style for the current day */
        .calendar-cell.current-day {
            border: 4px solid red; /* Change border color to red */
            font-weight: bold; /* Make text bolder */
        }

        /* Shift label styling for the current day */
        .calendar-cell.current-day .shift-label {
            color: inherit; /* Keep original color */
        }

        /* Add hover effect for calendar cells */
        .calendar-cell:hover {
            transform: scale(1.05); /* Slight zoom on hover */
        }

        /* Shift label below the date */
        .calendar-cell .shift-label {
            font-size: 12px;
            margin-top: 5px;
        }

        /* Night shift styles */
        .night-shift {
            background-color: white; /* White background for night shift */
            color: white; /* White text for night shift */
        }

        .night-shift .shift-label {
            color: white; /* White text for night shift label */
        }

        /* Day cell styling */
        .day-cell {
            font-size: 14px;
            font-weight: bold;
            background-color: beige;
        }

        /* Button styling for reset */
        button#resetButton {
            padding: 10px 20px;
            font-size: 1em;
            margin-top: 20px;
            background-color: #e74c3c; /* Red reset button */
            color: white;
            border-radius: 8px;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s ease;
            box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1); /* Button shadow */
        }

        button#resetButton:hover {
            background-color: #c0392b; /* Darker red on hover */
        }

        /* Modal styling */
        .modal {
            display: none; /* Hidden by default */
            position: fixed;
            z-index: 1;
            padding-top: 100px;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0, 0, 0, 0.4);
        }

        .modal-content {
            background-color: #fff;
            margin: auto;
            padding: 20px;
            border: 1px solid #888;
            width: 300px;
            border-radius: 10px;
        }

        .close {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
        }

        .close:hover,
        .close:focus {
            color: black;
            text-decoration: none;
            cursor: pointer;
        }

        /* Scrollable select boxes */
        select {
            padding: 10px;
            margin-top: 10px;
            width: 100%;
            max-height: 150px; /* Limit dropdown height */
            overflow-y: auto; /* Scroll if necessary */
        }

    </style>
</head>
<body>

    <div class="container">
        <!-- Shift Selection Box -->
        <div class="shift-selection-container">
            <div class="shift-selection-box" id="shift1">
                <h3>الفئة الرابعة</h3>
            </div>
            <div class="shift-selection-box" id="shift2">
                <h3>الفئة الثالثة</h3>
            </div>
            <div class="shift-selection-box" id="shift3">
                <h3>الفئة الخامسة</h3>
            </div>
            <div class="shift-selection-box" id="shift4">
                <h3>تزويد صباح ١</h3>
            </div>
            <div class="shift-selection-box" id="shift5">
                <h3>تزويد صباح ٢</h3>
            </div>
            <div class="shift-selection-box" id="shift6">
                <h3>تزويد ليل ١</h3> <!-- Added night shift -->
            </div>
            <div class="shift-selection-box" id="shift7">
                <h3>تزويد ليل ٢</h3> <!-- Added night shift -->
            </div>
        </div>

        <!-- Calendar and navigation -->
        <div class="calendar-container">
            <div class="header">
                Shift Schedule
            </div>

            <div class="calendar-navigation">
                <button id="prevMonthButton">«</button>
                <span id="monthYearLabel"></span>
                <button id="nextMonthButton">»</button>
            </div>

            <button id="resetButton">Reset</button>

            <!-- Calendar Grid -->
            <div class="calendar-grid" id="calendarGrid"></div>
        </div>
    </div>

    <!-- Modal for selecting month and year -->
    <div id="monthYearModal" class="modal">
        <div class="modal-content">
            <span class="close">&times;</span>
            <h3>Change Month and Year</h3>
            <label for="monthSelect">Select Month:</label>
            <select id="monthSelect">
                <option value="0">January</option>
                <option value="1">February</option>
                <option value="2">March</option>
                <option value="3">April</option>
                <option value="4">May</option>
                <option value="5">June</option>
                <option value="6">July</option>
                <option value="7">August</option>
                <option value="8">September</option>
                <option value="9">October</option>
                <option value="10">November</option>
                <option value="11">December</option>
            </select>
            <label for="yearSelect">Select Year:</label>
            <select id="yearSelect"></select>
            <button id="saveDateButton">Save</button>
        </div>
    </div>

    <script>
        // Reference dates for employees
        const referenceDates = {
            "الفئة الرابعة": new Date(2010, 0, 26),
            "تزويد صباح ١": new Date(2010, 9, 26), 
            "تزويد صباح ٢": new Date(2010, 9, 23),
            "الفئة الثالثة": new Date(2010, 0, 28),
            "الفئة الخامسة": new Date(2010, 0, 24),
            "تزويد ليل ١": new Date(2010, 10, 1), // New shift
            "تزويد ليل ٢": new Date(2010, 10, 4)  // New shift
        };

        const months = [
            "January", "February", "March", "April", "May", "June",
            "July", "August", "September", "October", "November", "December"
        ];

        const daysOfWeek = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"];

        let currentMonth = new Date().getMonth();
        let currentYear = new Date().getFullYear();
        let currentEmployee = "الفئة الرابعة";

        const today = new Date(); // Current day for red box

        // Get DOM elements
        const monthYearLabel = document.getElementById("monthYearLabel");
        const prevMonthButton = document.getElementById("prevMonthButton");
        const nextMonthButton = document.getElementById("nextMonthButton");
        const resetButton = document.getElementById("resetButton");
        const calendarGrid = document.getElementById("calendarGrid");
        const monthYearModal = document.getElementById("monthYearModal");
        const monthSelect = document.getElementById("monthSelect");
        const yearSelect = document.getElementById("yearSelect");
        const saveDateButton = document.getElementById("saveDateButton");
        const closeSpan = document.getElementsByClassName("close")[0];

        // Populate yearSelect with options from 2010 to 2099
        for (let i = 2010; i <= 2099; i++) {
            const option = document.createElement("option");
            option.value = i;
            option.text = i;
            yearSelect.appendChild(option);
        }

        // Modal open and close functionality
        monthYearLabel.addEventListener("click", () => {
            monthSelect.value = currentMonth;
            yearSelect.value = currentYear;
            monthYearModal.style.display = "block";
        });

        closeSpan.onclick = function () {
            monthYearModal.style.display = "none";
        };

        window.onclick = function (event) {
            if (event.target == monthYearModal) {
                monthYearModal.style.display = "none";
            }
        };

        // Save the selected month and year
        saveDateButton.addEventListener("click", function () {
            currentMonth = parseInt(monthSelect.value);
            currentYear = parseInt(yearSelect.value);
            updateCalendar();
            monthYearModal.style.display = "none";
        });

        // Shift buttons
        const shift1 = document.getElementById("shift1");
        const shift2 = document.getElementById("shift2");
        const shift3 = document.getElementById("shift3");
        const shift4 = document.getElementById("shift4");
        const shift5 = document.getElementById("shift5");
        const shift6 = document.getElementById("shift6"); // New shift
        const shift7 = document.getElementById("shift7"); // New shift

        // Event listeners for shift selection
        shift1.addEventListener("click", function() {
            currentEmployee = "الفئة الرابعة";
            updateCalendar();
        });

        shift2.addEventListener("click", function() {
            currentEmployee = "الفئة الثالثة";
            updateCalendar();
        });

        shift3.addEventListener("click", function() {
            currentEmployee = "الفئة الخامسة";
            updateCalendar();
        });

        shift4.addEventListener("click", function() {
            currentEmployee = "تزويد صباح ١";
            updateCalendar();
        });

        shift5.addEventListener("click", function() {
            currentEmployee = "تزويد صباح ٢";
            updateCalendar();
        });

        shift6.addEventListener("click", function() {
            currentEmployee = "تزويد ليل ١"; // New shift
            updateCalendar();
        });

        shift7.addEventListener("click", function() {
            currentEmployee = "تزويد ليل ٢"; // New shift
            updateCalendar();
        });

        // Function to move to the previous month
        function previousMonth() {
            if (currentMonth === 0) {
                currentMonth = 11;
                currentYear -= 1;
            } else {
                currentMonth -= 1;
            }
            updateCalendar();
        }

        // Function to move to the next month
        function nextMonth() {
            if (currentMonth === 11) {
                currentMonth = 0;
                currentYear += 1;
            } else {
                currentMonth += 1;
            }
            updateCalendar();
        }

        // Function to reset to the current date
        function resetToCurrentDate() {
            currentMonth = today.getMonth();
            currentYear = today.getFullYear();
            updateCalendar();
        }

        // Helper function to get the shift color for a date
        function getShiftColor(date) {
            const referenceDate = referenceDates[currentEmployee];
            const daysBetween = Math.floor((date - referenceDate) / (1000 * 60 * 60 * 24));

            if (currentEmployee === "الفئة الرابعة" || currentEmployee === "الفئة الثالثة" || currentEmployee === "الفئة الخامسة") {
                const cycleDay = daysBetween % 10;
                if (cycleDay === 0 || cycleDay === 1)
                    return "#ADD8E6"; // Morning shift
                if (cycleDay === 2 || cycleDay === 3)
                    return "#FFFF99"; // Afternoon shift
                if (cycleDay === 4 || cycleDay === 5)
                    return "#818181"; // Night shift
                return "#FFFFFF"; // Off days
            } else if (currentEmployee === "تزويد ليل ١" || currentEmployee === "تزويد ليل ٢") {
                const cycleDay = daysBetween % 6; // 3 night shifts + 3 off days = 6-day cycle
                if (cycleDay === 0 || cycleDay === 1 || cycleDay === 2)
                    return "#818181"; // Night shift
                return "#FFFFFF"; // Off days
            } else {
                const cycleDay = daysBetween % 6; // 6-day cycle for تزويد صباح 1 and تزويد صباح 2
                if (cycleDay === 0 || cycleDay === 1 || cycleDay === 2)
                    return "#ADD8E6"; // Morning shift
                return "#FFFFFF"; // Off days
            }
        }

        // Helper function to get the shift label and symbol for a date
        function getShiftLabel(date) {
            const referenceDate = referenceDates[currentEmployee];
            const daysBetween = Math.floor((date - referenceDate) / (1000 * 60 * 60 * 24));

            if (currentEmployee === "الفئة الرابعة" || currentEmployee === "الفئة الثالثة" || currentEmployee === "الفئة الخامسة") {
                const cycleDay = daysBetween % 10;
                if (cycleDay === 0 || cycleDay === 1)
                    return "AM ⛅️"; // Morning shift with ⛅️
                if (cycleDay === 2 || cycleDay === 3)
                    return "PM ☀️"; // Afternoon shift with ☀️
                if (cycleDay === 4 || cycleDay === 5)
                    return "Night 🌙"; // Night shift with 🌙
                return "OFF";
            } else if (currentEmployee === "تزويد ليل ١" || currentEmployee === "تزويد ليل ٢") {
                const cycleDay = daysBetween % 6; // 6-day cycle for تزويد ليل 1 and تزويد ليل 2
                if (cycleDay === 0 || cycleDay === 1 || cycleDay === 2)
                    return "Night 🌙"; // Night shift with 🌙
                return "OFF";
            } else {
                const cycleDay = daysBetween % 6; // 6-day cycle for تزويد صباح 1 and تزويد صباح 2
                if (cycleDay === 0 || cycleDay === 1 || cycleDay === 2)
                    return "AM ⛅️"; // Morning shift with ⛅️
                return "OFF";
            }
        }

        // Function to update the calendar display
        function updateCalendar() {
            // Update the month and year label
            monthYearLabel.textContent = `${months[currentMonth]} ${currentYear}`;

            // Clear the existing calendar grid
            calendarGrid.innerHTML = "";

            // Add day labels (Monday to Sunday)
            daysOfWeek.forEach(day => {
                const dayCell = document.createElement("div");
                dayCell.classList.add("day-cell");
                dayCell.textContent = day;
                calendarGrid.appendChild(dayCell);
            });

            // Get the first day of the month and total days in the month
            const startOfMonth = new Date(currentYear, currentMonth, 1);
            const firstWeekday = (startOfMonth.getDay() + 6) % 7; // Adjust to start from Monday
            const daysInMonth = new Date(currentYear, currentMonth + 1, 0).getDate();

            // Add empty cells to shift the dates (shift dates to the right)
            for (let i = 0; i < firstWeekday; i++) {
                const emptyCell = document.createElement("div");
                emptyCell.classList.add("calendar-cell");
                emptyCell.style.border = "none"; // No border for empty cells
                calendarGrid.appendChild(emptyCell);
            }

            // Add cells for the actual days of the month
            for (let day = 1; day <= daysInMonth; day++) {
                const date = new Date(currentYear, currentMonth, day);
                const cell = document.createElement("div");
                cell.classList.add("calendar-cell");

                // Add red border if the day is today
                if (
                    date.getDate() === today.getDate() &&
                    date.getMonth() === today.getMonth() &&
                    date.getFullYear() === today.getFullYear()
                ) {
                    cell.classList.add("current-day"); // Apply red border for current day
                }

                const shiftColor = getShiftColor(date);
                cell.style.backgroundColor = shiftColor;

                // Apply night shift class for styling
                if (shiftColor === "#818181") {
                    cell.classList.add("night-shift");
                }

                // Display the date number and shift label below it with symbols
                cell.innerHTML = `
                    ${day}
                    <div class="shift-label">${getShiftLabel(date)}</div>`;
                calendarGrid.appendChild(cell);
            }
        }

        // Event listeners
        prevMonthButton.addEventListener("click", previousMonth);
        nextMonthButton.addEventListener("click", nextMonth);
        resetButton.addEventListener("click", resetToCurrentDate);

        // Initialize the calendar on page load
        updateCalendar();
    </script>

</body>
</html>
