<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Jadwal Bola Hari Ini</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    body {
      margin: 0;
      padding: 20px;
      font-family: 'Segoe UI', sans-serif;
      background-color: #0d1117;
      color: #e6edf3;
    }
    h1 {
      text-align: center;
      margin-bottom: 30px;
      color: #00ffc6;
    }
    .match-card {
      background: #161b22;
      padding: 15px;
      margin: 10px auto;
      border-radius: 10px;
      max-width: 600px;
      box-shadow: 0 0 10px #000;
    }
    .league {
      font-weight: bold;
      color: #facc15;
      margin-bottom: 5px;
    }
    .teams {
      font-size: 18px;
      font-weight: bold;
      color: #ffffff;
    }
    .time {
      color: #9ca3af;
      margin-top: 5px;
    }
    #matches {
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    .loading {
      text-align: center;
      font-style: italic;
      color: #888;
    }
  </style>
</head>
<body>

  <h1>Jadwal Bola Hari Ini</h1>
  <div id="matches" class="loading">Memuat data...</div>

  <script>
    const apiToken = "123"; // Ganti dengan token dari soccerdataapi.com
    const today = new Date().toISOString().split("T")[0];

    fetch(`https://api.soccerdataapi.com/fixtures/?auth_token=${123}&date=${today}`, {
      headers: {
        "Accept-Encoding": "gzip"
      }
    })
    .then(response => response.json())
    .then(data => {
      const container = document.getElementById("matches");
      container.classList.remove("loading");
      container.innerHTML = "";

      if (!data.results || data.results.length === 0) {
        container.innerHTML = "<p>Tidak ada pertandingan hari ini.</p>";
        return;
      }

      data.results.forEach(match => {
        const matchEl = document.createElement("div");
        matchEl.className = "match-card";

        const leagueName = match.competition_name || "Kompetisi Tidak Diketahui";
        const home = match.home_team?.name || "Tuan Rumah";
        const away = match.away_team?.name || "Tamu";
        const kickoff = new Date(match.date).toLocaleTimeString("id-ID", {
          hour: "2-digit",
          minute: "2-digit"
        });

        matchEl.innerHTML = `
          <div class="league">${leagueName}</div>
          <div class="teams">${home} vs ${away}</div>
          <div class="time">Kick-off: ${kickoff}</div>
        `;

        container.appendChild(matchEl);
      });
    })
    .catch(error => {
      console.error("Gagal mengambil data:", error);
      document.getElementById("matches").innerText = "Gagal memuat data jadwal.";
    });
  </script>
</body>
</html>
