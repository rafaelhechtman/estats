<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Curiosidades Estatísticas do Futebol</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: #f8f9fa;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #ffffff;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        h1 {
            text-align: center;
        }
        .question-box {
            margin: 20px 0;
        }
        .question-box input, .question-box button {
            width: 100%;
            padding: 10px;
            margin: 5px 0;
        }
        .results {
            margin-top: 20px;
        }
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.0/papaparse.min.js"></script>
</head>
<body>
    <div class="container">
        <h1>Curiosidades Estatísticas do Futebol</h1>
        <div class="question-box">
            <input type="text" id="questionInput" placeholder="Faça sua pergunta aqui...">
            <button onclick="answerQuestion()">Perguntar</button>
        </div>
        <div class="results" id="results"></div>
    </div>
    <script>
        document.addEventListener("DOMContentLoaded", () => {
            // Function to load CSV data
            async function loadCSV(filePath) {
                const response = await fetch(filePath);
                const data = await response.text();
                return Papa.parse(data, { header: true }).data;
            }

            // Load all CSV files
            Promise.all([
                loadCSV('Libertadores_Matches.csv'),
                loadCSV('Brazilian_Cup_Matches.csv'),
                loadCSV('Brasileirao_Matches.csv'),
                loadCSV('fixtures.csv')
            ]).then(([libertadoresData, brazilianCupData, brasileiraoData, fixturesData]) => {
                window.libertadoresData = libertadoresData;
                window.brazilianCupData = brazilianCupData;
                window.brasileiraoData = brasileiraoData;
                window.fixturesData = fixturesData;
            });

            window.answerQuestion = function() {
                const question = document.getElementById("questionInput").value.toLowerCase();
                const resultsDiv = document.getElementById("results");

                let answer = "Desculpe, não encontrei a resposta para sua pergunta.";

                // Example: "qual o time que mais ganhou jogos na libertadores?"
                if (question.includes("time que mais ganhou jogos na libertadores")) {
                    const teamWins = {};
                    window.libertadoresData.forEach(match => {
                        const winner = match.Winner;
                        if (winner) {
                            teamWins[winner] = (teamWins[winner] || 0) + 1;
                        }
                    });
                    const maxWins = Math.max(...Object.values(teamWins));
                    const topTeam = Object.keys(teamWins).find(team => teamWins[team] === maxWins);
                    answer = `O time que mais ganhou jogos na Libertadores é o ${topTeam} com ${maxWins} vitórias.`;
                }

                // Additional logic for other questions can be added here

                resultsDiv.textContent = answer;
            }
        });
    </script>
</body>
</html>
