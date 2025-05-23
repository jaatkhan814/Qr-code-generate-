<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>QR Code Generator</title>
  <style>
    /* Centered, simple, modern layout */
    body, html {
      height: 100%;
      margin: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: #f9f9f9;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    .container {
      background: white;
      padding: 2rem;
      border-radius: 8px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      text-align: center;
      width: 320px;
    }
    input[type="text"] {
      width: 100%;
      padding: 0.5rem;
      font-size: 1rem;
      border: 1px solid #ccc;
      border-radius: 4px;
      margin-bottom: 1rem;
      box-sizing: border-box;
    }
    button {
      background-color: #0078d7;
      color: white;
      border: none;
      padding: 0.6rem 1.2rem;
      font-size: 1rem;
      border-radius: 4px;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }
    button:hover {
      background-color: #005ea1;
    }
    #qrcode {
      margin-top: 1rem;
    }
    #downloadBtn {
      margin-top: 1rem;
      display: none;
      background-color: #28a745;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>QR Code Generator</h2>
    <input type="text" id="text" placeholder="Enter URL or text" />
    <button id="generateBtn">Generate QR Code</button>
    <div id="qrcode"></div>
    <a id="downloadBtn" href="#" download="qrcode.png">Download QR Code</a>
  </div>

  <!-- QRCode.js library -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
  <script>
    const generateBtn = document.getElementById('generateBtn');
    const qrcodeContainer = document.getElementById('qrcode');
    const downloadBtn = document.getElementById('downloadBtn');
    const input = document.getElementById('text');
    let qr;

    generateBtn.addEventListener('click', () => {
      const text = input.value.trim();
      if (!text) {
        alert('Please enter a valid URL or text');
        return;
      }

      // Clear previous QR code if any
      qrcodeContainer.innerHTML = '';
      downloadBtn.style.display = 'none';

      // Generate QR code
      qr = new QRCode(qrcodeContainer, {
        text: text,
        width: 200,
        height: 200,
        colorDark: '#000000',
        colorLight: '#ffffff',
        correctLevel: QRCode.CorrectLevel.H
      });

      // Wait for QR code image to render, then enable download
      setTimeout(() => {
        const img = qrcodeContainer.querySelector('img');
        if (img) {
          downloadBtn.href = img.src;
          downloadBtn.style.display = 'inline-block';
        } else {
          // fallback for canvas
          const canvas = qrcodeContainer.querySelector('canvas');
          if (canvas) {
            downloadBtn.href = canvas.toDataURL('image/png');
            downloadBtn.style.display = 'inline-block';
          }
        }
      }, 300);
    });
  </script>
</body>
</html>

