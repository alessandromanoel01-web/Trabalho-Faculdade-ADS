# Trabalho-Faculdade-ADS
Cartão Pessoal 
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Alessandro | Cartão Pessoal</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background: #110927;
            overflow: hidden;
            position: relative;
            padding: 20px;
        }

        /* Fundo Interativo com Canvas */
        #bg-canvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
        }

        /* Cartão Central */
        .card {
            position: relative;
            z-index: 2;
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            width: 100%;
            max-width: 360px;
            padding: 36px 28px;
            border-radius: 24px;
            text-align: center;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.5), 0 0 30px rgba(124, 92, 255, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        .avatar {
            width: 90px;
            height: 90px;
            border-radius: 50%;
            object-fit: cover;
            margin-bottom: 16px;
            border: 3px solid #7c5cff;
            box-shadow: 0 0 15px rgba(124, 92, 255, 0.5);
        }

        h1 {
            font-size: 1.6rem;
            color: #12092b;
            font-weight: 700;
            margin-bottom: 4px;
        }

        .cargo {
            color: #7c5cff;
            font-size: 0.95rem;
            font-weight: 600;
            margin-bottom: 16px;
        }

        .bio {
            color: #4b5563;
            font-size: 0.88rem;
            line-height: 1.6;
            margin-bottom: 24px;
        }

        .btn-link {
            display: block;
            width: 100%;
            padding: 12px 24px;
            background-color: #12092b;
            color: #ffffff;
            text-decoration: none;
            border-radius: 12px;
            font-weight: 600;
            font-size: 0.95rem;
            transition: all 0.3s ease;
            box-shadow: 0 4px 12px rgba(18, 9, 43, 0.3);
        }

        .btn-link:hover {
            background-color: #7c5cff;
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(124, 92, 255, 0.5);
        }
    </style>
</head>
<body>

    <!-- Fundo de interação -->
    <canvas id="bg-canvas"></canvas>

    <!-- Cartão de Perfil -->
    <div class="card">
        <!-- Nomeie a foto da sua imagem para "0.jpg" e salve na mesma pasta do arquivo HTML -->

 <img src="IMG_20260131_123951_Original.jpeg" alt="Alessandro" class="avatar">
        
        <h1>Alessandro</h1>
        <p class="cargo">Desenvolvedor Frontend</p>
        
        <p class="bio">
            Olá, meu nome é Alessandro, sou desenvolvedor frontend e estou em busca de oportunidades para aplicar meus conhecimentos e contribuir para projetos inovadores.
        </p>

        <a href="https://www.linkedin.com/in/alessandro-manoel-363351277" target="_blank" class="btn-link">LinkedIn</a>
    </div>

    <script>
        const canvas = document.getElementById('bg-canvas');
        const ctx = canvas.getContext('2d');

        let width = canvas.width = window.innerWidth;
        let height = canvas.height = window.innerHeight;

        window.addEventListener('resize', () => {
            width = canvas.width = window.innerWidth;
            height = canvas.height = window.innerHeight;
        });

        const codeSnippets = [
            '<div class="card">',
            '.social-links {',
            '<img src="avatar.jpg" />',
            '<a href="#">',
            '#1b1035;',
            '</p>',
            'const dev = true;',
            'display: flex;',
            'border-radius: 50%;'
        ];

        const particles = [];
        const mouse = { x: width / 2, y: height / 2 };

        window.addEventListener('mousemove', (e) => {
            mouse.x = e.clientX;
            mouse.y = e.clientY;
        });

        class Particle {
            constructor() {
                this.x = Math.random() * width;
                this.y = Math.random() * height;
                this.size = Math.random() * 12 + 10;
                this.text = codeSnippets[Math.floor(Math.random() * codeSnippets.length)];
                this.speedX = (Math.random() - 0.5) * 0.8;
                this.speedY = (Math.random() - 0.5) * 0.8;
                this.opacity = Math.random() * 0.4 + 0.1;
            }

            update() {
                this.x += this.speedX;
                this.y += this.speedY;

                if (this.x < 0 || this.x > width) this.speedX *= -1;
                if (this.y < 0 || this.y > height) this.speedY *= -1;

                // Reação com a posição do mouse
                let dx = mouse.x - this.x;
                let dy = mouse.y - this.y;
                let distance = Math.sqrt(dx * dx + dy * dy);

                if (distance < 120) {
                    this.x -= dx * 0.02;
                    this.y -= dy * 0.02;
                }
            }

            draw() {
                ctx.fillStyle = `rgba(168, 140, 255, ${this.opacity})`;
                ctx.font = `${this.size}px monospace`;
                ctx.fillText(this.text, this.x, this.y);
            }
        }

        for (let i = 0; i < 35; i++) {
            particles.push(new Particle());
        }

        function animate() {
            ctx.clearRect(0, 0, width, height);

            // Gradiente suave no fundo
            let radialGlow = ctx.createRadialGradient(mouse.x, mouse.y, 10, mouse.x, mouse.y, 400);
            radialGlow.addColorStop(0, 'rgba(124, 92, 255, 0.15)');
            radialGlow.addColorStop(1, 'rgba(17, 9, 39, 0)');
            ctx.fillStyle = radialGlow;
            ctx.fillRect(0, 0, width, height);

            particles.forEach(p => {
                p.update();
                p.draw();
            });

            requestAnimationFrame(animate);
        }

        animate();
    </script>
</body>
</html>
http://127.0.0.1:5500/index.html
