<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Mixtape Retro</title>

<style>
    body {
        font-family: Arial, sans-serif;
        background-color: #4b2e19;
        color: white;
        margin: 0;
        padding: 20px;
    }

    .container {
        background-color: #d7b899;
        color: #4b2e19;
        max-width: 900px;
        margin: auto;
        padding: 25px;
        border-radius: 20px;
        box-shadow: 0 0 20px rgba(0,0,0,0.4);
    }

    h1, h2 {
        text-align: center;
    }

    label { font-weight: bold; }

    input {
        width: 100%;
        padding: 10px;
        margin: 6px 0;
        border-radius: 8px;
        border: 1px solid #4b2e19;
    }

    button {
        width: 100%;
        padding: 12px;
        background: #4b2e19;
        color: white;
        border: none;
        border-radius: 8px;
        margin-top: 12px;
        cursor: pointer;
        font-size: 16px;
    }
    button:hover { opacity: 0.8; }

    .cassette {
        width: 220px;
        display: block;
        margin: auto;
        transition: transform 0.4s ease-in-out;
    }

    .rotate {
        transform: rotate(20deg);
    }

    #result {
        margin-top: 25px;
        padding: 20px;
        background: #fff4e6;
        color: #4b2e19;
        border-radius: 12px;
    }

    iframe {
        width: 100%;
        height: 80px;
        border-radius: 10px;
        margin-top: 10px;
    }
</style>

</head>
<body>

<div class="container">
    <h1>🎵 Mixtape Retro 🎵</h1>

<script>
    // === Generate 10 Input otomatis ===
    let container = document.getElementById("inputs");
    for (let i = 1; i <= 10; i++) {
        container.innerHTML += `<input type="text" id="song${i}" placeholder="Link Lagu Spotify ${i}">`;
    }

    // === Load dari URL (share link) ===
    window.onload = () => {
        const params = new URLSearchParams(window.location.search);

        if (params.has("nama")) {
            document.getElementById("nama").value = params.get("nama");
        }

        for (let i = 1; i <= 10; i++) {
            if (params.has("s" + i)) {
                document.getElementById("song" + i).value = params.get("s" + i);
            }
        }

        if (params.has("nama")) generateMixtape();
    };

    // === Generate Mixtape ===
    function generateMixtape() {
        const cassette = document.getElementById("cassette");
        cassette.classList.add("rotate");

        setTimeout(() => cassette.classList.remove("rotate"), 800);

        let nama = document.getElementById("nama").value;
        let result = document.getElementById("result");

        result.innerHTML = `<h2>Mixtape Untuk: ${nama}</h2>`;

        for (let i = 1; i <= 10; i++) {
            let link = document.getElementById("song" + i).value.trim();

            if (link.includes("open.spotify.com")) {
                let embed = link.replace("open.spotify.com/track", "open.spotify.com/embed/track");

                result.innerHTML += `
                    <div>
                        <b>Lagu ${i}:</b>
                        <iframe src="${embed}" allow="encrypted-media"></iframe>
                    </div>
                `;
            }
        }

        updateURL();
    }

    // === Update URL untuk share ===
    function updateURL() {
        let params = new URLSearchParams();

        params.set("nama", document.getElementById("nama").value);

        for (let i = 1; i <= 10; i++) {
            let link = document.getElementById("song" + i).value.trim();
            if (link) params.set("s" + i, link);
        }

        let newURL = window.location.pathname + "?" + params.toString();
        history.replaceState({}, "", newURL);
    }

    // === Copy share link ===
    function copyShareLink() {
        navigator.clipboard.writeText(window.location.href);
        alert("Link mixtape berhasil disalin!");
    }
</script>

</body>
</html>
