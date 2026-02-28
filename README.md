# index.html
live school marks status
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Science Fest Live Scoreboard</title>
    <style>
        body { margin: 0; padding: 0; font-family: 'Segoe UI', sans-serif; overflow-x: hidden; background: #87CEEB; }
        /* Sky & Aeroplane Theme */
        .sky { height: 100vh; background: linear-gradient(#b3e5fc, #81d4fa); position: relative; }
        .airplane { position: absolute; font-size: 50px; animation: fly 15s linear infinite; top: 10%; z-index: 1; }
        @keyframes fly { 
            from { left: -100px; transform: rotate(5deg); } 
            to { left: 110%; transform: rotate(-5deg); } 
        }
        /* Colorful Leaderboard */
        #scoreboard { position: relative; z-index: 10; width: 80%; margin: 50px auto; background: rgba(255,255,255,0.9); border-radius: 20px; padding: 20px; box-shadow: 0 10px 30px rgba(0,0,0,0.2); }
        h1 { text-align: center; color: #0277bd; }
        .school-row { display: flex; justify-content: space-between; padding: 15px; margin: 10px 0; border-radius: 10px; color: white; font-weight: bold; font-size: 1.2rem; }
        /* Alternating colourful rows */
        .school-row:nth-child(odd) { background-color: #ff5722; }
        .school-row:nth-child(even) { background-color: #4caf50; }
    </style>
</head>
<body>
    <div class="sky">
        <div class="airplane">✈️</div>
        <div id="scoreboard">
            <h1>Science Fest: Live School Marks</h1>
            <div id="data-container">Loading live marks...</div>
        </div>
    </div>

    <script>
        // PASTE YOUR PUBLISHED GOOGLE SHEET CSV LINK HERE
        const sheetUrl = 'YOUR_PUBLISHED_CSV_LINK_HERE';

        async function updateScores() {
            try {
                const response = await fetch(sheetUrl);
                const data = await response.text();
                const rows = data.split('\n').slice(1); // Skip header row
                let html = '';
                
                rows.forEach(row => {
                    const [school, marks] = row.split(',');
                    if (school && marks) {
                        html += `<div class="school-row"><span>${school}</span><span>${marks} pts</span></div>`;
                    }
                });
                document.getElementById('data-container').innerHTML = html;
            } catch (e) {
                console.error("Error loading marks", e);
            }
        }

        // Update scores every 10 seconds automatically
        setInterval(updateScores, 10000);
        updateScores();
    </script>
</body>
</html>
