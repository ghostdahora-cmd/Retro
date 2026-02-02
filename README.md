<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>RetroStats | prozinhghost</title>
    <style>
        :root { --main-color: #2da44e; --bg-dark: #0d1117; --card-bg: #161b22; }
        body { background: var(--bg-dark); color: #c9d1d9; font-family: sans-serif; margin: 0; padding: 20px; }
        .container { max-width: 800px; margin: auto; }
        
        /* Cabeçalho igual ao perfil do RA */
        .profile-header { background: var(--card-bg); padding: 20px; border-radius: 10px; border: 1px solid #30363d; display: flex; align-items: center; gap: 20px; }
        #avatar { width: 100px; height: 100px; border-radius: 8px; border: 2px solid var(--main-color); }
        .user-info h1 { margin: 0; color: white; }
        .stats-row { display: flex; gap: 15px; margin-top: 10px; font-weight: bold; }
        .stat-item { color: var(--main-color); }

        /* Lista de Jogos */
        .section-title { margin-top: 30px; border-bottom: 1px solid #30363d; padding-bottom: 10px; color: white; }
        .game-card { background: var(--card-bg); margin-top: 15px; padding: 15px; border-radius: 8px; display: flex; justify-content: space-between; align-items: center; border-left: 5px solid var(--main-color); }
        .game-title { font-size: 1.1em; color: white; font-weight: bold; }
        .badge { background: #21262d; padding: 5px 10px; border-radius: 5px; font-size: 12px; }
    </style>
</head>
<body>

<div class="container">
    <div class="profile-header">
        <img id="avatar" src="" alt="Carregando...">
        <div class="user-info">
            <h1 id="username">prozinhghost</h1>
            <div class="stats-row">
                <span class="stat-item">🏆 <span id="points">0</span></span>
                <span class="stat-item">🥇 Rank: #<span id="rank">--</span></span>
            </div>
            <p id="motto" style="font-style: italic; color: #8b949e;">Carregando resumo do RetroAchievements...</p>
        </div>
    </div>

    <h2 class="section-title">Atividade Recente</h2>
    <div id="recent-games">
        </div>
</div>

<script>
    const USER = "prozinhghost";
    const KEY = "LrIRtdUr2A0vg5BTMWNq3lRr5kYP8Z43";

    async function getRetroData() {
        const response = await fetch(`https://retroachievements.org/API/API_GetUserSummary.php?u=${USER}&y=${KEY}`);
        const data = await response.json();

        // Preenche o topo
        document.getElementById('avatar').src = "https://retroachievements.org" + data.UserPic;
        document.getElementById('points').innerText = data.TotalPoints.toLocaleString();
        document.getElementById('rank').innerText = data.Rank;
        document.getElementById('motto').innerText = data.Motto || "Membro de Elite";

        // Preenche o último jogo (exemplo simplificado)
        const gamesDiv = document.getElementById('recent-games');
        const game = data.LastGame;
        
        gamesDiv.innerHTML = `
            <div class="game-card">
                <div>
                    <div class="game-title">${game.Title}</div>
                    <div style="font-size: 12px; color: #8b949e;">${game.ConsoleName}</div>
                </div>
                <div class="badge">Última sessão: ${data.LastActivity.LastUpdate}</div>
            </div>
        `;
    }

    getRetroData();
</script>
</body>
</html>
