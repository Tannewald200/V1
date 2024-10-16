<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Skalierungsfragen zur Selbsteinschätzung</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Roboto', sans-serif;
            font-size: 1.1em;
            background-color: #e3f2fd;
            color: #333;
            margin: 0;
            padding: 0;
        }
        h1, h2 {
            text-align: center;
            color: #1e88e5;
        }
        h2 {
            margin-top: 30px;
        }
        .slider-container {
            display: flex;
            align-items: center;
            margin-bottom: 15px;
            background: #ffffff;
            padding: 10px;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s;
        }
        .slider-container:hover {
            transform: scale(1.02);
        }
        .question {
            flex: 3;
            margin-right: 20px;
        }
        .label-left, .label-right {
            width: 50px;
            text-align: center;
            font-weight: bold;
        }
        input[type='range'] {
            flex-grow: 2;
            margin: 0 15px;
            -webkit-appearance: none;
            appearance: none;
            height: 8px;
            background: #bbdefb;
            outline: none;
            border-radius: 5px;
            transition: background 0.3s;
        }
        input[type='range']:hover {
            background: #1e88e5;
        }
        #ergebnis {
            margin-top: 30px;
            font-size: 1.5em;
            text-align: center;
        }
        .result-bar {
            display: flex;
            justify-content: center;
            align-items: flex-end;
            height: 300px;
            margin-top: 20px;
            gap: 20px;
        }
        .bar {
            width: 60px;
            margin: 0 10px;
            background: linear-gradient(to top, #1976d2, #42a5f5);
            text-align: center;
            color: white;
            padding: 5px 0;
            border-radius: 10px 10px 0 0;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            transition: transform 0.3s, background 0.3s;
            position: relative;
            overflow: hidden;
        }
        .bar:hover {
            transform: scale(1.15);
            background: linear-gradient(to top, #0d47a1, #1976d2);
        }
        .bar::after {
            content: "";
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 10px;
            background: rgba(255, 255, 255, 0.3);
            animation: shine 1.5s infinite;
        }
        @keyframes shine {
            0% {
                transform: translateY(100%);
            }
            100% {
                transform: translateY(-100%);
            }
        }
        button {
            display: block;
            margin: 40px auto;
            padding: 10px 20px;
            background-color: #1e88e5;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 1.2em;
            cursor: pointer;
            transition: background-color 0.3s, transform 0.3s;
        }
        button:hover {
            background-color: #1565c0;
            transform: scale(1.05);
        }
        .top-image {
            display: block;
            max-width: 100%;
            height: auto;
            margin: 20px auto;
        }
        .summary-table {
            width: 80%;
            margin: 40px auto;
            border-collapse: collapse;
            text-align: center;
        }
        .summary-table th, .summary-table td {
            border: 1px solid #ccc;
            padding: 10px;
        }
        .summary-table th {
            background-color: #1e88e5;
            color: white;
        }
    </style>
    <script>
        function berechneDurchschnitte() {
            const blockAnzahl = 5;
            let durchschnittswerte = [];
            let maxHoehe = 300;
            let fragenNamen = [];
            
            for (let i = 1; i <= blockAnzahl; i++) {
                let fragen = document.querySelectorAll(`.block${i} input[type='range']`);
                let fragenLabels = document.querySelectorAll(`.block${i} .question`);
                let summe = 0;
                let anzahl = 0;

                fragen.forEach((frage, index) => {
                    let wert = parseInt(frage.value);
                    if (!isNaN(wert)) {
                        summe += wert;
                        anzahl++;
                        fragenNamen.push({ name: fragenLabels[index].innerText.split(':')[0], wert: wert });
                    }
                });

                let durchschnitt = anzahl > 0 ? (summe / anzahl).toFixed(2) : "Keine Werte";
                durchschnittswerte.push(durchschnitt);
            }

            fragenNamen.sort((a, b) => b.wert - a.wert);
            let staerken = fragenNamen.slice(0, 3);
            let schwaechen = fragenNamen.slice(-3);

            let ergebnisContainer = document.getElementById("ergebnis");
            ergebnisContainer.innerHTML = "";

            let resultBarContainer = document.createElement("div");
            resultBarContainer.className = "result-bar";

            durchschnittswerte.forEach((wert, index) => {
                let bar = document.createElement("div");
                bar.className = "bar";
                bar.style.height = (wert / 10 * maxHoehe) + "px";
                bar.innerHTML = `<div style="position: absolute; top: -30px; width: 100%; font-weight: bold;">Säule ${index + 1}</div><div style="position: relative; top: 50%; transform: translateY(-50%);">${wert}</div>`;
                resultBarContainer.appendChild(bar);
            });

            ergebnisContainer.appendChild(resultBarContainer);

            // Tabelle der Stärken und Schwächen erstellen
            let summaryTable = document.createElement("table");
            summaryTable.className = "summary-table";

            let headerRow = document.createElement("tr");
            headerRow.innerHTML = "<th>Deine Stärken 💪</th><th>Deine Schwächen 🤔</th>";
            summaryTable.appendChild(headerRow);

            for (let i = 0; i < 3; i++) {
                let row = document.createElement("tr");
                row.innerHTML = `<td>${staerken[i].name}</td><td>${schwaechen[i].name}</td>`;
                summaryTable.appendChild(row);
            }

            ergebnisContainer.appendChild(summaryTable);
        }
    </script>
</head>
<body>
        <h1>Skalierungsfragen zur Selbsteinschätzung</h1>
    <h2>Dieses Programm speichert oder sendet keine deiner Daten und nur du siehst die Ergebnisse.</h2>
    
    <!-- Block 1: Leiblichkeit -->
    <div class="block1">
        <h2>1. Säule: Leiblichkeit</h2>
        <div class="slider-container">
            <span class="question">💪 Körperliche Vitalität: Fühlen Sie sich wohl in Ihrem Körper?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">🏃 Fitness: Fühlen Sie sich gesund und leistungsfähig?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">🧠 Mentale Gesundheit: Fühlen Sie sich mental gesund?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">🧘 Seelisches Wohlbefinden: Fühlen Sie sich ausgeglichen?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">👀 Aussehen: Sind Sie zufrieden mit Ihrem Aussehen?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">❤️ Sexualität: Befriedigt Sie Ihre Sexualität?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">🛡️ Belastbarkeit: Sind Sie belastbar?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
    </div>

    <!-- Block 2: Soziale Beziehungen -->
    <div class="block2">
        <h2>2. Säule: Soziale Beziehungen</h2>
        <div class="slider-container">
            <span class="question">🤝 Kontaktfähigkeit: Fällt es Ihnen leicht, persönliche Nähe zu anderen Menschen zu finden?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">💬 Selbstbehauptung: Können Sie den Platz in Gruppen einnehmen, den Sie wollen?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">👨‍👩‍👧 Familie: Fühlen Sie sich in Ihrer Familie angenommen und respektiert?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">❤️ Beziehung/Partnerschaft: Können Sie in der Partnerschaft sich selbst sein?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">👫 Freunde/Vertrauenspersonen: Haben Sie jemanden, mit dem Sie über alles sprechen können?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">🎨 Freizeitgestaltung: Können Sie sich in Ihrer Freizeit beschäftigen (Hobbies, Vereinstätigkeit)?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">📱 Soziale Medien: Wie zufrieden sind Sie mit Ihren Kontakten in sozialen Medien?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
    </div>

    <!-- Block 3: Arbeit und Leistung -->
    <div class="block3">
        <h2>3. Säule: Arbeit und Leistung</h2>
        <div class="slider-container">
            <span class="question">💼 Arbeit: Betrachten Sie Ihre Arbeit als wichtig und sinnvoll?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">🏆 Anerkennung: Erhalten Sie Anerkennung für Ihre Arbeit?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">📊 Beurteilung: Sind Sie mit Ihren Leistungsbeurteilungen zufrieden?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">😌 Arbeitszufriedenheit: Entspricht Ihre Arbeit Ihren Neigungen und Vorstellungen?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">⚖️ Arbeitsauslastung: Beurteilen Sie Ihre Arbeitsbeanspruchung als angemessen?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">💡 Fähigkeiten: Erfüllt Ihre Arbeit die Ansprüche, die Sie sich gesetzt haben?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">📈 Chancen: Können Sie sich bei Ihrer Arbeit nach Ihren Vorstellungen weiterentwickeln?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
    </div>

    <!-- Block 4: Materielle Sicherheit -->
    <div class="block4">
        <h2>4. Säule: Materielle Sicherheit</h2>
        <div class="slider-container">
            <span class="question">💰 Einkommen: Können Sie sich leisten, was Sie benötigen?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">🛍️ Entfaltungsmöglichkeit: Können Sie sich Ihre materiellen Wünsche erfüllen?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">🏠 Besitz/Vermögen: Wie beurteilen Sie Ihre finanziellen Perspektiven?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">🏡 Wohnsituation: Sind Sie zufrieden mit Ihrer Wohnsituation?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">🔒 Vorsorge/Sicherheit: Sehen Sie Ihre Existenz langfristig gesichert?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
    </div>

    <!-- Block 5: Werte und Ideale -->
    <div class="block5">
        <h2>5. Säule: Werte und Ideale</h2>
        <div class="slider-container">
            <span class="question">⚖️ Moral: Können Sie Ihr Leben nach Ihren persönlichen moralischen Grundsätzen führen?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">💬 Ethik: Können Sie für Ihre Überzeugungen einstehen?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">🙏 Religion/Glaube: Können Sie gemäß Ihrer Religion oder Ihrem Glauben leben?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">📜 Traditionen: Wie stark werden Sie von Traditionen beeinflusst?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">🌟 Ideale: Wissen Sie, was für Sie im Leben erstrebenswert ist?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">👥 Vorbilder: Haben Vorbilder für Sie eine wichtige Bedeutung?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
        <div class="slider-container">
            <span class="question">⚖️ Gerechtigkeit: Fühlen Sie sich in unserer Gesellschaft gerecht behandelt?</span>
            <span class="label-left">😞 1</span>
            <input type="range" min="1" max="10" value="5">
            <span class="label-right">😊 10</span>
        </div>
    </div>

    <button onclick="berechneDurchschnitte()">Durchschnitt berechnen</button>
    <div id="ergebnis"></div>
</body>
</html>
