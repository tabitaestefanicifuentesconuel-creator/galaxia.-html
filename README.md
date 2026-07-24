<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Galaxia a 3 seg / Destellos</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body {
            height: 100vh;
            overflow: hidden;
            background: radial-gradient(circle at center, #1a0a2e 0%, #000000 70%);
        }
        canvas {
            display: block;
            width: 100%;
            height: 100%;
        }
    </style>
</head>
<body>
    <canvas id="galaxia"></canvas>

    <script>
        const lienzo = document.getElementById('galaxia');
        const ctx = lienzo.getContext('2d');

        // Ajustar tamaño automático
        function ajustarLienzo() {
            lienzo.width = window.innerWidth;
            lienzo.height = window.innerHeight;
        }
        ajustarLienzo();
        window.addEventListener('resize', ajustarLienzo);

        // ⚙️ CONFIGURACIÓN EXACTA:
        const config = {
            cantidad: 4000,
            radioTotal: Math.min(lienzo.width, lienzo.height) / 2.5,
            poderConcentracion: 2.5,
            // ⏱️ Una vuelta completa EXACTA cada 5 segundos:
            velocidad: (Math.PI * 2) / 3000,
            // ✨ Colores con destello brillante:
            colores: ['#ffffff', '#ffffcc', '#ffcc80', '#ff9800', '#ffd700', '#ff8a80', '#b39ddb', '#80d8ff']
        };

        // Crear estrellas con efecto de destello
        const estrellas = [];
        for(let i = 0; i < config.cantidad; i++){
            const angulo = Math.random() * Math.PI * 2;
            const distancia = Math.pow(Math.random(), config.poderConcentracion) * config.radioTotal;
            estrellas.push({
                angulo: angulo,
                distancia: distancia,
                tamano: Math.random() * 1.5 + 0.2,
                color: config.colores[Math.floor(Math.random() * config.colores.length)],
                giroExtra: (Math.random() - 0.5) * 0.0008,
                parpadeo: Math.random() * Math.PI * 2,
                velocidadParpadeo: Math.random() * 0.15 + 0.05
            });
        }

        // Animación
        function dibujar(){
            // Fondo suave para rastro
            ctx.fillStyle = 'rgba(0, 0, 0, 0.1)';
            ctx.fillRect(0, 0, lienzo.width, lienzo.height);

            const centroX = lienzo.width / 2;
            const centroY = lienzo.height / 2;

            // Dibujar cada estrella con destello
            estrellas.forEach(estrella => {
                estrella.angulo += config.velocidad + estrella.giroExtra;
                estrella.parpadeo += estrella.velocidadParpadeo;

                const x = centroX + Math.cos(estrella.angulo) * estrella.distancia;
                const y = centroY + Math.sin(estrella.angulo) * estrella.distancia;

                // ✨ Efecto de destello (brillo variable)
                const brillo = 0.4 + Math.abs(Math.sin(estrella.parpadeo)) * 0.6;

                ctx.beginPath();
                ctx.arc(x, y, estrella.tamano, 0, Math.PI * 2);
                ctx.globalAlpha = brillo;
                ctx.fillStyle = estrella.color;
                ctx.shadowBlur = 8 + brillo * 12;
                ctx.shadowColor = estrella.color;
                ctx.fill();
                ctx.globalAlpha = 1;
            });

            requestAnimationFrame(dibujar);
        }

        dibujar();
    </script>
</body>
</html>
