
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>A-Level Revision Planner</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary-color: #051014;
            --secondary-color: #2E2F2F;
            --physics-color: #7a3131;
            --maths-color: #0e6e54;
            --cs-color: #8C1A6A;
            --completed-color: #0cca2f;
            --text-color: #333333;              /* editable */
            --light-bg: #AAFFE5;
            --container-bg: #A657AE;            /* editable: main card/bg */
            --button-color: #9D75CB;            /* editable */
            --button-hover: #8C1A6A;            /* derived-ish */
            --week-number-color: #3498db;       /* editable */
        }
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            font-family: 'Poppins', sans-serif;
            background: linear-gradient(135deg, var(--primary-color) 0%, var(--secondary-color) 100%);
            color: var(--text-color);
            line-height: 1.6;
            padding: 20px;
            min-height: 100vh;
        }
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: var(--container-bg);
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
            overflow: hidden;
        }
        header { text-align: center; padding: 25px; background: #2c3e50; color: white; }
        h1 { margin-bottom: 10px; font-size: 2.2rem; }
        .subtitle { color: #3498db; font-size: 1.1rem; margin-bottom: 15px; }
        /* Countdown — de-yellowed & non-pulsing */
        .countdown {
            background: transparent;
            color: inherit;
            display: inline-block;
            padding: 6px 12px;
            border-radius: 8px;
            font-weight: 600;
            margin-top: 10px;
            border: 1px solid rgba(255,255,255,0.35);
            animation: none; /* removed pulsing */
        }
        .info-box { padding: 20px; background: var(--light-bg); border-bottom: 1px solid #e9ecef; }
        .info-box p { margin-bottom: 10px; }
        .controls {
            display: flex; justify-content: center; gap: 15px; padding: 15px; background: #e9ecef; flex-wrap: wrap;
        }
        .btn {
            padding: 10px 20px;
            background: var(--button-color);
            color: white;
            border: none; border-radius: 5px; cursor: pointer; font-weight: 500; transition: all 0.3s ease;
            display: flex; align-items: center; gap: 8px;
        }
        .btn:hover { background: var(--button-hover); transform: translateY(-2px); }
        .color-customizer { display: flex; gap: 10px; align-items: center; background: white; padding: 10px; border-radius: 5px; }
        .color-input { display: flex; align-items: center; gap: 6px; }
        .color-input label { font-size: 0.85rem; white-space: nowrap; }
        .color-input input { width: 30px; height: 30px; border: none; border-radius: 5px; cursor: pointer; }
        .weeks-view { padding: 20px; max-height: 50vh; overflow-y: auto; }
        .weeks-container { display: grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 20px; }
        .week { background: white; border-radius: 10px; padding: 15px; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); transition: transform 0.3s ease; border: 2px solid #e9ecef; cursor: pointer; }
        .week:hover { transform: translateY(-5px); border-color: #3498db; }
        .week-header { font-weight: 600; color: var(--text-color); margin-bottom: 15px; padding-bottom: 10px; border-bottom: 2px solid #3498db; display: flex; justify-content: space-between; align-items: center; }
        .week-number { background: var(--week-number-color); color: white; width: 30px; height: 30px; border-radius: 50%; display: flex; align-items: center; justify-content: center; }
        .week-summary { font-size: 0.9rem; color: #6c757d; }
        .week-view { display: none; padding: 20px; }
        .week-detail-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 20px; padding-bottom: 10px; border-bottom: 2px solid #3498db; gap: 10px; flex-wrap: wrap; }
        .back-btn { background: #6c757d; color: white; border: none; border-radius: 5px; padding: 8px 15px; cursor: pointer; display: flex; align-items: center; gap: 5px; }
        .days-container { display: grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 20px; }
        .day { background: white; border-radius: 10px; padding: 15px; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); transition: all 0.3s ease; }
        .day.completed { background: var(--completed-color); color: white; }
        .day.completed h3, .day.completed p, .day.completed a { color: white; }
        .day-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; }
        .day-title { font-weight: 600; color: var(--text-color); }
        .day.completed .day-title { color: white; }
        .subject { margin-bottom: 15px; }
        .subject h4 { color: var(--text-color); margin-bottom: 8px; display: flex; align-items: center; gap: 8px; }
        .day.completed .subject h4 { color: white; }
        .subject-icon { font-size: 1.2rem; }
        .physics-icon { color: var(--physics-color); }
        .maths-icon { color: var(--maths-color); }
        .cs-icon { color: var(--cs-color); }
        .resource-links { margin-top: 10px; font-size: 0.9rem; }
        .resource-links a { color: #3498db; text-decoration: none; display: block; margin-bottom: 5px; }
        .resource-links a:hover { text-decoration: underline; }
        .day.completed .resource-links a { color: #a7e6ff; }
        .checklist { margin-left: 5px; }
        .checklist-item { display: flex; align-items: center; margin-bottom: 8px; }
        .checklist-item input { margin-right: 10px; cursor: pointer; width: 18px; height: 18px; }
        .checklist-item label { cursor: pointer; font-size: 0.95rem; }
        .checklist-item input:checked + label { text-decoration: line-through; color: #777; }
        .day.completed .checklist-item input:checked + label { color: #ddd; }
        .progress-container { padding: 15px 20px; background: #f8f9fa; border-top: 1px solid #e9ecef; display: flex; align-items: center; justify-content: space-between; gap: 12px; }
        .progress-text { font-weight: 500; color: var(--text-color); }
        .progress-bar { height: 10px; flex-grow: 1; background: #e9ecef; border-radius: 5px; overflow: hidden; min-width: 120px; }
        .progress-bar.small { height: 8px; min-width: 160px; }
        .progress-fill { height: 100%; background: #3498db; border-radius: 5px; width: 0%; transition: width 0.5s ease; }
        footer { text-align: center; padding: 15px; color: #6c757d; font-size: 0.9rem; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        @keyframes pulse { 0% { transform: scale(1); } 50% { transform: scale(1.05); } 100% { transform: scale(1); } }
        @media (max-width: 768px) {
            .weeks-container, .days-container { grid-template-columns: 1fr; }
            .controls { flex-direction: column; }
            .color-customizer { flex-wrap: wrap; justify-content: center; }
            .progress-container { flex-direction: column; gap: 10px; }
            .progress-bar { width: 100%; }
            h1 { font-size: 1.8rem; }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <!-- TITLE: Change the text below to customize your title -->
            <h1>A-Level Revision Plan</h1>
            <div class="subtitle">
                <!-- SUBTITLE: Change the text below to customize your subtitle -->
                Structured revision plan for Physics, Maths, and Computer Science
            </div>
            <div>hi hello go get an A*</div>
        </header>

        <div class="info-box">
            <!-- BODY TEXT: Updated per request -->
            <p>This is a 35 week plan for A level revision in Maths, Physics and Computer Science. Completely vibecoded by smango23 and deepseek.</p>
            <p>
                <strong>Phase 1: Foundation &amp; First Pass</strong><br>
                <em>Goal:</em> To ensure you have a complete set of notes and a fundamental understanding of every single topic. Depth over speed.
            </p>
            <p>
                <strong>Phase 2: Intensify &amp; Past Papers</strong><br>
                <em>Goal:</em> To apply your knowledge through extensive past paper practice, identify weak areas, and interleave topics.
            </p>
            <p>
                <strong>Phase 3: Exam Condition &amp; Refinement</strong><br>
                <em>Goal:</em> To fine-tune exam technique, manage time under pressure, and have a final review of all content.
            </p>
        </div>

        <div class="controls">
            <button class="btn" id="saveProgress"><i class="fas fa-save"></i> Save Progress (File)</button>
            <button class="btn" id="saveLocal"><i class="fas fa-laptop"></i> Save to Device</button>
            <button class="btn" id="loadProgress"><i class="fas fa-folder-open"></i> Load Progress (File)</button>
            <button class="btn" id="resetProgress"><i class="fas fa-redo"></i> Reset Progress</button>

            <div class="color-customizer">
                <div class="color-input">
                    <label for="primaryColor">Primary</label>
                    <input type="color" id="primaryColor" value="#6a11cb">
                </div>
                <div class="color-input">
                    <label for="secondaryColor">Secondary</label>
                    <input type="color" id="secondaryColor" value="#2575fc">
                </div>
                <div class="color-input">
                    <label for="containerBg">Body BG</label>
                    <input type="color" id="containerBg" value="#ffffff">
                </div>
                <div class="color-input">
                    <label for="textColor">Text</label>
                    <input type="color" id="textColor" value="#333333">
                </div>
                <div class="color-input">
                    <label for="buttonColor">Buttons</label>
                    <input type="color" id="buttonColor" value="#3498db">
                </div>
                <div class="color-input">
                    <label for="weekNumColor">Week #</label>
                    <input type="color" id="weekNumColor" value="#3498db">
                </div>
                <button class="btn" id="applyColors"><i class="fas fa-palette"></i> Apply Colors</button>
            </div>
        </div>

        <div class="weeks-view" id="weeksView">
            <div class="weeks-container" id="weeksContainer">
                <!-- Weeks will be generated by JavaScript -->
            </div>
        </div>

        <div class="week-view" id="weekView">
            <div class="week-detail-header">
                <button class="back-btn" id="backToWeeks"><i class="fas fa-arrow-left"></i> Back to All Weeks</button>
                <h2 id="weekDetailTitle">Week 1 - Foundation Phase</h2>
                <!-- Per-week progress bar -->
                <div style="display:flex; align-items:center; gap:10px;">
                    <div class="progress-bar small"><div class="progress-fill" id="weekProgressFill"></div></div>
                    <div class="progress-text" id="weekProgressText">0%</div>
                </div>
            </div>
            <div class="days-container" id="daysContainer"><!-- Days will be injected --></div>
        </div>

        <div class="progress-container">
            <div class="progress-text">Overall Progress:</div>
            <div class="progress-bar"><div class="progress-fill" id="overallProgress"></div></div>
            <div class="progress-text" id="progressPercentage">0%</div>
        </div>

        <footer>
            <!-- FOOTER: Change the text below to customize your footer -->
            <p>© 2023 A-Level Revision Plan | AQA Physics, Edexcel Maths, OCR Computer Science</p>
        </footer>
    </div>

    <!-- File handling input (hidden) -->
    <input type="file" id="fileInput" style="display: none;" accept=".json">

    <script>
        // CONFIGURATION: Set your exam date here (YYYY-MM-DD format)
        const EXAM_DATE = new Date('2024-05-15');

        // CONFIGURATION: Update these arrays with your specific exam board topics
        const physicsTopics = [
            "Section 1 pt1. : Particles",
            "Section 1 pt2. : Radiation",
            "Rest",
            "Section 2 pt1. : Electromagnetic radiation",
            "Section 2 pt2. : Quantum Phenomena",
            "Rest",
            "Section 3 pt1. : Waves",
            "Section 3 pt2. : Waves 2",
            "Rest",
            "Section 4: Mechanics",
            "Section 5 pt1. : Materials",
            "Rest",
            "Section 5 pt2. : Materials 2 - Stress, Strain, YM",
            "Section 6 pt1. : Electricity",
            "Rest",
            "Section 6 pt2. : Electricity 2",
            "Section 7 pt1. : Further Mechanics",
            "Rest",
            "Section 7 pt2. : Further Mechanics 2",
            "Section 8: Thermal Physics",
            "Rest",
            "Section 9: Gravitational and Electric fields",
            "Section 10: Capacitors",
            "Rest",
            "Section 11: Magnetic Fields",
            "Section 12: Nuclear Physics",
            "Rest",
            "Option C: Engineering Physics / Unknown",
            "Option C: Engineering Physics / Unknown",
            "Rest",
            "Option C: Engineering Physics / Unknown",
            "Option C: Engineering Physics / Unknown",
            "Rest", 
            "Option C: Engineering Physics / Unknown",
            "Option C: Engineering Physics / Unknown",
            "Rest",
        ];
        const mathsTopics = [
            "Algebraic expressions and equations",
            "Rest",
            "Quadratic functions and graphs",
            "Coordinate geometry and circles",
            "Rest",
            "Rest",
            "Differentiation (first principles, rules, applications)",
            "Integration (indefinite, definite, applications)",
            "Rest",
            "Trigonometry (identities, equations, graphs)",
            "Exponentials and logarithms",
            "Rest",
            "Vectors (2D and 3D)",
            "Statistical sampling and data presentation",
            "Rest",
            "Probability distributions (binomial, normal)",
            "Hypothesis testing",
            "Rest",
            "Kinematics (constant acceleration, variable acceleration)",
            "Forces and Newton's laws",
            "Rest",
            "Moments and equilibrium"
        ];
        const csTopics = [
            "Rest",
            "Structure and function of the processor",
            "Work on project",
            "Rest",
            "Types of processor (CISC, RISC, multicore)",
            "Input, output and storage devices",
            "Rest",
            "Work on project",
            "Operating systems and utility software",
            "Rest",
            "Software development (libraries, interpreters, compilers)",
            "Work on project",
            "Rest",
            "Programming basics (variables, data types, operators)",
            "Structured programming (functions, procedures)",
            "Rest",
            "Work on project",
            "Robust and secure programming",
            "Rest",
            "Computational thinking (algorithms, efficiency)",
            "Work on project",
            "Rest",
            "Recursion and backtracking",
            "Data structures (arrays, records, lists, stacks, queues)",
            "Rest",
            "Work on project",
            "Boolean algebra and logic gates",
            "Rest",
            "Data representation (binary, hexadecimal, floating point)",
            "Work on project",
            "Rest",
            "Networks (topologies, protocols, security)",
            "Web technologies (HTML, CSS, JavaScript)",
            "Rest",
            "Work on project",
            "Databases (SQL, normalisation)",
            "Rest",
            "Ethical, legal and environmental issues",
            "Work on project",
            "Rest",
            "Work on project",
            "Work on project",
            "Computational thinking (algorithms, efficiency)"
        ];

        // Progress math
        const TOTAL_WEEKS = 35;
        const DAYS_PER_WEEK = 7;
        const TASKS_PER_DAY = 7; // 1 day-complete + 2x3 subject tasks
        function taskIds(weekNum, dayIdx) {
            return [
                `complete-${weekNum}-${dayIdx}`,
                `physics-${weekNum}-${dayIdx}-1`,
                `physics-${weekNum}-${dayIdx}-2`,
                `maths-${weekNum}-${dayIdx}-1`,
                `maths-${weekNum}-${dayIdx}-2`,
                `cs-${weekNum}-${dayIdx}-1`,
                `cs-${weekNum}-${dayIdx}-2`
            ];
        }

        document.addEventListener('DOMContentLoaded', function () {
            initializeApp();
            setupEventListeners();
            updateCountdown();
            setInterval(updateCountdown, 60000); // Update every minute (non-pulsing)
        });

        function initializeApp() {
            generateWeeks();
            loadProgressFromStorage();
            updateProgress();
            applySavedColorsOnLoad();
        }

        function setupEventListeners() {
            document.getElementById('saveProgress').addEventListener('click', saveProgressToFile);
            document.getElementById('saveLocal').addEventListener('click', () => {
                saveProgressToStorage();
                alert('Progress saved to this device.');
            });
            document.getElementById('loadProgress').addEventListener('click', () => document.getElementById('fileInput').click());
            document.getElementById('fileInput').addEventListener('change', loadProgressFromFile);
            document.getElementById('resetProgress').addEventListener('click', resetProgress);
            document.getElementById('backToWeeks').addEventListener('click', showWeeksView);
            document.getElementById('applyColors').addEventListener('click', applyCustomColors);
        }

        function updateCountdown() {
            const now = new Date();
            const timeDiff = EXAM_DATE - now;
            const daysDiff = Math.ceil(timeDiff / (1000 * 60 * 60 * 24));
            document.getElementById('countdown').textContent = `${daysDiff} days until exams`;
            // Removed dynamic info-box insertion per request
        }

        function generateWeeks() {
            const weeksContainer = document.getElementById('weeksContainer');
            weeksContainer.innerHTML = '';
            for (let weekNum = 1; weekNum <= TOTAL_WEEKS; weekNum++) {
                const weekEl = document.createElement('div');
                weekEl.className = 'week';
                weekEl.dataset.week = weekNum;
                let phase = '';
                if (weekNum <= 15) phase = 'Foundation Phase';
                else if (weekNum <= 28) phase = 'Intensify Phase';
                else phase = 'Exam Phase';
                weekEl.innerHTML = `
                            <div class="week-header">
                                <span>Week ${weekNum} - ${phase}</span>
                                <div class="week-number">${weekNum}</div>
                            </div>
                            <div class="week-summary"><p>Click to view daily revision plan</p></div>
                        `;
                weekEl.addEventListener('click', () => { showWeekDetail(weekNum); });
                weeksContainer.appendChild(weekEl);
            }
        }

        function showWeekDetail(weekNum) {
            document.getElementById('weeksView').style.display = 'none';
            document.getElementById('weekView').style.display = 'block';
            let phase = '';
            if (weekNum <= 15) phase = 'Foundation Phase';
            else if (weekNum <= 28) phase = 'Intensify Phase';
            else phase = 'Exam Phase';
            document.getElementById('weekDetailTitle').textContent = `Week ${weekNum} - ${phase}`;

            const daysContainer = document.getElementById('daysContainer');
            daysContainer.innerHTML = '';
            const days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'];

            for (let dayIdx = 0; dayIdx < 7; dayIdx++) {
                const dayEl = document.createElement('div');
                dayEl.className = 'day';
                dayEl.id = `week-${weekNum}-day-${dayIdx}`;
                const physicsTopic = physicsTopics[(weekNum + dayIdx - 1) % physicsTopics.length];
                const mathsTopic = mathsTopics[(weekNum + dayIdx - 1) % mathsTopics.length];
                const csTopic = csTopics[(weekNum + dayIdx - 1) % csTopics.length];

                dayEl.innerHTML = `
                            <div class="day-header">
                                <div class="day-title">${days[dayIdx]}</div>
                                <div>
                                    <input type="checkbox" id="complete-${weekNum}-${dayIdx}" class="day-complete-check">
                                    <label for="complete-${weekNum}-${dayIdx}">Mark day complete</label>
                                </div>
                            </div>
                            <div class="subject">
                                <h4><span class="subject-icon physics-icon"><i class="fas fa-atom"></i></span> Physics</h4>
                                <p>${physicsTopic}</p>
                                <div class="resource-links">
                                    <a href="https://www.physicsandmathstutor.com/physics-revision/" target="_blank">Physics &amp; Maths Tutor</a>
                                    <a href="https://www.savemyexams.com/a-level/physics/" target="_blank">Save My Exams</a>
                                </div>
                                <div class="checklist">
                                    <div class="checklist-item">
                                        <input type="checkbox" id="physics-${weekNum}-${dayIdx}-1">
                                        <label for="physics-${weekNum}-${dayIdx}-1">Study topic</label>
                                    </div>
                                    <div class="checklist-item">
                                        <input type="checkbox" id="physics-${weekNum}-${dayIdx}-2">
                                        <label for="physics-${weekNum}-${dayIdx}-2">Practice problems</label>
                                    </div>
                                </div>
                            </div>
                            <div class="subject">
                                <h4><span class="subject-icon maths-icon"><i class="fas fa-calculator"></i></span> Maths</h4>
                                <p>${mathsTopic}</p>
                                <div class="resource-links">
                                    <a href="https://www.physicsandmathstutor.com/maths-revision/" target="_blank">Physics &amp; Maths Tutor</a>
                                    <a href="https://www.examsolutions.net/" target="_blank">Exam Solutions</a>
                                </div>
                                <div class="checklist">
                                    <div class="checklist-item">
                                        <input type="checkbox" id="maths-${weekNum}-${dayIdx}-1">
                                        <label for="maths-${weekNum}-${dayIdx}-1">Learn concepts</label>
                                    </div>
                                    <div class="checklist-item">
                                        <input type="checkbox" id="maths-${weekNum}-${dayIdx}-2">
                                        <label for="maths-${weekNum}-${dayIdx}-2">Solve problems</label>
                                    </div>
                                </div>
                            </div>
                            <div class="subject">
                                <h4><span class="subject-icon cs-icon"><i class="fas fa-laptop-code"></i></span> Computer Science</h4>
                                <p>${csTopic}</p>
                                <div class="resource-links">
                                    <a href="https://www.physicsandmathstutor.com/computer-science-revision/" target="_blank">Physics &amp; Maths Tutor</a>
                                    <a href="https://www.csnewbs.com/" target="_blank">CS Newbs</a>
                                </div>
                                <div class="checklist">
                                    <div class="checklist-item">
                                        <input type="checkbox" id="cs-${weekNum}-${dayIdx}-1">
                                        <label for="cs-${weekNum}-${dayIdx}-1">Study theory</label>
                                    </div>
                                    <div class="checklist-item">
                                        <input type="checkbox" id="cs-${weekNum}-${dayIdx}-2">
                                        <label for="cs-${weekNum}-${dayIdx}-2">Practice coding</label>
                                    </div>
                                </div>
                            </div>
                        `;

                const dayCompleteCheck = dayEl.querySelector('.day-complete-check');
                dayCompleteCheck.addEventListener('change', function () {
                    if (this.checked) {
                        dayEl.classList.add('completed');
                        dayEl.querySelectorAll('input[type="checkbox"]').forEach(cb => { cb.checked = true; });
                    } else {
                        dayEl.classList.remove('completed');
                    }
                    saveProgressToStorage();
                    updateProgress();
                    updateWeekProgress(weekNum);
                });

                dayEl.querySelectorAll('input[type="checkbox"]').forEach(checkbox => {
                    checkbox.addEventListener('change', function () {
                        saveProgressToStorage();
                        updateProgress();
                        updateWeekProgress(weekNum);
                        const allChecked = Array.from(dayEl.querySelectorAll('input[type="checkbox"]')).every(cb => cb.checked);
                        if (allChecked) {
                            dayEl.classList.add('completed');
                            dayCompleteCheck.checked = true;
                        } else {
                            dayEl.classList.remove('completed');
                            dayCompleteCheck.checked = false;
                        }
                    });
                });

                daysContainer.appendChild(dayEl);
            }

            loadWeekState(weekNum);
            updateWeekProgress(weekNum);
        }

        function showWeeksView() {
            document.getElementById('weeksView').style.display = 'block';
            document.getElementById('weekView').style.display = 'none';
        }

        function loadWeekState(weekNum) {
            const savedState = JSON.parse(localStorage.getItem('revisionProgress')) || {};
            for (let dayIdx = 0; dayIdx < 7; dayIdx++) {
                const dayId = `week-${weekNum}-day-${dayIdx}`;
                const dayEl = document.getElementById(dayId);
                if (dayEl && savedState[dayId]) {
                    const checkboxes = dayEl.querySelectorAll('input[type="checkbox"]');
                    checkboxes.forEach(checkbox => {
                        if (savedState[dayId][checkbox.id] !== undefined) {
                            checkbox.checked = savedState[dayId][checkbox.id];
                        }
                    });
                    if (savedState[dayId].completed) dayEl.classList.add('completed');
                }
            }
        }

        function saveProgressToStorage() {
            const state = JSON.parse(localStorage.getItem('revisionProgress')) || {};
            for (let weekNum = 1; weekNum <= TOTAL_WEEKS; weekNum++) {
                for (let dayIdx = 0; dayIdx < 7; dayIdx++) {
                    const dayId = `week-${weekNum}-day-${dayIdx}`;
                    const dayEl = document.getElementById(dayId);
                    // Create an object even if the day isn't currently rendered
                    if (!state[dayId]) state[dayId] = { completed: false };
                    if (dayEl) {
                        state[dayId].completed = dayEl.classList.contains('completed');
                        const checkboxes = dayEl.querySelectorAll('input[type="checkbox"]');
                        checkboxes.forEach(checkbox => { state[dayId][checkbox.id] = checkbox.checked; });
                    } else {
                        // Ensure keys exist even when not rendered
                        taskIds(weekNum, dayIdx).forEach(id => {
                            if (state[dayId][id] === undefined) state[dayId][id] = false;
                        });
                    }
                }
            }
            localStorage.setItem('revisionProgress', JSON.stringify(state));
        }

        function loadProgressFromStorage() {
            const savedState = JSON.parse(localStorage.getItem('revisionProgress')) || {};
            // Only restore for what's in the DOM now (week view) — rest is accounted for in overall progress math
            if (document.getElementById('weekView').style.display === 'block') {
                const daysContainer = document.getElementById('daysContainer');
                daysContainer.querySelectorAll('.day').forEach(dayEl => {
                    const dayId = dayEl.id;
                    if (savedState[dayId]) {
                        const checkboxes = dayEl.querySelectorAll('input[type="checkbox"]');
                        checkboxes.forEach(checkbox => {
                            if (savedState[dayId][checkbox.id] !== undefined) checkbox.checked = savedState[dayId][checkbox.id];
                        });
                        if (savedState[dayId].completed) dayEl.classList.add('completed');
                    }
                });
            }
            updateProgress();
        }

        function saveProgressToFile() {
            const state = JSON.parse(localStorage.getItem('revisionProgress')) || {};
            const dataStr = JSON.stringify(state, null, 2);
            const dataUri = 'data:application/json;charset=utf-8,' + encodeURIComponent(dataStr);
            const exportFileDefaultName = 'revision-progress.json';
            const linkElement = document.createElement('a');
            linkElement.setAttribute('href', dataUri);
            linkElement.setAttribute('download', exportFileDefaultName);
            linkElement.click();
        }

        function loadProgressFromFile(event) {
            const file = event.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function (e) {
                try {
                    const state = JSON.parse(e.target.result);
                    localStorage.setItem('revisionProgress', JSON.stringify(state));
                    loadProgressFromStorage();
                    alert('Progress loaded successfully!');
                } catch (error) {
                    alert('Error loading file: Invalid format');
                }
            };
            reader.readAsText(file);
            event.target.value = '';
        }

        function resetProgress() {
            if (confirm('Are you sure you want to reset all progress? This cannot be undone.')) {
                localStorage.removeItem('revisionProgress');
                document.querySelectorAll('input[type="checkbox"]').forEach(cb => cb.checked = false);
                document.querySelectorAll('.day').forEach(day => day.classList.remove('completed'));
                updateProgress();
                alert('Progress reset successfully!');
            }
        }

        function updateProgress() {
            // Compute global progress from storage to include ALL weeks/days
            const savedState = JSON.parse(localStorage.getItem('revisionProgress')) || {};
            let checked = 0;
            for (let weekNum = 1; weekNum <= TOTAL_WEEKS; weekNum++) {
                for (let dayIdx = 0; dayIdx < 7; dayIdx++) {
                    const dayId = `week-${weekNum}-day-${dayIdx}`;
                    const ids = taskIds(weekNum, dayIdx);
                    const dayState = savedState[dayId] || {};
                    ids.forEach(id => { if (dayState[id]) checked++; });
                }
            }
            const total = TOTAL_WEEKS * DAYS_PER_WEEK * TASKS_PER_DAY;
            const percentage = total > 0 ? Math.round((checked / total) * 100) : 0;
            document.getElementById('overallProgress').style.width = percentage + '%';
            document.getElementById('progressPercentage').textContent = percentage + '%';
        }

        function updateWeekProgress(weekNum) {
            const savedState = JSON.parse(localStorage.getItem('revisionProgress')) || {};
            let checked = 0;
            for (let dayIdx = 0; dayIdx < 7; dayIdx++) {
                const dayId = `week-${weekNum}-day-${dayIdx}`;
                const ids = taskIds(weekNum, dayIdx);
                const dayState = savedState[dayId] || {};
                ids.forEach(id => { if (dayState[id]) checked++; });
            }
            const total = DAYS_PER_WEEK * TASKS_PER_DAY; // 49
            const pct = total > 0 ? Math.round((checked / total) * 100) : 0;
            const fill = document.getElementById('weekProgressFill');
            const text = document.getElementById('weekProgressText');
            if (fill) fill.style.width = pct + '%';
            if (text) text.textContent = pct + '%';
        }

        function applyCustomColors() {
            const primaryColor = document.getElementById('primaryColor').value;
            const secondaryColor = document.getElementById('secondaryColor').value;
            const containerBg = document.getElementById('containerBg').value;
            const textColor = document.getElementById('textColor').value;
            const buttonColor = document.getElementById('buttonColor').value;
            const weekNumColor = document.getElementById('weekNumColor').value;

            document.documentElement.style.setProperty('--primary-color', primaryColor);
            document.documentElement.style.setProperty('--secondary-color', secondaryColor);
            document.documentElement.style.setProperty('--container-bg', containerBg);
            document.documentElement.style.setProperty('--text-color', textColor);
            document.documentElement.style.setProperty('--button-color', buttonColor);
            document.documentElement.style.setProperty('--week-number-color', weekNumColor);
            // Simple hover fallback: slightly darker or same if not provided
            document.documentElement.style.setProperty('--button-hover', buttonColor);

            // Persist selections
            localStorage.setItem('colorPrimary', primaryColor);
            localStorage.setItem('colorSecondary', secondaryColor);
            localStorage.setItem('colorContainerBg', containerBg);
            localStorage.setItem('colorText', textColor);
            localStorage.setItem('colorButton', buttonColor);
            localStorage.setItem('colorWeekNum', weekNumColor);

            // Update gradient background immediately
            document.body.style.background = `linear-gradient(135deg, ${primaryColor} 0%, ${secondaryColor} 100%)`;
        }

        function applySavedColorsOnLoad() {
            const savedPrimary = localStorage.getItem('colorPrimary');
            const savedSecondary = localStorage.getItem('colorSecondary');
            const savedContainerBg = localStorage.getItem('colorContainerBg');
            const savedText = localStorage.getItem('colorText');
            const savedButton = localStorage.getItem('colorButton');
            const savedWeekNum = localStorage.getItem('colorWeekNum');
            if (savedPrimary) document.getElementById('primaryColor').value = savedPrimary;
            if (savedSecondary) document.getElementById('secondaryColor').value = savedSecondary;
            if (savedContainerBg) document.getElementById('containerBg').value = savedContainerBg;
            if (savedText) document.getElementById('textColor').value = savedText;
            if (savedButton) document.getElementById('buttonColor').value = savedButton;
            if (savedWeekNum) document.getElementById('weekNumColor').value = savedWeekNum;
            if (savedPrimary && savedSecondary) document.body.style.background = `linear-gradient(135deg, ${savedPrimary} 0%, ${savedSecondary} 100%)`;
            if (savedPrimary) document.documentElement.style.setProperty('--primary-color', savedPrimary);
            if (savedSecondary) document.documentElement.style.setProperty('--secondary-color', savedSecondary);
            if (savedContainerBg) document.documentElement.style.setProperty('--container-bg', savedContainerBg);
            if (savedText) document.documentElement.style.setProperty('--text-color', savedText);
            if (savedButton) {
                document.documentElement.style.setProperty('--button-color', savedButton);
                document.documentElement.style.setProperty('--button-hover', savedButton);
            }
            if (savedWeekNum) document.documentElement.style.setProperty('--week-number-color', savedWeekNum);
        }
    </script>

    <section>
        <h2>Revision Notes</h2>

        <div class="note-section">
            <h3>Physics</h3>
            <textarea id="notes-physics" placeholder="Type your Physics notes..."></textarea>
        </div>

        <div class="note-section">
            <h3>Maths</h3>
            <textarea id="notes-maths" placeholder="Type your Maths notes..."></textarea>
        </div>

        <div class="note-section">
            <h3>Computer Science</h3>
            <textarea id="notes-cs" placeholder="Type your Computer Science notes..."></textarea>
        </div>
    </section>
    <script>
        function setupNotes(id, storageKey) {
            const box = document.getElementById(id);
            box.value = localStorage.getItem(storageKey) || "";
            box.addEventListener("input", () => {
                localStorage.setItem(storageKey, box.value);
            });
        }

        setupNotes("notes-physics", "notesPhysics");
        setupNotes("notes-maths", "notesMaths");
        setupNotes("notes-cs", "notesCS");
    </script>

    <style>
        section {
            margin-top: 20px;
        }

        .note-section {
            margin-bottom: 20px;
        }

        textarea {
            width: 100%;
            max-width: 600px;
            height: 150px;
            padding: 12px;
            font-size: 16px;
            border: 2px solid #ccc;
            border-radius: 10px;
            outline: none;
            resize: vertical;
            box-shadow: 2px 2px 6px rgba(0,0,0,0.1);
        }

            textarea:focus {
                border-color: #4CAF50;
                box-shadow: 0 0 8px rgba(76, 175, 80, 0.4);
            }
    </style>
</body>
</html>
