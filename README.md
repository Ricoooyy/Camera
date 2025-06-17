<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Web Camera App</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@600&display=swap" rel="stylesheet" />
  <style>
    :root {
      --color-bg: #ffffff;
      --color-text: #374151;
      --color-text-muted: #6b7280;
      --color-card-bg: #f9fafb;
      --color-primary: #111827;
      --shadow-default: 0 2px 10px rgb(0 0 0 / 0.05);
      --border-radius: 0.75rem;
      --transition-fast: 0.25s ease-in-out;
      --font-family: 'Poppins', sans-serif;
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: var(--font-family);
      background-color: var(--color-bg);
      color: var(--color-text);
      line-height: 1.6;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 2rem 1rem 4rem;
    }

    header {
      width: 100%;
      max-width: 1200px;
      display: flex;
      align-items: center;
      justify-content: center;
      margin-bottom: 2rem;
    }

    h1 {
      font-weight: 600;
      font-size: 3rem;
      margin: 0;
      text-align: center;
      user-select: none;
    }

    main {
      width: 100%;
      max-width: 600px;
      display: flex;
      flex-direction: column;
      gap: 2rem;
    }

    .card {
      background: var(--color-card-bg);
      box-shadow: var(--shadow-default);
      border-radius: var(--border-radius);
      padding: 1.5rem;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    video {
      border-radius: var(--border-radius);
      max-width: 100%;
      box-shadow: var(--shadow-default);
      background-color: #000;
    }

    img.captured-photo {
      border-radius: var(--border-radius);
      max-width: 100%;
      box-shadow: var(--shadow-default);
      margin-top: 1rem;
      display: block;
    }

    button {
      margin-top: 1.5rem;
      background-color: var(--color-primary);
      color: #fff;
      border: none;
      padding: 0.75rem 2rem;
      font-weight: 600;
      font-size: 1.125rem;
      border-radius: var(--border-radius);
      box-shadow: var(--shadow-default);
      cursor: pointer;
      transition: transform var(--transition-fast), background-color var(--transition-fast);
      user-select: none;
    }

    button:hover,
    button:focus {
      background-color: #1f2937;
      transform: scale(1.05);
      outline: none;
    }

    button:active {
      transform: scale(0.98);
    }

    @media (max-width: 640px) {
      h1 {
        font-size: 2.25rem;
      }
    }
  </style>
</head>
<body>
  <header>
    <h1>Web Camera App</h1>
  </header>
  <main>
    <section class="card" aria-label="Camera preview section">
      <video id="video" autoplay playsinline muted></video>
      <button id="snap" type="button" aria-label="Take snapshot">Take Snapshot</button>
    </section>
    <section class="card" aria-label="Captured photo section">
      <img id="photo" class="captured-photo" alt="Captured photo will appear here" />
    </section>
  </main>
  <script>
    const video = document.getElementById('video');
    const snapBtn = document.getElementById('snap');
    const photo = document.getElementById('photo');

    // Get user media stream from webcam
    async function setupCamera() {
      try {
        const stream = await navigator.mediaDevices.getUserMedia({ video: true });
        video.srcObject = stream;
        await video.play();
      } catch (err) {
        alert('Error accessing the camera: ' + err.message);
      }
    }

    function takeSnapshot() {
      if (!video.srcObject) {
        alert('Camera not initialized');
        return;
      }
      const canvas = document.createElement('canvas');
      canvas.width = video.videoWidth;
      canvas.height = video.videoHeight;
      const ctx = canvas.getContext('2d');
      ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
      const dataURL = canvas.toDataURL('image/png');
      photo.src = dataURL;
    }

    snapBtn.addEventListener('click', takeSnapshot);
    setupCamera();
  </script>
</body>
</html>



