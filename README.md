<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Navegador Customizado</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
            background-color: #111;
            color: white;
            text-align: center;
        }
        iframe {
            width: 100%;
            height: 80vh;
            border: none;
            background: white;
            transition: opacity 0.3s ease;
        }
        .bottom-bar {
            position: fixed;
            bottom: 0;
            width: 100%;
            background: #222;
            padding: 10px;
            display: flex;
            justify-content: center;
            gap: 10px;
            transition: transform 0.3s ease;
        }
        .hidden {
            transform: translateY(100%);
        }
        .iframe-hidden {
            opacity: 0;
            pointer-events: none;
        }
        button {
            background: #444;
            color: white;
            border: none;
            padding: 10px;
            cursor: pointer;
            border-radius: 5px;
        }
        input {
            padding: 5px;
            border-radius: 5px;
            border: none;
        }
        .saved-sites {
            position: fixed;
            top: 10px;
            left: 10px;
            background: rgba(0, 0, 0, 0.8);
            padding: 10px;
            border-radius: 5px;
            display: none;
        }
        .saved-sites button {
            display: block;
            margin-top: 5px;
            background: none;
            border: none;
            color: white;
            font-size: 16px;
        }
        .icon {
            font-size: 20px;
            cursor: pointer;
        }
        .saved-sites-btn {
            font-size: 20px;
            cursor: pointer;
            margin-left: 10px;
        }
    </style>
</head>
<body>
    <div class="saved-sites" id="savedSites"></div>
    <iframe id="browser" src="https://www.google.com"></iframe>
    <div class="bottom-bar" id="bottomBar">
        <input type="text" id="urlInput" placeholder="Digite a URL...">
        <button onclick="navigate()" class="icon">🔍</button>
        <button onclick="saveSite()" class="icon">💾</button>
        <button class="saved-sites-btn" onclick="toggleSavedSitesList()">📑</button>
    </div>
    <script>
        let showBar = true;
        let showIframe = true;
        const savedSites = [];

        document.addEventListener("keydown", (e) => {
            if (e.key === "0") {
                showBar = !showBar;
                showIframe = !showIframe;
                document.getElementById("bottomBar").classList.toggle("hidden", !showBar);
                document.getElementById("savedSites").style.display = showBar ? "block" : "none";
                document.getElementById("browser").classList.toggle("iframe-hidden", !showIframe);
            }
        });

        function navigate() {
            const url = document.getElementById("urlInput").value;
            if (url) {
                document.getElementById("browser").src = url.startsWith("http") ? url : "https://" + url;
            }
        }

        function saveSite() {
            const url = document.getElementById("browser").src;
            const faviconUrl = `https://www.google.com/s2/favicons?domain=${new URL(url).hostname}`;
            const savedSitesDiv = document.getElementById("savedSites");

            if (!savedSites.includes(url)) {
                savedSites.push(url);
                const button = document.createElement("button");
                button.innerHTML = `<img src="${faviconUrl}" style="width: 20px; height: 20px;"> ${new URL(url).hostname}`;
                button.onclick = () => document.getElementById("browser").src = url;
                savedSitesDiv.appendChild(button);
            }
        }

        function toggleSavedSitesList() {
            const savedSitesDiv = document.getElementById("savedSites");
            savedSitesDiv.style.display = savedSitesDiv.style.display === "block" ? "none" : "block";
        }
    </script>
</body>
</html>
