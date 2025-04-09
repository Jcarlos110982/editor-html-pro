<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Editor HTML + App Android</title>
    <style>
        :root {
            --primary: #4285f4;
            --secondary: #34a853;
            --danger: #ea4335;
            --light: #f8f9fa;
            --dark: #343a40;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f5f7fa;
            color: #333;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        header {
            text-align: center;
            margin-bottom: 30px;
            padding: 20px 0;
            background: linear-gradient(135deg, var(--primary), #2a75f3);
            color: white;
            border-radius: 0 0 20px 20px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }
        
        h1 {
            margin: 0;
            font-size: 2.2rem;
        }
        
        .subtitle {
            font-size: 1.1rem;
            opacity: 0.9;
            margin-top: 10px;
        }
        
        .app-download {
            background-color: white;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 30px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
            text-align: center;
        }
        
        .download-btn {
            display: inline-flex;
            align-items: center;
            background-color: var(--secondary);
            color: white;
            padding: 12px 25px;
            border-radius: 50px;
            text-decoration: none;
            font-weight: bold;
            margin-top: 15px;
            transition: all 0.3s;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        
        .download-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 12px rgba(0,0,0,0.15);
            background-color: #2d9246;
        }
        
        .download-btn i {
            margin-right: 10px;
            font-size: 1.2rem;
        }
        
        .editor-container {
            display: flex;
            flex-direction: column;
            gap: 25px;
        }
        
        .panel {
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
            overflow: hidden;
        }
        
        .panel-header {
            background-color: var(--light);
            padding: 15px 20px;
            border-bottom: 1px solid #eee;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .panel-title {
            font-weight: 600;
            color: var(--dark);
        }
        
        .filename {
            font-size: 0.9rem;
            color: #666;
        }
        
        .controls {
            display: flex;
            gap: 10px;
        }
        
        .btn {
            padding: 8px 15px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-weight: 500;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .btn i {
            font-size: 0.9rem;
        }
        
        .btn-primary {
            background-color: var(--primary);
            color: white;
        }
        
        .btn-primary:hover {
            background-color: #3367d6;
        }
        
        .btn-success {
            background-color: var(--secondary);
            color: white;
        }
        
        .btn-success:hover {
            background-color: #2d9246;
        }
        
        .btn-light {
            background-color: var(--light);
            color: var(--dark);
        }
        
        .btn-light:hover {
            background-color: #e2e6ea;
        }
        
        #htmlEditor {
            width: 100%;
            min-height: 300px;
            padding: 15px;
            border: none;
            resize: vertical;
            font-family: 'Courier New', Courier, monospace;
            font-size: 14px;
            line-height: 1.5;
        }
        
        #previewFrame {
            width: 100%;
            min-height: 400px;
            border: none;
            background-color: white;
        }
        
        .file-input-wrapper {
            position: relative;
            display: inline-block;
        }
        
        .file-input {
            position: absolute;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            opacity: 0;
            cursor: pointer;
        }
        
        @media (max-width: 768px) {
            .controls {
                flex-direction: column;
            }
            
            #htmlEditor {
                min-height: 200px;
            }
            
            #previewFrame {
                min-height: 300px;
            }
        }
    </style>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body>
    <header>
        <div class="container">
            <h1>Editor HTML Pro</h1>
            <p class="subtitle">Edita, visualiza y exporta tus proyectos HTML</p>
        </div>
    </header>
    
    <div class="container">
        <!-- Sección de descarga de la app Android -->
        <div class="app-download">
            <h2>Descarga nuestra aplicación para Android</h2>
            <p>Lleva tu editor HTML siempre contigo</p>
            <a href="#" id="androidDownload" class="download-btn">
                <i class="fab fa-android"></i> Descargar APK
            </a>
            <div style="margin-top: 15px; font-size: 0.9rem; color: #666;">
                Versión 1.0.0 | Tamaño: 15MB
            </div>
        </div>
        
        <!-- Editor HTML -->
        <div class="editor-container">
            <div class="panel">
                <div class="panel-header">
                    <div>
                        <span class="panel-title">Editor HTML</span>
                        <span id="filenameDisplay" class="filename">Ningún archivo seleccionado</span>
                    </div>
                    <div class="controls">
                        <div class="file-input-wrapper">
                            <button class="btn btn-light">
                                <i class="fas fa-folder-open"></i> Abrir
                            </button>
                            <input type="file" id="fileInput" class="file-input" accept=".html,.htm">
                        </div>
                        <button id="downloadHtmlBtn" class="btn btn-success" disabled>
                            <i class="fas fa-download"></i> Exportar HTML
                        </button>
                    </div>
                </div>
                <textarea id="htmlEditor" placeholder="Escribe tu código HTML aquí..."></textarea>
            </div>
            
            <div class="panel">
                <div class="panel-header">
                    <span class="panel-title">Vista Previa</span>
                </div>
                <iframe id="previewFrame" sandbox="allow-scripts allow-same-origin"></iframe>
            </div>
        </div>
    </div>
    
    <script>
        // Elementos del DOM
        const fileInput = document.getElementById('fileInput');
        const htmlEditor = document.getElementById('htmlEditor');
        const previewFrame = document.getElementById('previewFrame');
        const downloadHtmlBtn = document.getElementById('downloadHtmlBtn');
        const filenameDisplay = document.getElementById('filenameDisplay');
        const androidDownloadBtn = document.getElementById('androidDownload');
        
        // Variables de estado
        let currentFilename = 'documento.html';
        
        // URL de descarga de la app Android (reemplazar con tu enlace real)
        const ANDROID_APP_URL = 'https://tudominio.com/app/app-release.apk';
        
        // Inicialización
        init();
        
        function init() {
            setupEventListeners();
            loadWelcomeContent();
        }
        
        function setupEventListeners() {
            // Selección de archivo
            fileInput.addEventListener('change', handleFileSelect);
            
            // Edición de contenido
            htmlEditor.addEventListener('input', function() {
                updatePreview(this.value);
                downloadHtmlBtn.disabled = this.value.trim() === '';
            });
            
            // Descarga de HTML
            downloadHtmlBtn.addEventListener('click', downloadHtmlFile);
            
            // Descarga de app Android
            androidDownloadBtn.addEventListener('click', function(e) {
                e.preventDefault();
                downloadAndroidApp();
            });
        }
        
        function handleFileSelect(e) {
            if (e.target.files && e.target.files.length > 0) {
                const file = e.target.files[0];
                currentFilename = file.name;
                filenameDisplay.textContent = file.name;
                
                const reader = new FileReader();
                reader.onload = function(e) {
                    htmlEditor.value = e.target.result;
                    updatePreview(e.target.result);
                    downloadHtmlBtn.disabled = false;
                };
                reader.readAsText(file);
            }
        }
        
        function updatePreview(content) {
            try {
                const previewDoc = previewFrame.contentDocument || previewFrame.contentWindow.document;
                previewDoc.open();
                previewDoc.write(ensureCompleteHtml(content));
                previewDoc.close();
                
                setTimeout(() => {
                    previewFrame.style.height = previewDoc.body.scrollHeight + 'px';
                }, 100);
            } catch (error) {
                console.error("Error al actualizar vista previa:", error);
            }
        }
        
        function ensureCompleteHtml(content) {
            if (!content.includes('<html') && !content.includes('<HTML')) {
                return `<!DOCTYPE html>
                <html>
                    <head>
                        <meta charset="UTF-8">
                        <meta name="viewport" content="width=device-width, initial-scale=1.0">
                        <title>${currentFilename.replace('.html', '').replace('.htm', '')}</title>
                    </head>
                    <body>${content}</body>
                </html>`;
            }
            return content;
        }
        
        function downloadHtmlFile() {
            const content = htmlEditor.value;
            const blob = new Blob([ensureCompleteHtml(content)], { type: 'text/html' });
            const url = URL.createObjectURL(blob);
            
            const a = document.createElement('a');
            a.href = url;
            a.download = currentFilename;
            document.body.appendChild(a);
            a.click();
            
            setTimeout(() => {
                document.body.removeChild(a);
                URL.revokeObjectURL(url);
            }, 100);
        }
        
        function downloadAndroidApp() {
            // Simulación de descarga (reemplazar con tu lógica real)
            console.log("Iniciando descarga de la app Android...");
            
            // Esto es un ejemplo - reemplaza con tu APK real
            const link = document.createElement('a');
            link.href = ANDROID_APP_URL;
            link.download = 'EditorHTMLPro.apk';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            // Mensaje alternativo si no hay APK configurado
            if (ANDROID_APP_URL === '#') {
                alert('Por favor, configura la URL de descarga de tu aplicación Android en el código.');
            }
        }
        
        function loadWelcomeContent() {
            const welcomeHtml = `
                <!DOCTYPE html>
                <html>
                    <head>
                        <meta charset="UTF-8">
                        <style>
                            body {
                                font-family: Arial, sans-serif;
                                padding: 30px;
                                line-height: 1.6;
                                color: #333;
                            }
                            .welcome-container {
                                max-width: 600px;
                                margin: 0 auto;
                                text-align: center;
                            }
                            .welcome-title {
                                color: #4285f4;
                                margin-bottom: 20px;
                            }
                        </style>
                    </head>
                    <body>
                        <div class="welcome-container">
                            <h1 class="welcome-title">¡Bienvenido al Editor HTML Pro!</h1>
                            <p>Selecciona un archivo HTML o comienza a escribir tu código.</p>
                            <p>Los cambios se mostrarán en tiempo real en la vista previa.</p>
                            <p>Descarga nuestra app Android para llevar el editor contigo.</p>
                        </div>
                    </body>
                </html>
            `;
            updatePreview(welcomeHtml);
        }
    </script>
</body>
</html>
