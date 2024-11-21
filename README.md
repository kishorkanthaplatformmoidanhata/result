<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>কিশোরকন্ঠ মেধাবৃত্তি পরীক্ষা</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f9f9f9;
            margin: 0;
            padding: 0;
        }

        .container {
            max-width: 800px;
            margin: 50px auto;
            background: #ffffff;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
        }

        h3, h4 {
            text-align: center;
            color: #333;
            margin-bottom: 10px;
        }

        h4 {
            background-color: yellow;
        }

        form {
            margin-bottom: 30px;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        label {
            display: block;
            font-weight: bold;
            margin-bottom: 5px;
            color: #555;
        }

        input[type="text"] {
            width: calc(100% - 20px);
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
        }

        .button-group {
            display: flex;
            gap: 10px;
        }

        button {
            flex: 1;
            padding: 10px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        button[type="submit"] {
            background-color: #007BFF;
            color: white;
        }

        button[type="submit"]:hover {
            background-color: #0056b3;
        }

        button[type="reset"] {
            background-color: #FF4D4D;
            color: white;
        }

        button[type="reset"]:hover {
            background-color: #cc0000;
        }

        .result-section {
            display: none;
            margin-top: 20px;
        }

        .table-section {
            margin-bottom: 20px;
        }

        .table-section h3 {
            background-color: #28a745;
            color: white;
            padding: 10px;
            text-align: center;
            margin: 0;
            border-radius: 5px 5px 0 0;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 16px;
            background-color: #e9f5e9;
        }

        th, td {
            padding: 12px;
            text-align: left;
            border: 1px solid #ddd;
        }

        th {
            background-color: white;
            color: black;
        }

        tr:nth-child(even) {
            background-color: #f9f9f9;
        }

        tr:hover {
            background-color: #f1f1f1;
        }

        .error {
            color: red;
            text-align: center;
            font-weight: bold;
            margin-top: 20px;
        }

        .moving-text {
            width: 100%;
            white-space: nowrap;
            overflow: hidden;
            background-color: #28a745;
            color: white;
            font-size: 18px;
            font-weight: bold;
            padding: 10px;
            box-sizing: border-box;
        }

        .moving-text span {
            display: inline-block;
            padding-left: 100%;
            animation: move-text 10s linear infinite;
        }

        @keyframes move-text {
            from {
                transform: translateX(0);
            }
            to {
                transform: translateX(-100%);
            }
        }
    </style>
</head>
<body>
    <section class="container">
        <div class="moving-text">
            <span>কিশোরকন্ঠ মেধাবৃত্তি পরীক্ষা, ময়দানহাটা</span>
        </div>
        
        <h3>পরীক্ষার ফলাফল</h3>
        <form id="resultForm" onsubmit="showResult(event)">
            <label for="roll">রোল নাম্বার:</label>
            <input type="text" id="roll" required>

            <label for="reg">রেজিস্ট্রেশন নাম্বার:</label>
            <input type="text" id="reg" required>

            <div class="button-group">
                <button type="submit">রেজাল্ট দেখুন</button>
                <button type="reset" onclick="resetForm()">রিসেট করুন</button>
            </div>
        </form>

        <div id="resultSection" class="result-section">
            <div class="table-section">
                <h3>শিক্ষার্থীর তথ্য</h3>
                <table>
                    <tr>
                        <td>শিক্ষার্থীর নাম</td>
                        <td id="studentName"></td>
                    </tr>
                    <tr>
                        <td>প্রতিষ্ঠানের নাম</td>
                        <td id="institutionName"></td>
                    </tr>
                    <tr>
                        <td>রোল</td>
                        <td id="studentRoll"></td>
                    </tr>
                    <tr>
                        <td>রেজিস্ট্রেশন নাম্বার</td>
                        <td id="studentReg"></td>
                    </tr>
                    <tr>
                        <td>র‍্যাঙ্ক</td>
                        <td id="studentRank"></td>
                    </tr>
                </table>
            </div>

            <div class="table-section">
                <h3>ফলাফল</h3>
                <table>
                    <thead>
                        <tr>
                            <th>বিষয় কোড</th>
                            <th>বিষয়</th>
                            <th>নাম্বার</th>
                            <th>গ্রেড</th>
                        </tr>
                    </thead>
                    <tbody id="resultTable"></tbody>
                </table>
            </div>
        </div>

        <p id="errorMessage" class="error"></p>
    </section>

    <script>
      const students = [
   
//রেজাল্ট ডেটাবেজ    
    //কারিগড়ি কলেজ
    {
        roll: "১০১",
        reg: "২০২৪০১",
        name: "মো: মনির হোসাইন",
        institution: "কারিগড়ি কলেজ",
        rank: "১",
        subjects: [
            { code: "১০১", name: "কিশোরন্ঠ", marks: 85, grade: "A" }
        ]
    },
    {
        roll: "১০২",
        reg: "২০২৪০২",
        name: "মো: শফিউল ইসলাম",
        institution: " কারিগড়ি কলেজ",
        rank: "২",
        subjects: [
            { code: "১০২", name: "কিশোরকন্ঠ", marks: 88, grade: "A" }
        ]
    },
    {
        roll: "১০৩",
        reg: "২০২৪০৩",
        name: "মো: রাকিব ইসলাম",
        institution: "কারিগড়ি কলেজ",
        rank: "৩",
        subjects: [
            { code: "১০৩", name: "কিশোরকন্ঠ", marks: 92, grade: "A+" }
        ]
    },
    {
        roll: "১০৪",
        reg: "২০২৪০৪",
        name: "মো: জাহেদ রহমান বাবু ",
        institution: "কাড়িগড়ি কলেজ",
        rank: "৪",
        subjects: [
            { code: "১০৪", name: "কিশোরকন্ঠ", marks: 75, grade: "B+" }
        ]
    },
    {
        roll: "১০৫",
        reg: "২০২৪০৫",
        name: "মো: শহীদ মিয়া  ",
        institution: "কাড়িগড়ি কলেজ",
        rank: "৫",
        subjects: [
            { code: "১০৫", name: "কিশোরকন্ঠ", marks: 90, grade: "A+" }
        ]
    },
    {
        roll: "১০৬",
        reg: "২০২৪০৬",
        name: "মো: সজীব মিয়া",
        institution: "কারিগড়ি কলেজ",
        rank: "৬",
        subjects: [
            { code: "১০৬", name: "কিশোরকন্ঠ", marks: 80, grade: "A" }
        ]
    },


    {
        roll: "১০৭",
        reg: "২০২৪০৭",
        name: "নূর মোহাম্মদ",
        institution: "কাড়িগড়ি কলেজ",
        rank: "৭",
        subjects: [
            { code: "১০৭", name: " কিশোরকন্ঠ", marks: 78, grade: "A" }
        ]
    },
    {
        roll: "১০৮",
        reg: "২০২৪০৮",
        name: "হাসান মাহমুদ",
        institution: "কারিগড়ি কলেজ",
        rank: "৮",
        subjects: [
            { code: "১০৮", name: "কিশোরকন্ঠ", marks: 85, grade: "A" }
        ]
    },
    {
        roll: "১০৯",
        reg: "২০২৪০৯",
        name: "ফাহিম আহমেদ",
        institution: "কারিগড়ি কলেজ",
        rank: "৯",
        subjects: [
            { code: "১০৯", name: "কিশোরকন্ঠ", marks: 70, grade: "B" }
        ]
    },
    {
        roll: "১১০",
        reg: "২০২৪১০",
        name: "সোহেল রানা",
        institution: "কারিগড়ি কলেজ",
        rank: "১০",
        subjects: [
            { code: "১১০", name: "কিশোরকন্ঠ", marks: 88, grade: "A" }
        ]
    },
    
    
    //রেজাল্ট
  
];
    
        function showResult(event) {
            event.preventDefault();

            const rollInput = document.getElementById("roll").value.trim();
            const regInput = document.getElementById("reg").value.trim();
            const resultSection = document.getElementById("resultSection");
            const studentName = document.getElementById("studentName");
            const institutionName = document.getElementById("institutionName");
            const studentRoll = document.getElementById("studentRoll");
            const studentReg = document.getElementById("studentReg");
            const studentRank = document.getElementById("studentRank");
            const resultTable = document.getElementById("resultTable");
            const errorMessage = document.getElementById("errorMessage");

            studentName.textContent = "";
            institutionName.textContent = "";
            studentRoll.textContent = "";
            studentReg.textContent = "";
            studentRank.textContent = "";
            resultTable.innerHTML = "";
            errorMessage.textContent = "";
            resultSection.style.display = "none";

            const student = students.find(
                s => s.roll === rollInput && s.reg === regInput
            );

            if (student) {
                studentName.textContent = student.name;
                institutionName.textContent = student.institution;
                studentRoll.textContent = student.roll;
                studentReg.textContent = student.reg;
                studentRank.textContent = student.rank;

                student.subjects.forEach(subject => {
                    const row = document.createElement("tr");
                    row.innerHTML = `
                        <td>${subject.code}</td>
                        <td>${subject.name}</td>
                        <td>${subject.marks}</td>
                        <td>${subject.grade}</td>
                    `;
                    resultTable.appendChild(row);
                });

                resultSection.style.display = "block";
            } else {
                errorMessage.textContent = "এই রোল এবং রেজিস্ট্রেশন নাম্বারের জন্য কোনো রেজাল্ট পাওয়া যায়নি!";
            }
        }

        function resetForm() {
            document.getElementById("resultSection").style.display = "none";
            document.getElementById("errorMessage").textContent 
            = "";
        }
    </script>
</body>
</html>
