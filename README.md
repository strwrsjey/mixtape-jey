<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Mixtape Retro</title>

<style>
    body {
        font-family: Arial;
        background-color: #4b2e19;
        color: white;
        margin: 0;
        padding: 20px;
    }

    .container {
        background-color: #d7b899;
        color: #4b2e19;
        max-width: 850px;
        margin: auto;
        padding: 25px;
        border-radius: 20px;
        box-shadow: 0 0 20px rgba(0,0,0,0.4);
    }

    h1, h2 { text-align: center; }

    input {
        width: 100%;
        padding: 10px;
        margin: 7px 0;
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
        cursor: pointer;
        margin-top: 20px;
    }
    
    button:hover { background: #3b2414; }

    iframe { width: 100%; border-radius: 12px; }

    .cassette {
        width: 250px;
        margin: 20px auto;
        display: block;
        animation: float 2s ease-in-out infinite;
    }

    /* Animasi kaset naik turun */
    @keyframes float {
        0% { transform: translateY(0px); }
        50% { transform: translateY(-10px); }
        100% { transform: translateY(0px); }
    }

    #shareBtn {
        background-color: #2a180c;
        margin-top: 10px;
        display: none;
    }
</style>
</head>
<body>

<div class="container">
    <h1>Mixtape Retro</h1>

    <img src="https://i.imgur.com/rRnJQVL.png" class="cassette" id="cassette">

    <label>Nama Penerima:</label>
    <input type="text" id="nama">

    <h2>10 Link Lagu Spotify</h2>
    <div id="inputs"></div>

    <button onclick="generateMixtape()">Buat Mixtape</button>
    <button id="shareBtn" onclick="copyShareLink()">Copy Link Mixtape</button>

    <div id="result"></div>
</div>

<!-- Suara klik + hiss kaset -->
<audio id="cassetteSound">
    <source src="https://cdn.pixabay.com/download/audio/2021/08/04/audio_f2a4c1c778.mp3?filename=tape-start-103306.mp3" type="audio/mpeg">
</audio>

<script>
    // Generate 10 input fields
    const inputsDiv = document.getElementById("inputs");
    for (let i = 1; i <= 10; i++) {
        inputsDiv.innerHTML += `<input id="link${i}" placeholder="Link Spotify Lagu ${i}">`;
    }

    // Load data dari URL otomatis
    const params = new URLSearchParams(window.location.search);

    if (params.has("nama")) {
        document.getElementById("nama").value = params.get("nama");

        for (let i = 1; i <= 10; i++) {
            if (params.get("song" + i)) {
                document.getElementById("link" + i).value = params.get("song" + i);
            }
        }

        // Auto Generate
        setTimeout(generateMixtape, 300);
    }

    function generateMixtape() {
        document.getElementById("shareBtn").style.display = "block";

        const nama = document.getElementById("nama").value;
        const cassetteSound = document.getElementById("cassetteSound");
        cassetteSound.play();

        let html = `<h2 style="text-align:center;">Mixtape untuk ${nama}</h2>`;

        for (let i = 1; i <= 10; i++) {
            let url = document.getElementById("link" + i).value.trim();
            if (url === "") continue;
            let embed = url.replace("open.spotify.com", "open.spotify.com/embed");
            html += `<iframe src="${embed}" height="152" allow="encrypted-media"></iframe>`;
        }

        document.getElementById("result").innerHTML = html;

        // Update URL (shareable)
        let params = new URLSearchParams();
        params.set("nama", nama);

        for (let i = 1; i <= 10; i++) {
            let url = document.getElementById("link" + i).value.trim();
            if (url !== "") params.set("song" + i, url);
        }

        history.replaceState({}, "", "?" + params.toString());
    }

    function copyShareLink() {
        navigator.clipboard.writeText(window.location.href);
        alert("Link mixtape berhasil dicopy!");
    }
</script>

</body>
</html>
