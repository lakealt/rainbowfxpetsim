<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rainbow Gradient - RainbowFX</title>
    <style>
        body {
            margin: 0;
            padding: 20px;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: #1a1a1a;
            color: white;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }

        .gradient-container {
            width: 420px;
            height: 420px;
            border-radius: 15px;
            position: relative;
            overflow: hidden;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
            margin-bottom: 30px;
            transition: all 0.3s ease;
        }

        /* Image 2 (Behind everything) */
        .background-image {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-size: cover;
            background-position: center;
            opacity: 0.5; /* Stays at 0.5 transparency */
            z-index: 1;
        }

        /* The Gradient Overlay */
        .gradient-layer {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(
                -25deg,
                rgb(255, 0, 0) 0%,
                rgb(255, 127, 0) 16.66%,
                rgb(255, 255, 0) 33.33%,
                rgb(0, 255, 0) 50%,
                rgb(0, 255, 255) 66.66%,
                rgb(0, 0, 255) 83.33%,
                rgb(255, 0, 255) 100%
            );
            opacity: 0.25; /* <-- FIX: Set gradient transparency to 0.25 */
            z-index: 3; /* Must be higher than front-image to be on top */
        }

        /* Image 1 (The main image) */
        .front-image {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-size: cover;
            background-position: center;
            opacity: 1; /* <-- FIX: Set Image1 transparency to 0 (fully visible) */
            z-index: 2; /* Sits below the gradient layer */
        }

        .upload-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            background: rgba(0, 0, 0, 0.8);
            border: 2px dashed #667eea;
            border-radius: 15px;
            z-index: 10;
            transition: all 0.3s ease;
            cursor: pointer;
        }

        .upload-overlay.dragover {
            background: rgba(102, 126, 234, 0.2);
            border-color: #4ecdc4;
        }

        .upload-overlay.hidden {
            display: none;
        }

        .upload-text {
            text-align: center;
            color: white;
            font-size: 18px;
            font-weight: 600;
        }

        .upload-subtext {
            color: #888;
            font-size: 14px;
            margin-top: 10px;
        }

        .controls {
            display: flex;
            flex-wrap: wrap; /* Allow buttons to wrap on smaller screens */
            justify-content: center;
            gap: 15px;
            margin-bottom: 30px;
        }

        .rainbow-btn {
            background: linear-gradient(45deg, #667eea, #764ba2);
            border: none;
            color: white;
            padding: 12px 25px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
            font-size: 16px;
        }

        .rainbow-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
        }

        .rainbow-btn.active {
            background: linear-gradient(45deg, #27ae60, #2ecc71);
        }

        .rainbow-btn.disabled, .rainbow-btn:disabled {
            background: #555;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
            opacity: 0.7;
        }

        .rainbow-btn.recording {
            background: linear-gradient(45deg, #e74c3c, #c0392b);
        }

        .info-panel {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 12px;
            padding: 25px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            max-width: 600px;
            width: 100%;
        }

        h1 {
            text-align: center;
            margin-bottom: 30px;
            background: linear-gradient(45deg, #ff6b6b, #4ecdc4, #45b7d1, #96ceb4, #feca57);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            font-size: 2.5em;
            font-weight: bold;
        }

        .property {
            display: flex;
            justify-content: space-between;
            margin: 10px 0;
            padding: 8px 12px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 6px;
        }

        .property-label {
            font-weight: 600;
            color: #ff6b6b;
        }

        .color-stops {
            margin-top: 20px;
        }

        .color-stop {
            display: flex;
            align-items: center;
            margin: 8px 0;
            padding: 8px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 6px;
        }

        .color-preview {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            margin-right: 15px;
            border: 2px solid rgba(255, 255, 255, 0.3);
        }

        .speed-control {
            margin-top: 20px;
            text-align: center;
        }

        .speed-slider {
            width: 100%;
            height: 8px;
            border-radius: 4px;
            background: linear-gradient(to right, #3498db, #e74c3c);
            outline: none;
            margin: 15px 0;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="header-comment">
            RainbowFX<br>
            by: @AngelGamingBTW<br>
        🐱❤️
    </div>

    <h1>RainbowFX Gradient</h1>
    
    <div class="gradient-container" id="gradientContainer">
        <div class="background-image" id="backgroundImage"></div>
        <div class="front-image" id="frontImage"></div>
        <div class="gradient-layer" id="gradientLayer"></div>
        <div class="upload-overlay" id="uploadOverlay">
            <div>
                <div class="upload-text">📸 Upload Image</div>
                <div class="upload-subtext">Drag & drop or click to select<br>Auto-crops to 420x420</div>
            </div>
        </div>
        <input type="file" id="fileInput" accept="image/*" style="display: none;">
    </div>
    
    <div class="controls">
        <button class="rainbow-btn" id="toggleBtn" onclick="toggleRainbow()">Start Rainbow FX</button>
        <button class="rainbow-btn" id="saveBtn" onclick="saveImage()">Save Photo</button>
        <!-- "Copy Lua Script" button removed from here -->
        <button class="rainbow-btn" onclick="clearImage()">Clear Image</button>
    </div>
    
    <div class="info-panel">
        <h3>Gradient Properties</h3>
        <div class="property">
            <span class="property-label">Rotation:</span>
            <span id="rotationDisplay">-25°</span>
        </div>
        <div class="property">
            <span class="property-label">Size:</span>
            <span>420x420px</span>
        </div>
        <div class="property">
            <span class="property-label">Transparency:</span>
            <span>0.25 (Gradient Overlay)</span>
        </div>
        <div class="property">
            <span class="property-label">Animation:</span>
            <span id="animationStatus">Disabled</span>
        </div>
        
        <div class="speed-control">
            <h4>Animation Speed</h4>
            <input type="range" class="speed-slider" id="speedSlider" min="10" max="200" value="50">
            <p>Speed: <span id="speedValue">0.05</span>s</p>
        </div>

        <div class="speed-control">
            <h4>Rotation Angle</h4>
            <input type="range" class="speed-slider" id="rotationSlider" min="-180" max="180" value="-25">
            <p>Angle: <span id="rotationValue">-25</span>°</p>
        </div>

        <div class="color-stops">
            <h4>Color Stops (RainbowFX Colors)</h4>
            <div class="color-stop"><div class="color-preview" style="background: rgb(255, 0, 0);"></div><span>Red (255, 0, 0)</span></div>
            <div class="color-stop"><div class="color-preview" style="background: rgb(255, 127, 0);"></div><span>Orange (255, 127, 0)</span></div>
            <div class="color-stop"><div class="color-preview" style="background: rgb(255, 255, 0);"></div><span>Yellow (255, 255, 0)</span></div>
            <div class="color-stop"><div class="color-preview" style="background: rgb(0, 255, 0);"></div><span>Green (0, 255, 0)</span></div>
            <div class="color-stop"><div class="color-preview" style="background: rgb(0, 255, 255);"></div><span>Cyan (0, 255, 255)</span></div>
            <div class="color-stop"><div class="color-preview" style="background: rgb(0, 0, 255);"></div><span>Blue (0, 0, 255)</span></div>
            <div class="color-stop"><div class="color-preview" style="background: rgb(255, 0, 255);"></div><span>Magenta (255, 0, 255)</span></div>
        </div>
    </div>

    <!-- External Libraries for Saving -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gif.js/0.2.0/gif.js"></script>
    
    <script>
        let rainbowActive = false;
        let animationFrame;
        let speed = 0.05;
        let rotation = -25;
        let uploadedImage = null;
        
        const colors = [
            {r: 255, g: 0, b: 0},     // Red
            {r: 255, g: 127, b: 0},   // Orange
            {r: 255, g: 255, b: 0},   // Yellow
            {r: 0, g: 255, b: 0},     // Green
            {r: 0, g: 255, b: 255},   // Cyan
            {r: 0, g: 0, b: 255},     // Blue
            {r: 255, g: 0, b: 255}    // Magenta
        ];

        const gradientContainer = document.getElementById('gradientContainer');
        const gradientLayer = document.getElementById('gradientLayer');
        const backgroundImage = document.getElementById('backgroundImage');
        const frontImage = document.getElementById('frontImage');
        const uploadOverlay = document.getElementById('uploadOverlay');
        const fileInput = document.getElementById('fileInput');
        const toggleBtn = document.getElementById('toggleBtn');
        const saveBtn = document.getElementById('saveBtn'); 
        const speedSlider = document.getElementById('speedSlider');
        const speedValue = document.getElementById('speedValue');
        const rotationSlider = document.getElementById('rotationSlider');
        const rotationValue = document.getElementById('rotationValue');
        const rotationDisplay = document.getElementById('rotationDisplay');
        const animationStatus = document.getElementById('animationStatus');

        // This function updates the gradient style based on a given time
        function updateGradient(time) {
            const tick = (time * speed * 0.001 % 3) / 3;
            let gradientStops = [];
            
            for (let i = 0; i < colors.length; i++) {
                let stopTime = tick + i / colors.length;
                if (stopTime > 1) stopTime -= 1;
                
                const color = colors[i];
                gradientStops.push({
                    time: stopTime,
                    color: `rgb(${color.r}, ${color.g}, ${color.b})`
                });
            }
            
            gradientStops.sort((a, b) => a.time - b.time);
            
            const gradientString = gradientStops.map(stop => 
                `${stop.color} ${(stop.time * 100).toFixed(1)}%`
            ).join(', ');
            
            gradientLayer.style.background = `linear-gradient(${rotation}deg, ${gradientString})`;
        }
        
        // This is the LIVE animation loop
        function animateRainbow() {
            updateGradient(Date.now());
            if (rainbowActive) {
                animationFrame = requestAnimationFrame(animateRainbow);
            }
        }
        
        uploadOverlay.addEventListener('click', () => fileInput.click());
        uploadOverlay.addEventListener('dragover', (e) => { e.preventDefault(); uploadOverlay.classList.add('dragover'); });
        uploadOverlay.addEventListener('dragleave', () => uploadOverlay.classList.remove('dragover'));
        uploadOverlay.addEventListener('drop', (e) => {
            e.preventDefault();
            uploadOverlay.classList.remove('dragover');
            if (e.dataTransfer.files.length > 0) handleImageUpload(e.dataTransfer.files[0]);
        });
        fileInput.addEventListener('change', (e) => {
            if (e.target.files.length > 0) handleImageUpload(e.target.files[0]);
        });

        function handleImageUpload(file) {
            if (!file.type.startsWith('image/')) {
                alert('Please upload an image file');
                return;
            }
            const reader = new FileReader();
            reader.onload = (e) => {
                const img = new Image();
                img.onload = () => {
                    const canvas = document.createElement('canvas');
                    const ctx = canvas.getContext('2d');
                    canvas.width = 420;
                    canvas.height = 420;
                    const scale = Math.max(420 / img.width, 420 / img.height);
                    const scaledWidth = img.width * scale;
                    const scaledHeight = img.height * scale;
                    const x = (420 - scaledWidth) / 2;
                    const y = (420 - scaledHeight) / 2;
                    ctx.drawImage(img, x, y, scaledWidth, scaledHeight);
                    const croppedImageUrl = canvas.toDataURL();
                    uploadedImage = croppedImageUrl;
                    backgroundImage.style.backgroundImage = `url(${croppedImageUrl})`;
                    frontImage.style.backgroundImage = `url(${croppedImageUrl})`;
                    uploadOverlay.classList.add('hidden');
                };
                img.src = e.target.result;
            };
            reader.readAsDataURL(file);
        }

        function clearImage() {
            uploadedImage = null;
            backgroundImage.style.backgroundImage = '';
            frontImage.style.backgroundImage = '';
            uploadOverlay.classList.remove('hidden');
            fileInput.value = '';
        }

        function toggleRainbow() {
            rainbowActive = !rainbowActive;
            
            if (rainbowActive) {
                toggleBtn.textContent = 'Stop Rainbow FX';
                toggleBtn.classList.add('active');
                animationStatus.textContent = 'Active';
                animateRainbow();
            } else {
                toggleBtn.textContent = 'Start Rainbow FX';
                toggleBtn.classList.remove('active');
                if (animationFrame) cancelAnimationFrame(animationFrame);
                // Reset to default gradient
                gradientLayer.style.background = `linear-gradient(${rotation}deg, rgb(255, 0, 0) 0%, rgb(255, 127, 0) 16.66%, rgb(255, 255, 0) 33.33%, rgb(0, 255, 0) 50%, rgb(0, 255, 255) 66.66%, rgb(0, 0, 255) 83.33%, rgb(255, 0, 255) 100%)`;
                animationStatus.textContent = 'Disabled';
            }
        }

        speedSlider.addEventListener('input', function() { speed = parseInt(this.value) / 1000; speedValue.textContent = speed.toFixed(3); });
        rotationSlider.addEventListener('input', function() {
            rotation = parseInt(this.value);
            rotationValue.textContent = rotation;
            rotationDisplay.textContent = rotation + '°';
            if (!rainbowActive) {
                gradientLayer.style.background = `linear-gradient(${rotation}deg, rgb(255, 0, 0) 0%, rgb(255, 127, 0) 16.66%, rgb(255, 255, 0) 33.33%, rgb(0, 255, 0) 50%, rgb(0, 255, 255) 66.66%, rgb(0, 0, 255) 83.33%, rgb(255, 0, 255) 100%)`;
            }
        });

        function saveImage() {
            if (!uploadedImage) {
                alert("Please upload an image first!");
                return;
            }
            if (rainbowActive) {
                saveAsGIF();
            } else {
                saveAsPNG();
            }
        }

        function saveAsPNG() {
            saveBtn.textContent = 'Saving...';
            saveBtn.disabled = true;
            html2canvas(gradientContainer, { useCORS: true }).then(canvas => {
                const link = document.createElement('a');
                link.download = 'rainbowfx-static.png';
                link.href = canvas.toDataURL('image/png');
                link.click();
            }).catch(err => {
                console.error("Error saving PNG:", err);
                alert("Sorry, an error occurred while saving the image.");
            }).finally(() => {
                saveBtn.textContent = 'Save Photo';
                saveBtn.disabled = false;
            });
        }

        // --- NEW AND IMPROVED saveAsGIF FUNCTION ---
        async function saveAsGIF() {
            // 1. Setup UI and stop the live animation to prevent conflicts
            saveBtn.textContent = '🔴 Recording...';
            saveBtn.classList.add('recording');
            saveBtn.disabled = true;
            if (animationFrame) {
                cancelAnimationFrame(animationFrame);
            }

            try {
                // 2. Setup the GIF encoder
                const gif = new GIF({
                    workers: 2,
                    quality: 10,
                    width: 420,
                    height: 420,
                    workerScript: 'https://cdnjs.cloudflare.com/ajax/libs/gif.js/0.2.0/gif.worker.js'
                });

                // 3. Define recording parameters
                const frameRate = 15; // Capture 15 frames per second
                const duration = 3000; // Record for 3 seconds (one full loop)
                const totalFrames = frameRate * (duration / 1000);
                const delay = 1000 / frameRate;
                const startTime = Date.now();

                // 4. Sequentially capture frames using a loop with "await"
                for (let i = 0; i < totalFrames; i++) {
                    const currentTime = startTime + (i * delay);
                    updateGradient(currentTime);

                    // Wait for html2canvas to finish before proceeding
                    const canvas = await html2canvas(gradientContainer, { useCORS: true });
                    gif.addFrame(canvas, { delay: delay });
                }

                // 5. Render the GIF and wait for it to finish
                // We "promisify" the 'finished' event
                const onFinished = new Promise(resolve => {
                    gif.on('finished', blob => resolve(blob));
                });
                
                gif.render();
                
                const blob = await onFinished;
                
                // 6. Create download link
                const link = document.createElement('a');
                link.download = 'rainbowfx-animated.gif';
                link.href = URL.createObjectURL(blob);
                link.click();
                URL.revokeObjectURL(link.href);

            } catch (error) {
                console.error("Failed to create GIF:", error);
                alert("Sorry, an error occurred while recording the GIF. Please try again.");
            } finally {
                // 7. ALWAYS clean up the UI and restart the animation
                saveBtn.textContent = 'Save Photo';
                saveBtn.classList.remove('recording');
                saveBtn.disabled = false;
                if (rainbowActive) {
                    animateRainbow();
                }
            }
        }

        // --- END OF SAVE FUNCTIONALITY ---

        speedValue.textContent = speed.toFixed(3);
        rotationValue.textContent = rotation;
        rotationDisplay.textContent = rotation + '°';
    </script>
</body>
</html>
