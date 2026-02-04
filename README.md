<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lagro Reviewer: Career Guidance</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&family=Playfair+Display:wght@700&display=swap" rel="stylesheet">

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        'lagro-green': '#064e3b',
                        'lagro-emerald': '#059669',
                        'lagro-light': '#ecfdf5',
                        'lagro-accent': '#facc15',
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        serif: ['Playfair Display', 'serif'],
                    }
                }
            }
        }
    </script>

    <style>
        .chart-container { position: relative; width: 100%; max-width: 600px; height: 300px; margin: 0 auto; }
        .subject-card:hover { transform: translateY(-4px); box-shadow: 0 10px 15px -3px rgba(6, 78, 59, 0.2); }
        .lesson-item:hover { background-color: #f0fdf4; padding-left: 1.5rem; }
        .fade-in { animation: fadeIn 0.3s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .custom-scroll::-webkit-scrollbar { width: 6px; }
        .custom-scroll::-webkit-scrollbar-track { background: #f1f1f1; }
        .custom-scroll::-webkit-scrollbar-thumb { background: #059669; border-radius: 4px; }
        
        .lesson-content h3 { color: #064e3b; font-weight: 700; font-size: 1.25rem; margin-top: 1.5rem; margin-bottom: 0.75rem; border-bottom: 2px solid #059669; padding-bottom: 0.5rem; }
        .lesson-content h4 { color: #059669; font-weight: 600; font-size: 1.1rem; margin-top: 1rem; margin-bottom: 0.5rem; }
        .lesson-content p { margin-bottom: 0.75rem; line-height: 1.6; color: #374151; }
        .lesson-content ul { list-style-type: disc; padding-left: 1.5rem; margin-bottom: 1rem; color: #374151; }
        .lesson-content li { margin-bottom: 0.5rem; }
        .lesson-content strong { color: #064e3b; font-weight: 700; }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 font-sans antialiased h-screen flex flex-col overflow-hidden">

    <header class="bg-lagro-green border-b border-lagro-emerald shadow-md z-20 flex-none">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center gap-3 cursor-pointer" onclick="goHome()">
                <div class="w-10 h-10 bg-white rounded-lg flex items-center justify-center shadow-inner overflow-hidden border border-emerald-400">
                    <img src="lagro.png" 
                         alt="Lagro Logo" 
                         class="w-full h-full object-cover"
                         onerror="this.src='data:image/svg+xml;charset=UTF-8,%3Csvg viewBox%3D%220 0 24 24%22 xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%3Cpath d%3D%22M12 3L1 9l11 6 9-4.91V17h2V9L12 3z%22 fill%3D%22%23064e3b%22%2F%3E%3C%2Fsvg%3E'">
                </div>
                <h1 class="font-serif font-bold text-white text-xl tracking-tight hidden sm:block">Lagro Reviewer</h1>
            </div>
            
            <nav class="flex text-sm font-medium text-emerald-100 overflow-hidden whitespace-nowrap">
                <button onclick="goHome()" class="hover:text-white transition">Home</button>
                <span id="crumb-subject-sep" class="mx-2 hidden">/</span>
                <button id="crumb-subject" onclick="goBackToLessons()" class="hover:text-white transition hidden">...</button>
                <span id="crumb-lesson-sep" class="mx-2 hidden">/</span>
                <span id="crumb-lesson" class="text-white font-bold hidden">...</span>
            </nav>

            <div class="w-10"></div>
        </div>
    </header>

    <main class="flex-grow overflow-y-auto custom-scroll relative p-4 sm:p-6 lg:p-8 max-w-7xl mx-auto w-full">
        
        <div id="view-subjects" class="fade-in">
            <div class="text-center mb-10">
                <h2 class="text-3xl font-serif font-bold text-lagro-green">Academic Dashboard</h2>
                <p class="text-slate-500 mt-2">WAZUP LAGRONIANS!</p>
            </div>
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6" id="subject-grid"></div>
        </div>

        <div id="view-lessons" class="hidden fade-in">
            <div class="mb-6">
                <button onclick="goHome()" class="text-xs font-bold text-lagro-emerald hover:text-lagro-green mb-2 flex items-center gap-1">← BACK TO DASHBOARD</button>
                <h2 id="lesson-page-title" class="text-3xl font-serif font-bold text-lagro-green">Subject Title</h2>
            </div>
            <div class="bg-white rounded-2xl shadow-sm border border-emerald-100 overflow-hidden">
                <ul id="lesson-list" class="divide-y divide-emerald-50"></ul>
            </div>
        </div>

        <div id="view-content" class="hidden fade-in h-full flex flex-col">
            <div class="mb-6 flex-none">
                <button onclick="goBackToLessons()" class="text-xs font-bold text-lagro-emerald hover:text-lagro-green mb-2 flex items-center gap-1">← BACK TO LESSONS</button>
                <h2 id="content-title" class="text-2xl font-serif font-bold text-lagro-green">Lesson Title</h2>
            </div>

            <div class="flex space-x-1 bg-emerald-50 p-1.5 rounded-xl mb-6 w-full max-w-md mx-auto flex-none shadow-inner">
                <button onclick="switchTab('read')" id="tab-read" class="flex-1 py-2 text-sm font-bold rounded-lg shadow-sm bg-white text-lagro-green transition">📖 READ</button>
                <button onclick="switchTab('visuals')" id="tab-visuals" class="flex-1 py-2 text-sm font-bold rounded-lg text-emerald-700 hover:bg-white/50 transition">📊 CHARTS</button>
                <button onclick="switchTab('quiz')" id="tab-quiz" class="flex-1 py-2 text-sm font-bold rounded-lg text-emerald-700 hover:bg-white/50 transition">🧠 QUIZ (10)</button>
            </div>

            <div class="flex-grow bg-white rounded-2xl shadow-xl border border-emerald-100 p-6 sm:p-10 overflow-y-auto custom-scroll relative mb-4">
                <div id="content-read" class="lesson-content"></div>
                <div id="content-visuals" class="hidden h-full flex flex-col items-center justify-center space-y-8"></div>
                <div id="content-quiz" class="hidden"><div id="quiz-wrapper" class="max-w-2xl mx-auto"></div></div>
            </div>
        </div>

    </main>

    <script>
        const subjects = {
            "career": {
                id: "career",
                title: "Career Guidance",
                icon: "🎓",
                color: "bg-emerald-50 text-lagro-green border-emerald-200",
                desc: "Higher Education Pathways, Holistic Career Dev, and Employment Types.",
                lessons: [
                    {
                        id: "l1",
                        title: "Lesson 1: Higher Education Pathways",
                        desc: "Academic, Vocational, Pathway/Foundation, and Transfer routes.",
                        content: {
                            read: `
                                <h3>1. Academic Pathways</h3>
                                <p>Traditional routes focused on theoretical knowledge and academic disciplines.</p>
                                <h4>Undergraduate Degrees:</h4>
                                <ul>
                                    <li><strong>Associate Degree:</strong> Usually 2 years at a community or technical college.</li>
                                    <li><strong>Bachelor’s Degree:</strong> Usually 3–4 years at a university or college.</li>
                                </ul>
                                <h4>Graduate Degrees:</h4>
                                <ul>
                                    <li><strong>Master’s Degree:</strong> 1–2 years after a bachelor's degree.</li>
                                    <li><strong>Doctoral Degree:</strong> 3–7 years after a master's or directly after a bachelor's.</li>
                                </ul>
                                <h4>Special Programs:</h4>
                                <ul>
                                    <li><strong>Honors Programs:</strong> For high-achieving undergraduates, often involving research.</li>
                                    <li><strong>Dual Degrees / Double Majors:</strong> Earning two degrees simultaneously.</li>
                                    <li><strong>Accelerated Programs:</strong> Faster completion (e.g., 3-year bachelor's); often whole-year round instead of by semester.</li>
                                    <li><strong>Pre-Professional Programs:</strong> For careers like medicine, law, or education.</li>
                                </ul>
                                <h3>2. Vocational / Technical Pathways</h3>
                                <p>Focused on practical skills and job-specific training.</p>
                                <ul>
                                    <li><strong>Certificates and Diplomas:</strong> Often 6 months to 2 years in technical schools or community colleges.</li>
                                    <li><strong>Apprenticeships:</strong> Work-based learning with classroom instruction in skilled trades.</li>
                                    <li><strong>Traineeships:</strong> Similar to apprenticeships, but often shorter and less technical.</li>
                                </ul>
                                <h3>3. Pathway / Foundation Programs</h3>
                                <ul>
                                    <li><strong>University Foundation Year:</strong> One-year programs preparing students for undergraduate studies.</li>
                                    <li><strong>English Language Pathways:</strong> For international students needing to improve language skills.</li>
                                    <li><strong>Bridging Courses:</strong> Short programs to help students meet academic prerequisites.</li>
                                </ul>
                                <h3>4. Transfer Pathways</h3>
                                <ul>
                                    <li><strong>2+2 Programs:</strong> Start with an associate degree then transfer to a university for the final two years.</li>
                                    <li><strong>Articulation Agreements:</strong> Formal partnerships that simplify credit transfer.</li>
                                </ul>
                                <h3>5. Online / Distance Learning Pathways</h3>
                                <ul>
                                    <li><strong>MOOCs:</strong> Massive Open Online Courses (free or low-cost).</li>
                                    <li><strong>Microcredentials:</strong> Targeted, focused approach to learning a specific practical skill.</li>
                                    <li><strong>Blended Learning:</strong> A mix of online and on-campus instruction.</li>
                                </ul>
                            `,
                            chartType: "bar",
                            quiz: [
                                { q: "How long is a typical Associate Degree?", options: ["1 year", "2 years", "4 years", "6 months"], a: 1 },
                                { q: "Which program is designed for high-achieving undergraduates and often involves research?", options: ["Bridging Course", "Honors Program", "Vocational Course", "Traineeship"], a: 1 },
                                { q: "What is an 'Accelerated Program'?", options: ["A program for athletes", "Faster completion via year-round study", "A program for slow learners", "A degree taken via radio"], a: 1 },
                                { q: "Which pathway focuses on practical skills and job-specific training?", options: ["Academic", "Vocational", "Foundation", "Transfer"], a: 1 },
                                { q: "What does a '2+2' program allow a student to do?", options: ["Take 4 classes", "Transfer from community college to university", "Graduate in 2 years", "Get two degrees in 2 years"], a: 1 },
                                { q: "What are MOOCs?", options: ["Mandatory Online Only Classes", "Massive Open Online Courses", "Major Office Operations Code", "Mixed Online Open Credit"], a: 1 },
                                { q: "Which program is a one-year prep for undergraduate studies?", options: ["Bridging", "University Foundation Year", "Doctoral", "Microcredential"], a: 1 },
                                { q: "What allows entry based on life or work experience?", options: ["RPL", "Dual Degree", "Honors", "Articulations"], a: 0 },
                                { q: "How long are Certificates and Diplomas usually?", options: ["4 years", "6 months to 2 years", "10 years", "1 month"], a: 1 },
                                { q: "What involves work-based learning in skilled trades?", options: ["MOOC", "Apprenticeship", "Bridging", "Associate Degree"], a: 1 }
                            ]
                        }
                    },
                    {
                        id: "l2",
                        title: "Lesson 2: Holistic Career Development",
                        desc: "Whole person lens and key career choice influences.",
                        content: {
                            read: `
                                <h3>1. Career Pathways: A Holistic Perspective</h3>
                                <p>Career development is viewed through a <strong>'whole person' lens</strong>, considering skills, interests, values, personality, life circumstances, and cultural background. Pathways are <strong>not linear</strong>.</p>
                                <h3>2. Key Influences on Career Choice</h3>
                                <ul>
                                    <li><strong>Parental Influence:</strong> Recognized as the most significant factor.</li>
                                    <li><strong>Peer/Media Exposure:</strong> TikTok/YouTube trends (e.g., vloggers, K-pop idols) shape dreams.</li>
                                    <li><strong>Financial Constraints:</strong> Leads students toward quicker workforce routes (e.g., TESDA NC) over 4-year degrees.</li>
                                    <li><strong>Gender Roles:</strong> Societal push for females (teaching/caregiving) vs males (engineering/technical).</li>
                                    <li><strong>Interest and Aptitude:</strong> Central, but can be overridden by external pressures (e.g., parents wanting medical vs IT).</li>
                                </ul>
                                <h3>3. Summary of Academic/Vocational (Review)</h3>
                                <p>This lesson reiterates that Academic pathways are theoretical, while Vocational pathways are practical skills-based.</p>
                            `,
                            chartType: "doughnut",
                            quiz: [
                                { q: "What is the 'most significant' factor in career choice?", options: ["Media", "Peers", "Parental Influence", "Weather"], a: 2 },
                                { q: "Career pathways are described as being:", options: ["Linear", "Not linear", "Strictly vertical", "Short"], a: 1 },
                                { q: "The 'whole person' lens includes which of the following?", options: ["Only skills", "Interests, values, and life circumstances", "Only grades", "Strictly financial status"], a: 1 },
                                { q: "Why might a student choose a TESDA NC course over a 4-year degree?", options: ["They hate studying", "Financial constraints", "Gender roles", "It's more prestigious"], a: 1 },
                                { q: "Social trends on which platforms play a role in career choice?", options: ["Newspapers", "TikTok and YouTube", "Radio", "Posters"], a: 1 },
                                { q: "Gender roles often push males toward which fields?", options: ["Caregiving", "Engineering/Technical", "Teaching", "Nursing"], a: 1 },
                                { q: "What can sometimes override a student's aptitude?", options: ["Internal hobbies", "External pressures (e.g. Parents)", "Reading speed", "Sleep habits"], a: 1 },
                                { q: "Gender roles often push females toward which fields?", options: ["Aviation", "Teaching/Caregiving", "Construction", "Mining"], a: 1 },
                                { q: "A 'Holistic Perspective' considers which of the following?", options: ["Cultural background", "Personality", "Life circumstances", "All of the above"], a: 3 },
                                { q: "Vocational pathways are focused on what?", options: ["Theoretical research", "Practical skills/Job-specific training", "Pure philosophy", "Ancient history"], a: 1 }
                            ]
                        }
                    },
                    {
                        id: "l3",
                        title: "Lesson 3: Types of Employment",
                        desc: "Work hours, duration, and relationship with employers.",
                        content: {
                            read: `
                                <h3>1. Definition of Employment Types</h3>
                                <p>Describes the nature of a work arrangement, categorized by hours, duration, and worker-employer relationship.</p>
                                <h3>2. By Work Hours and Duration</h3>
                                <ul>
                                    <li><strong>Full-Time:</strong> Considered permanent and have job security.</li>
                                    <li><strong>Part-Time:</strong> Typically paid an hourly wage with limited or no benefits.</li>
                                    <li><strong>Casual:</strong> Work is flexible but provides no job security or paid leave.</li>
                                    <li><strong>Seasonal:</strong> Employment is temporary and ends when the season is over.</li>
                                </ul>
                                <h3>3. By Relationship with Employer</h3>
                                <ul>
                                    <li><strong>Gig:</strong> The worker is an independent contractor, not a traditional employee.</li>
                                    <li><strong>Freelance:</strong> Operates own business, sets own hours, handles own taxes and benefits.</li>
                                    <li><strong>Contractual:</strong> Governed by a contract outlining start/end dates, scope, and pay.</li>
                                </ul>
                                <h3>4. Resume</h3>
                                <p>A resume originated from the French word <strong>résumé</strong>, which means <strong>'summary'</strong>.</p>
                            `,
                            chartType: "none",
                            quiz: [
                                { q: "Which employment type is considered permanent with job security?", options: ["Casual", "Seasonal", "Full-Time", "Gig"], a: 2 },
                                { q: "What does the French word 'résumé' mean?", options: ["Job", "History", "Summary", "Letter"], a: 2 },
                                { q: "Which type of employment has no job security or paid leave?", options: ["Full-Time", "Contractual", "Casual", "Permanent"], a: 2 },
                                { q: "In which type is the worker an independent contractor, not an employee?", options: ["Full-Time", "Seasonal", "Gig", "Part-Time"], a: 2 },
                                { q: "Freelancers are responsible for their own:", options: ["Taxes", "Benefits", "Hours", "All of the above"], a: 3 },
                                { q: "Which employment ends when a specific season is over?", options: ["Full-Time", "Casual", "Seasonal", "Freelance"], a: 2 },
                                { q: "How are Part-Time workers typically paid?", options: ["Annual salary", "Hourly wage", "Gift cards", "Unpaid"], a: 1 },
                                { q: "Which arrangement is governed by start/end dates and specific scope of work?", options: ["Casual", "Contractual", "Gig", "Permanent"], a: 1 },
                                { q: "What describes the nature of a work arrangement?", options: ["Aptitude", "Employment Type", "Personality", "Major"], a: 1 },
                                { q: "Which worker operates their own business and sets their own hours?", options: ["Full-Time Employee", "Freelancer", "Casual Worker", "Seasonal Worker"], a: 1 }
                            ]
                        }
                    }
                ]
            }
        };

        let currentSubject = null, currentLesson = null, currentChart = null;

        function goHome() { currentSubject = null; currentLesson = null; renderSubjects(); showView('view-subjects'); updateBreadcrumbs(); }
        function selectSubject(id) { currentSubject = subjects[id]; renderLessons(); showView('view-lessons'); updateBreadcrumbs(); }
        function goBackToLessons() { currentLesson = null; showView('view-lessons'); updateBreadcrumbs(); }
        function selectLesson(id) { currentLesson = currentSubject.lessons.find(l => l.id === id); renderContent(); showView('view-content'); updateBreadcrumbs(); }
        function showView(id) { ['view-subjects', 'view-lessons', 'view-content'].forEach(v => document.getElementById(v).classList.add('hidden')); document.getElementById(id).classList.remove('hidden'); }
        
        function updateBreadcrumbs() {
            const s = document.getElementById('crumb-subject'), l = document.getElementById('crumb-lesson');
            document.getElementById('crumb-subject-sep').classList.toggle('hidden', !currentSubject);
            s.classList.toggle('hidden', !currentSubject);
            if (currentSubject) s.innerText = currentSubject.title;
            document.getElementById('crumb-lesson-sep').classList.toggle('hidden', !currentLesson);
            l.classList.toggle('hidden', !currentLesson);
            if (currentLesson) l.innerText = currentLesson.title;
        }

        function renderSubjects() {
            const g = document.getElementById('subject-grid'); g.innerHTML = '';
            Object.values(subjects).forEach(sub => {
                g.innerHTML += `<div onclick="selectSubject('${sub.id}')" class="subject-card bg-white p-8 rounded-2xl border ${sub.color.split(' ')[2]} shadow-sm cursor-pointer transition flex flex-col items-center text-center">
                    <div class="w-20 h-20 rounded-2xl ${sub.color.split(' ')[0]} flex items-center justify-center text-4xl mb-4 shadow-sm">${sub.icon}</div>
                    <h3 class="text-xl font-extrabold text-lagro-green mb-2">${sub.title}</h3>
                    <p class="text-sm text-slate-500 font-medium">${sub.desc}</p>
                </div>`;
            });
        }

        function renderLessons() {
            document.getElementById('lesson-page-title').innerText = currentSubject.title;
            const list = document.getElementById('lesson-list'); list.innerHTML = '';
            currentSubject.lessons.forEach(ls => {
                list.innerHTML += `<li onclick="selectLesson('${ls.id}')" class="lesson-item p-6 cursor-pointer transition flex justify-between items-center group">
                    <div><h4 class="text-lg font-bold text-lagro-green group-hover:text-emerald-600 transition">${ls.title}</h4><p class="text-sm text-slate-500 mt-1">${ls.desc}</p></div>
                    <span class="text-lagro-emerald opacity-50 group-hover:opacity-100 transition font-bold">START QUIZ →</span>
                </li>`;
            });
        }

        function renderContent() {
            document.getElementById('content-title').innerText = currentLesson.title;
            document.getElementById('content-read').innerHTML = currentLesson.content.read;
            switchTab('read');
            renderQuiz();
        }

        function switchTab(t) {
            ['read', 'visuals', 'quiz'].forEach(tab => {
                document.getElementById(`tab-${tab}`).classList.toggle('bg-white', tab === t);
                document.getElementById(`tab-${tab}`).classList.toggle('text-lagro-green', tab === t);
                document.getElementById(`content-${tab}`).classList.toggle('hidden', tab !== t);
            });
            if (t === 'visuals') renderVisuals();
        }

        function renderVisuals() {
            const c = document.getElementById('content-visuals'); c.innerHTML = '';
            if (currentLesson.content.chartType === 'none') { c.innerHTML = '<p class="text-slate-400 font-bold italic">Visual aids are not required for this topic.</p>'; return; }
            const cw = document.createElement('div'); cw.className = 'chart-container';
            const can = document.createElement('canvas'); cw.appendChild(can); c.appendChild(cw);
            if (currentChart) currentChart.destroy();
            const ctx = can.getContext('2d');
            if (currentLesson.content.chartType === 'bar') {
                currentChart = new Chart(ctx, { type: 'bar', data: { labels: ['Associate', 'Bachelor', 'Master', 'Doctoral'], datasets: [{ label: 'Approx Years Required', data: [2, 4, 2, 5], backgroundColor: '#059669' }] }, options: { responsive: true, maintainAspectRatio: false }});
            } else {
                currentChart = new Chart(ctx, { type: 'doughnut', data: { labels: ['Parental', 'Media', 'Financial', 'Personal'], datasets: [{ data: [60, 20, 10, 10], backgroundColor: ['#064e3b', '#059669', '#10b981', '#6ee7b7'] }] }, options: { responsive: true, maintainAspectRatio: false }});
            }
        }

        function renderQuiz() {
            const w = document.getElementById('quiz-wrapper'); w.innerHTML = '';
            currentLesson.content.quiz.forEach((q, i) => {
                const d = document.createElement('div'); d.className = 'mb-10 p-8 bg-emerald-50/50 rounded-2xl border border-emerald-100 shadow-sm';
                let h = `<p class="font-extrabold text-lagro-green text-lg mb-6">Q${i+1}: ${q.q}</p><div class="grid gap-3">`;
                q.options.forEach((o, oi) => { h += `<button onclick="checkAnswer(this, ${q.a === oi})" class="w-full text-left px-5 py-4 bg-white border-2 border-transparent rounded-xl hover:border-emerald-300 transition-all font-semibold text-slate-700 shadow-sm">${o}</button>`; });
                h += `</div><p class="feedback hidden mt-5 font-bold text-center"></p>`;
                d.innerHTML = h; w.appendChild(d);
            });
        }

        window.checkAnswer = function(btn, isCorrect) {
            const p = btn.parentElement; const f = p.parentElement.querySelector('.feedback');
            Array.from(p.children).forEach(b => { b.disabled = true; b.classList.add('opacity-40'); });
            btn.classList.replace('border-transparent', isCorrect ? 'border-green-500' : 'border-red-500');
            btn.classList.add(isCorrect ? 'bg-green-50' : 'bg-red-50');
            btn.classList.remove('opacity-40');
            f.innerText = isCorrect ? "CORRECT ✅" : "WRONG ❌";
            f.className = `feedback mt-5 font-black text-center ${isCorrect ? 'text-green-600' : 'text-red-600'}`;
            f.classList.remove('hidden');
        };

        document.addEventListener('DOMContentLoaded', renderSubjects);
    </script>
</body>
</html>
