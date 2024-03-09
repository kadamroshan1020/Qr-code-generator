<!DOCTYPE html>
<html lang="en">
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QR Code Generator</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <p>Enter your text or URL</p>
        <input type="text" placeholder="Text or URL" id="qrText">

        <div id="imgbox">
            <img src="" id="qrImage">
        </div>

        <button onclick="generateQR()">Generate QR Code</button>
        
        <!-- Added ID to the Download button -->
        <button onclick="downloadQR()" id="downloadBtn">Download QR Code</button>
    </div>

    <script>
        let imgbox = document.getElementById("imgbox");
        let qrImage = document.getElementById("qrImage");
        let qrText = document.getElementById("qrText");
        let downloadBtn = document.getElementById("downloadBtn"); // Reference to the Download button

        function generateQR() {
            if(qrText.value.length > 0) {
                qrImage.src = "https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=" + encodeURIComponent(qrText.value);
                imgbox.classList.add("show-img");
                downloadBtn.style.display = "block"; // Show the Download button
            }
        }

        function downloadQR() {
            if(qrImage.src.length > 0) {
                var image = new Image();
                image.crossOrigin = "Anonymous";

                image.onload = function() {
                    var canvas = document.createElement('canvas');
                    canvas.width = image.width;
                    canvas.height = image.height;

                    var ctx = canvas.getContext('2d');
                    ctx.drawImage(image, 0, 0);

                    canvas.toBlob(function(blob) {
                        var downloadLink = document.createElement('a');
                        downloadLink.href = URL.createObjectURL(blob);
                        downloadLink.download = 'qrcode.png';
                        document.body.appendChild(downloadLink);
                        downloadLink.click();
                        document.body.removeChild(downloadLink);
                    }, 'image/png');
                };

                image.src = qrImage.src;
            }
        }
    </script>
</body>
</html>

