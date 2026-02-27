<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>ASCII ART CONSTRUCTER</title>
<style>
body {
    background: black;
    color: white;
    font-family: monospace;
}
#output {
    white-space: pre;
    line-height: 6px;
    font-size: 6px;
}
</style>
</head>
<body>

<h2>이미지 아스키 아트 변환</h2>
<input type="file" id="fileInput">

<pre id="output"></pre>

<canvas id="canvas" style="display:none;"></canvas>

<script>
const asciiChars = "@%#*+=-:. "; // 진한 → 연한

document.getElementById("fileInput").addEventListener("change", function(e) {

    const file = e.target.files[0];
    const img = new Image();
    img.src = URL.createObjectURL(file);

    img.onload = function() {

        const canvas = document.getElementById("canvas");
        const ctx = canvas.getContext("2d");

        const width = 120;
        const height = img.height * (width / img.width);

        canvas.width = width;
        canvas.height = height;

        ctx.drawImage(img, 0, 0, width, height);

        const imageData = ctx.getImageData(0, 0, width, height).data;

        let ascii = "";

        for (let y = 0; y < height; y++) {

            for (let x = 0; x < width; x++) {

                const i = (y * width + x) * 4;

                const r = imageData[i];
                const g = imageData[i+1];
                const b = imageData[i+2];

                const brightness = (r + g + b) / 3;

                const charIndex = Math.floor(
                    brightness / 255 * (asciiChars.length - 1)
                );

                ascii += asciiChars[charIndex];
            }

            ascii += "\n";
        }

        document.getElementById("output").textContent = ascii;
    };

});
</script>

</body>
</html>
