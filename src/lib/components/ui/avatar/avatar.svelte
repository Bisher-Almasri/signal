<script lang="ts">
	let className = '';
	export { className as class };
</script>

<div class="relative flex h-10 w-10 shrink-0 overflow-hidden rounded-full {className}">
	<slot name="image">
		<slot name="fallback">
			<span
				class="flex h-full w-full items-center justify-center rounded-full bg-gray-500 text-white"
			>
				?
			</span>
		</slot>
	</slot>
</div>
