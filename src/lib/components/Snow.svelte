<script lang="ts">
    import { onMount } from 'svelte';

    let {snowflakes = 200} = $props(); // quantidade de flocos

    let canvasEl: HTMLCanvasElement;
    let ctx: CanvasRenderingContext2D;
    let flakes: { x: number; y: number; r: number; d: number }[] = [];

    onMount(() => {
        ctx = canvasEl.getContext('2d');
        resizeCanvas();
        window.addEventListener('resize', resizeCanvas);

        // Inicializar flocos de neve
        for (let i = 0; i < snowflakes; i++) {
            flakes.push({
                x: Math.random() * canvasEl.width,
                y: Math.random() * canvasEl.height,
                r: 1 + Math.random() * 4, // raio
                d: Math.random() * snowflakes // densidade
            });
        }

        animate();
    });

    function resizeCanvas() {
        canvasEl.width = window.innerWidth;
        canvasEl.height = window.innerHeight;
    }

    let angle = 0;

    function animate() {
        ctx.clearRect(0, 0, canvasEl.width, canvasEl.height);
        ctx.fillStyle = 'white';
        ctx.beginPath();

        for (let i = 0; i < flakes.length; i++) {
            const f = flakes[i];
            ctx.moveTo(f.x, f.y);
            ctx.arc(f.x, f.y, f.r, 0, Math.PI * 2, true);
        }

        ctx.fill();

        update();
        requestAnimationFrame(animate);
    }

    function update() {
        angle += 0.01;
        for (let i = 0; i < flakes.length; i++) {
            const f = flakes[i];
            f.y += Math.cos(angle + f.d) + 1 + f.r / 2;
            f.x += Math.sin(angle) * 2;

            if (f.x > canvasEl.width + 5 || f.x < -5 || f.y > canvasEl.height) {
                f.x = Math.random() * canvasEl.width;
                f.y = -10;
            }
        }
    }
</script>

<style>
    canvas {
        position: fixed;
        top: 0;
        left: 0;
        pointer-events: none;
        z-index: 50;
    }
</style>

<canvas bind:this={canvasEl}></canvas>
