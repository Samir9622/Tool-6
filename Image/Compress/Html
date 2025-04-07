<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Image Compression Tool</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <header>
    <h1><a href="../index.html">Tool</a></h1>
    <nav>
      <ul>
        <li><a href="../index.html">Home</a></li>
        <li><a href="#">Tools</a></li>
      </ul>
    </nav>
  </header>

  <main>
    <h2>Image Compression Tool</h2>
    <p>Reduce image size without losing quality</p>

    <section class="upload-section">
      <h3>Compress Your Images</h3>
      <div class="drop-zone" id="drop-zone">
        <span>⬆️<br>Drag & Drop Your Images Here</span>
        <p>Or</p>
        <input type="file" id="file-input" multiple accept="image/*"/>
        <button onclick="document.getElementById('file-input').click()">Select Files</button>
      </div>
      <p id="file-count">Selected Images (0)</p>
      <button onclick="clearFiles()">Clear Selection</button>
    </section>

    <section class="settings">
      <h3>Compression Settings</h3>
      <label>Compression Level: <span id="quality-label">80%</span></label>
      <input type="range" id="quality" min="10" max="100" value="80"/>
      
      <label>Output Format</label>
      <select id="format">
        <option value="jpeg">JPEG</option>
        <option value="png">PNG</option>
        <option value="webp">WebP</option>
      </select>
      
      <label>
        <input type="checkbox" id="keep-dimensions" checked/> Maintain original dimensions
      </label>
      
      <button id="compress-btn" onclick="compressImages()">Compress Images</button>
    </section>

    <section class="results">
      <h3>Compression Results</h3>
      <div id="preview-container"></div>
      <button id="download-all" onclick="downloadAll()">Download All</button>
    </section>

    <section class="info">
      <h4>Fast Processing</h4>
      <p>Our tool compresses your images in seconds using advanced algorithms to reduce file size efficiently.</p>

      <h4>Secure & Private</h4>
      <p>All image processing happens in your browser. Your files never leave your device, ensuring complete privacy.</p>
    </section>
  </main>

  <script src="script.js"></script>
</body>
</html>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
  background: #f9f9f9;
  color: #333;
}

header {
  background-color: #ffffff;
  border-bottom: 1px solid #ddd;
  padding: 1em;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

header h1 a {
  text-decoration: none;
  color: #007bff;
}

header nav ul {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
}

header nav li {
  margin-left: 1em;
}

main {
  padding: 2em;
}

h2, h3, h4 {
  color: #222;
}

.drop-zone {
  border: 2px dashed #aaa;
  border-radius: 10px;
  padding: 2em;
  text-align: center;
  margin-bottom: 1em;
  background: #fff;
}

.drop-zone span {
  display: block;
  font-size: 1.2em;
  margin-bottom: 0.5em;
}

input[type="file"] {
  display: none;
}

button {
  padding: 0.5em 1em;
  margin-top: 0.5em;
  cursor: pointer;
  background: #007bff;
  border: none;
  color: white;
  border-radius: 5px;
}

button:hover {
  background: #0056b3;
}

.settings, .results, .info {
  background: #fff;
  padding: 1em;
  border-radius: 10px;
  margin-top: 2em;
  box-shadow: 0 2px 5px rgba(0,0,0,0.05);
}

#preview-container img {
  max-width: 100px;
  margin: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

input[type="range"] {
  width: 100%;
}

select {
  padding: 0.3em;
  margin-top: 0.5em;
}
let selectedFiles = [];

const dropZone = document.getElementById("drop-zone");
const fileInput = document.getElementById("file-input");
const previewContainer = document.getElementById("preview-container");
const qualitySlider = document.getElementById("quality");
const qualityLabel = document.getElementById("quality-label");

dropZone.addEventListener("dragover", (e) => {
  e.preventDefault();
  dropZone.style.background = "#eef";
});

dropZone.addEventListener("dragleave", () => {
  dropZone.style.background = "#fff";
});

dropZone.addEventListener("drop", (e) => {
  e.preventDefault();
  dropZone.style.background = "#fff";
  handleFiles(e.dataTransfer.files);
});

fileInput.addEventListener("change", () => {
  handleFiles(fileInput.files);
});

function handleFiles(files) {
  selectedFiles = Array.from(files);
  document.getElementById("file-count").textContent = `Selected Images (${selectedFiles.length})`;
  previewContainer.innerHTML = "";
}

function clearFiles() {
  selectedFiles = [];
  fileInput.value = "";
  document.getElementById("file-count").textContent = "Selected Images (0)";
  previewContainer.innerHTML = "";
}

qualitySlider.addEventListener("input", () => {
  qualityLabel.textContent = `${qualitySlider.value}%`;
});

function compressImages() {
  if (selectedFiles.length === 0) return;

  const quality = qualitySlider.value;
  const format = document.getElementById("format").value;
  const keepDimensions = document.getElementById("keep-dimensions").checked;

  previewContainer.innerHTML = "";

  selectedFiles.forEach(file => {
    const reader = new FileReader();
    reader.onload = function (event) {
      const img = new Image();
      img.onload = function () {
        const canvas = document.createElement("canvas");
        const ctx = canvas.getContext("2d");

        canvas.width = keepDimensions ? img.width : img.width * 0.8;
        canvas.height = keepDimensions ? img.height : img.height * 0.8;
        ctx.drawImage(img, 0, 0, canvas.width, canvas.height);

        canvas.toBlob(blob => {
          const url = URL.createObjectURL(blob);
          const previewImg = document.createElement("img");
          previewImg.src = url;
          previewContainer.appendChild(previewImg);

          previewImg.setAttribute("download-url", url);
          previewImg.setAttribute("filename", `compressed.${format}`);
        }, `image/${format}`, quality / 100);
      };
      img.src = event.target.result;
    };
    reader.readAsDataURL(file);
  });
}

function downloadAll() {
  const imgs = previewContainer.querySelectorAll("img");
  imgs.forEach((img, index) => {
    const a = document.createElement("a");
    a.href = img.getAttribute("download-url");
    a.download = img.getAttribute("filename") || `compressed-${index + 1}.jpg`;
    a.click();
  });
}
