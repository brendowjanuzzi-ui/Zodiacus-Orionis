<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Zodiacus Orionis - Galáxia Completa</title>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body, html {
            margin: 0; padding: 0; width: 100%; height: 100%;
            overflow: hidden; background-color: #000002;
            font-family: 'Cinzel', serif; color: white;
        }
        canvas { display: block; }
        #overlay {
            position: absolute; bottom: 20px; width: 100%;
            text-align: center; pointer-events: none;
            letter-spacing: 5px; font-size: 10px; opacity: 0.4;
        }
    </style>
</head>
<body>

    <div id="overlay">MAPA CELESTE ATIVO // 12 CONSTELAÇÕES // SISTEMA SOLAR</div>
    <canvas id="canvas"></canvas>

    <script>
        const canvas = document.getElementById('canvas');
        const ctx = canvas.getContext('2d');

        let width, height, backgroundStars = [], zodiacPoints = [], planets = [], mouse = { x: null, y: null, r: 130 };

        // Configuração de Todos os 12 Signos (Silhuetas e Cores)
        const zodiacConfig = [
            { name: 'Áries', col: '#FF5733', p: [{x:0.1, y:0.2}, {x:0.15, y:0.15}, {x:0.2, y:0.22}, {x:0.15, y:0.28}] },
            { name: 'Touro', col: '#FFC300', p: [{x:0.3, y:0.1}, {x:0.35, y:0.15}, {x:0.32, y:0.22}, {x:0.28, y:0.18}] },
            { name: 'Gêmeos', col: '#DAF7A6', p: [{x:0.5, y:0.1}, {x:0.55, y:0.1}, {x:0.55, y:0.25}, {x:0.5, y:0.25}] },
            { name: 'Câncer', col: '#FFC0CB', p: [{x:0.7, y:0.15}, {x:0.75, y:0.2}, {x:0.7, y:0.3}] },
            { name: 'Leão', col: '#FF8D33', p: [{x:0.85, y:0.2}, {x:0.9, y:0.3}, {x:0.85, y:0.4}, {x:0.8, y:0.3}] },
            { name: 'Virgem', col: '#8E44AD', p: [{x:0.1, y:0.5}, {x:0.15, y:0.45}, {x:0.2, y:0.5}, {x:0.15, y:0.6}] },
            { name: 'Libra', col: '#3498DB', p: [{x:0.1, y:0.8}, {x:0.2, y:0.75}, {x:0.15, y:0.9}] },
            { name: 'Escorpião', col: '#C70039', p: [{x:0.35, y:0.85}, {x:0.4, y:0.75}, {x:0.45, y:0.85}, {x:0.4, y:0.95}] },
            { name: 'Sagitário', col: '#27AE60', p: [{x:0.6, y:0.8}, {x:0.65, y:0.7}, {x:0.7, y:0.8}, {x:0.65, y:0.9}] },
            { name: 'Capricórnio', col: '#5D6D7E', p: [{x:0.85, y:0.8}, {x:0.9, y:0.75}, {x:0.95, y:0.85}] },
            { name: 'Aquário', col: '#4df3ff', p: [{x:0.8, y:0.6}, {x:0.85, y:0.55}, {x:0.9, y:0.65}, {x:0.85, y:0.7}] },
            { name: 'Peixes', col: '#F4D03F', p: [{x:0.5, y:0.5}, {x:0.55, y:0.45}, {x:0.6, y:0.55}, {x:0.55, y:0.6}] }
        ];

        function init() {
            width = canvas.width = window.innerWidth;
            height = canvas.height = window.innerHeight;

            // 1. Constelação de Fundo (Estrelas Fixas / Nebulosa)
            backgroundStars = [];
            for (let i = 0; i < 600; i++) {
                backgroundStars.push({
                    x: Math.random() * width,
                    y: Math.random() * height,
                    size: Math.random() * 1.2,
                    o: Math.random()
                });
            }

            // 2. Pontos dos Signos com Drift
            zodiacPoints = [];
            zodiacConfig.forEach(config => {
                config.p.forEach((pos, i) => {
                    zodiacPoints.push({
                        id: `${config.name}_${i}`,
                        parent: config.name,
                        color: config.col,
                        x: pos.x * width,
                        y: pos.y * height,
                        vx: (Math.random() - 0.5) * 0.25,
                        vy: (Math.random() - 0.5) * 0.25
                    });
                });
            });

            // 3. Sistema Planetário
            planets = [
                { dist: 120, spd: 0.008, size: 4, col: '#4df3ff', a: 0 },
                { dist: 180, spd: 0.005, size: 7, col: '#ff4d4d', a: 2 },
                { dist: 250, spd: 0.003, size: 10, col: '#dfbdff', a: 4 }
            ];
        }

        function draw() {
            ctx.fillStyle = '#000002';
            ctx.fillRect(0, 0, width, height);

            // Desenhar Estrelas de Fundo (Nebulosa)
            backgroundStars.forEach(s => {
                ctx.fillStyle = `rgba(255, 255, 255, ${s.o})`;
                ctx.beginPath(); ctx.arc(s.x, s.y, s.size, 0, Math.PI*2); ctx.fill();
            });

            const cx = width / 2, cy = height / 2;

            // Desenhar Signos (Silhuetas e Glow)
            zodiacConfig.forEach(config => {
                const pts = zodiacPoints.filter(p => p.parent === config.name);
                let hover = false;
                pts.forEach(p => {
                    if (mouse.x && Math.hypot(p.x - mouse.x, p.y - mouse.y) < mouse.r) hover = true;
                    // Movimento de Drift
                    p.x += p.vx; p.y += p.vy;
                    if (p.x < 0 || p.x > width) p.vx *= -1;
                    if (p.y < 0 || p.y > height) p.vy *= -1;
                });

                if (pts.length > 0) {
                    ctx.beginPath();
                    ctx.moveTo(pts[0].x, pts[0].y);
                    pts.forEach(p => ctx.lineTo(p.x, p.y));
                    ctx.closePath();

                    if (hover) {
                        ctx.fillStyle = config.col + '33'; ctx.fill();
                        ctx.strokeStyle = config.col; ctx.lineWidth = 2;
                        ctx.shadowBlur = 15; ctx.shadowColor = config.col;
                        ctx.fillStyle = "white"; ctx.font = "14px Cinzel";
                        ctx.fillText(config.name, pts[0].x, pts[0].y - 10);
                    } else {
                        ctx.strokeStyle = 'rgba(255,255,255,0.1)'; ctx.lineWidth = 0.5;
                        ctx.shadowBlur = 0;
                    }
                    ctx.stroke();
                }

                // Desenhar as estrelas do signo
                pts.forEach(p => {
                    ctx.fillStyle = "white";
                    ctx.beginPath(); ctx.arc(p.x, p.y, 2, 0, Math.PI*2); ctx.fill();
                });
            });

            // Sol Central
            ctx.shadowBlur = 30; ctx.shadowColor = 'orange';
            ctx.fillStyle = 'orange'; ctx.beginPath(); ctx.arc(cx, cy, 20, 0, Math.PI*2); ctx.fill();
            ctx.shadowBlur = 0;

            // Planetas
            planets.forEach(p => {
                p.a += p.spd;
                const px = cx + Math.cos(p.a) * p.dist;
                const py = cy + Math.sin(p.a) * p.dist * 0.5;
                ctx.strokeStyle = 'rgba(255,255,255,0.05)';
                ctx.beginPath(); ctx.ellipse(cx, cy, p.dist, p.dist * 0.5, 0, 0, Math.PI*2); ctx.stroke();
                ctx.fillStyle = p.col; ctx.beginPath(); ctx.arc(px, py, p.size, 0, Math.PI*2); ctx.fill();
            });

            requestAnimationFrame(draw);
        }

        window.addEventListener('resize', init);
        window.addEventListener('mousemove', e => { mouse.x = e.clientX; mouse.y = e.clientY; });
        init(); draw();
    </script>
</body>
</html>
