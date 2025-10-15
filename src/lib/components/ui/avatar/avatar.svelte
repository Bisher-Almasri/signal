<script lang="ts">
	export let src: string | undefined = undefined;
	export let alt: string = '';
	export let fallback: string = '?';
	let className = '';
	export { className as class };

	let imageError = false;

	function handleImageLoad() {
		imageError = false;
	}

	function handleImageError() {
		imageError = true;
	}
</script>

<div class="relative flex h-10 w-10 shrink-0 overflow-hidden rounded-full {className}">
	{#if src && !imageError}
		<img
			{src}
			{alt}
			class="aspect-square h-full w-full object-cover"
			on:load={handleImageLoad}
			on:error={handleImageError}
		/>
	{:else}
		<span
			class="flex h-full w-full items-center justify-center rounded-full bg-[#3A76F0] font-medium text-white"
		>
			{fallback}
		</span>
	{/if}
</div>
