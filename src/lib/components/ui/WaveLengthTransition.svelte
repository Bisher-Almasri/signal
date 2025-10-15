<!-- ts so vibe coded thanks chat gpt :) -->

<script lang="ts">
	import { onMount } from 'svelte';
	import { fade } from 'svelte/transition';

	let mounted = false;
	let waveElements: (SVGPathElement | null)[] = [];

	const waveCount = 5;

	onMount(() => {
		mounted = true;

		setTimeout(() => {
			waveElements.forEach((element, i) => {
				if (element) {
					animateWavePath(element, i);
				}
			});
		}, 100);
	});

	function animateWavePath(pathElement: SVGPathElement, index: number) {
		const duration = 1200;
		const delay = index * 80;

		let startTime: number | null = null;

		function animate(currentTime: number) {
			if (!startTime) startTime = currentTime - delay;
			const elapsed = currentTime - startTime;

			if (elapsed < 0) {
				requestAnimationFrame(animate);
				return;
			}

			const progress = Math.min(elapsed / duration, 1);

			const easeInOutCubic = (t: number) =>
				t < 0.5 ? 4 * t * t * t : (t - 1) * (2 * t - 2) * (2 * t - 2) + 1;
			const easedProgress = easeInOutCubic(progress);

			const time = easedProgress * Math.PI * 2;
			const baseAmplitude = 8;
			const waveOffset = index * 0.8;

			const points = [];
			for (let x = 0; x <= 100; x += 5) {
				const normalizedX = x / 100;

				const primaryWave = Math.sin(time + normalizedX * Math.PI * 2 + waveOffset) * baseAmplitude;

				const secondaryWave =
					Math.sin(time * 1.5 + normalizedX * Math.PI * 3 + waveOffset) * (baseAmplitude * 0.3);

				const envelope = Math.sin(easedProgress * Math.PI) * (1 - index * 0.1);

				const y = 50 + (primaryWave + secondaryWave) * envelope;
				points.push({ x, y });
			}

			let pathData = `M${points[0].x},${points[0].y}`;

			for (let i = 1; i < points.length - 1; i++) {
				const current = points[i];
				const next = points[i + 1];
				const controlX = current.x + (next.x - current.x) * 0.5;
				const controlY = current.y;

				pathData += ` Q${controlX},${controlY} ${next.x},${next.y}`;
			}

			pathData += ` L100,100 L0,100 Z`;

			pathElement.setAttribute('d', pathData);

			if (progress < 1) {
				requestAnimationFrame(animate);
			}
		}

		requestAnimationFrame(animate);
	}

	function getInitialWavePath(index: number): string {
		const baseY = 50;
		const amplitude = index * 2;
		const phase = index * 0.5;

		const y1 = baseY + Math.sin(phase) * amplitude;
		const y2 = baseY + Math.sin(phase + Math.PI / 3) * amplitude;
		const y3 = baseY + Math.sin(phase + (2 * Math.PI) / 3) * amplitude;
		const y4 = baseY + Math.sin(phase + Math.PI) * amplitude;

		return `M0,${y1} Q25,${y2} 50,${y3} Q75,${y4} 100,${y1} L100,100 L0,100 Z`;
	}
</script>

<div class="pointer-events-none absolute inset-0 overflow-hidden">
	{#if mounted}
		{#each Array(waveCount) as _, i (i)}
			<div
				class="wave-container absolute inset-0"
				style="--delay: {i * 80}ms;"
				in:fade={{ duration: 1500, delay: i * 80, easing: (t) => t * t * (3 - 2 * t) }}
			>
				<svg class="h-full w-full" viewBox="0 0 100 100" preserveAspectRatio="none">
					<path
						bind:this={waveElements[i]}
						class="wave-path"
						d={getInitialWavePath(i)}
						fill="rgba(44, 107, 237, {Math.max(0.05, 0.15 - i * 0.02)})"
						style="--wave-index: {i};"
					/>
				</svg>
			</div>
		{/each}
	{/if}
</div>

<style>
	.wave-container {
		animation: waveOpacity 1.5s cubic-bezier(0.4, 0, 0.2, 1) var(--delay, 0ms) forwards;
	}

	.wave-path {
		transform-origin: center center;
		animation: waveMovement 1.5s cubic-bezier(0.25, 0.46, 0.45, 0.94) var(--delay, 0ms) forwards;
		filter: blur(0.5px);
		transition: all 0.1s ease-out;
	}

	@keyframes waveOpacity {
		0% {
			opacity: 0;
			transform: scale(0.8);
		}
		15% {
			opacity: 0.8;
			transform: scale(1);
		}
		85% {
			opacity: 0.8;
			transform: scale(1);
		}
		100% {
			opacity: 0;
			transform: scale(1.1);
		}
	}

	@keyframes waveMovement {
		0% {
			transform: translateY(2px) scaleY(0.95) scaleX(0.98);
		}
		20% {
			transform: translateY(-1px) scaleY(1.02) scaleX(1.01);
		}
		40% {
			transform: translateY(0px) scaleY(0.98) scaleX(1.02);
		}
		60% {
			transform: translateY(1px) scaleY(1.01) scaleX(0.99);
		}
		80% {
			transform: translateY(-0.5px) scaleY(0.99) scaleX(1.01);
		}
		100% {
			transform: translateY(0px) scaleY(1) scaleX(1);
		}
	}

	.wave-container:nth-child(1) .wave-path {
		animation-delay: 0ms;
		animation-duration: 1.5s;
	}

	.wave-container:nth-child(2) .wave-path {
		animation-delay: 80ms;
		animation-duration: 1.6s;
	}

	.wave-container:nth-child(3) .wave-path {
		animation-delay: 160ms;
		animation-duration: 1.4s;
	}

	.wave-container:nth-child(4) .wave-path {
		animation-delay: 240ms;
		animation-duration: 1.7s;
	}

	.wave-container:nth-child(5) .wave-path {
		animation-delay: 320ms;
		animation-duration: 1.5s;
	}
</style>
