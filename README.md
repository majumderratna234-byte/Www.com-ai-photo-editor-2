# Www.com-ai-photo-editor-2
Her you can edite photo using ai.
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Photo Editor</title>

    <style>
        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background: #101114;
            color: white;
            text-align: center;
        }

        h1 {
            margin: 20px;
        }

        .editor {
            max-width: 900px;
            margin: auto;
            padding: 15px;
        }

        canvas {
            width: 100%;
            max-height: 500px;
            object-fit: contain;
            background: #222;
            border-radius: 15px;
            margin-top: 15px;
        }

        .tools {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            justify-content: center;
            margin: 20px 0;
        }

        button, input {
            padding: 12px;
            border-radius: 10px;
            border: none;
        }

        button {
            background: #6c5ce7;
            color: white;
            cursor: pointer;
            font-weight: bold;
        }

        button:hover {
            background: #5746c7;
        }

        .tool {
            background: #1d1f25;
            padding: 12px;
            border-radius: 12px;
        }

        label {
            display: block;
            margin-bottom: 5px;
        }

        input[type="range"] {
            width: 150px;
        }

        .ai-tools {
            margin-top: 25px;
            padding: 15px;
            background: #191b20;
            border-radius: 15px;
        }

        #status {
            margin: 10px;
            color: #aaa;
        }
    </style>
</head>

<body>

<div class="editor">

    <h1>🤖 AI Photo Editor</h1>

    <input type="file" id="upload" accept="image/*">

    <canvas id="canvas"></canvas>

    <div id="status">Upload a photo to start editing.</div>

    <div class="tools">

        <div class="tool">
            <label>Brightness</label>
            <input type="range" id="brightness"
                   min="0" max="200" value="100">
        </div>

        <div class="tool">
            <label>Contrast</label>
            <input type="range" id="contrast"
                   min="0" max="200" value="100">
        </div>

        <div class="tool">
            <label>Saturation</label>
            <input type="range" id="saturation"
                   min="0" max="200" value="100">
        </div>

        <div class="tool">
            <label>Blur</label>
            <input type="range" id="blur"
                   min="0" max="10" value="0">
        </div>

    </div>


    <div class="tools">

        <button onclick="blackWhite()">
            🖤 Black & White
        </button>

        <button onclick="sepia()">
            🟤 Sepia
        </button>

        <button onclick="autoEnhance()">
            ✨ AI Enhance
        </button>

        <button onclick="resetImage()">
            🔄 Reset
        </button>

        <button onclick="downloadImage()">
            💾 Download
        </button>

    </div>


    <div class="ai-tools">

        <h2>🤖 AI Editing Tools</h2>

        <button onclick="aiTool('Remove Background')">
            ✂️ Remove Background
        </button>

        <button onclick="aiTool('Remove Object')">
            🧹 Remove Object
        </button>

        <button onclick="aiTool('Face Enhance')">
            😊 Face Enhance
        </button>

        <button onclick="aiTool('Upscale')">
            🔍 AI Upscale
        </button>

        <button onclick="aiTool('Change Background')">
            🌄 Change Background
        </button>

        <button onclick="aiTool('AI Image Generator')">
            🎨 AI Generate
        </button>

    </div>

</div>


<script>

const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

const upload = document.getElementById("upload");

let image = new Image();
let originalImage = null;


upload.addEventListener("change", function(event) {

    const file = event.target.files[0];

    if (!file) return;

    const reader = new FileReader();

    reader.onload = function(e) {

        image.onload = function() {

            canvas.width = image.width;
            canvas.height = image.height;

            originalImage = ctx.getImageData(
                0,
                0,
                canvas.width,
                canvas.height
            );

            drawImage();

            document.getElementById("status").innerText =
                "Photo loaded successfully!";

        };

        image.src = e.target.result;
    };

    reader.readAsDataURL(file);
});


function drawImage() {

    if (!image.src) return;

    ctx.filter =
        `brightness(${brightness.value}%)
         contrast(${contrast.value}%)
         saturate(${saturation.value}%)
         blur(${blur.value}px)`;

    ctx.drawImage(
        image,
        0,
        0,
        canvas.width,
        canvas.height
    );

    ctx.filter = "none";
}


brightness.addEventListener("input", drawImage);
contrast.addEventListener("input", drawImage);
saturation.addEventListener("input", drawImage);
blur.addEventListener("input", drawImage);


function blackWhite() {

    if (!originalImage) return;

    ctx.putImageData(originalImage, 0, 0);

    let data = ctx.getImageData(
        0,
        0,
        canvas.width,
        canvas.height
    );

    for (let i = 0; i < data.data.length; i += 4) {

        let r = data.data[i];
        let g = data.data[i + 1];
        let b = data.data[i + 2];

        let gray = (r + g + b) / 3;

        data.data[i] = gray;
        data.data[i + 1] = gray;
        data.data[i + 2] = gray;
    }

    ctx.putImageData(data, 0, 0);
}


function sepia() {

    if (!originalImage) return;

    ctx.putImageData(originalImage, 0, 0);

    let data = ctx.getImageData(
        0,
        0,
        canvas.width,
        canvas.height
    );

    for (let i = 0; i < data.data.length; i += 4) {

        let r = data.data[i];
        let g = data.data[i + 1];
        let b = data.data[i + 2];

        data.data[i] =
            Math.min(255, r * 0.393 + g * 0.769 + b * 0.189);

        data.data[i + 1] =
            Math.min(255, r * 0.349 + g * 0.686 + b * 0.168);

        data.data[i + 2] =
            Math.min(255, r * 0.272 + g * 0.534 + b * 0.131);
    }

    ctx.putImageData(data, 0, 0);
}


function autoEnhance() {

    brightness.value = 110;
    contrast.value = 115;
    saturation.value = 115;

    drawImage();

    document.getElementById("status").innerText =
        "✨ AI-style enhancement applied!";
}


function resetImage() {

    if (!originalImage) return;

    brightness.value = 100;
    contrast.value = 100;
    saturation.value = 100;
    blur.value = 0;

    ctx.putImageData(originalImage, 0, 0);

    document.getElementById("status").innerText =
        "Image reset.";
}


function aiTool(tool) {

    document.getElementById("status").innerText =
        tool + " selected.";

    alert(
        tool +
        " requires an AI image-processing API/backend. " +
        "The button is ready to connect to one."
    );
}


function downloadImage() {

    if (!image.src) return;

    const link = document.createElement("a");

    link.download = "ai-edited-photo.png";

    link.href = canvas.toDataURL("image/png");

    link.click();
}

</script>

</body>
</html>
