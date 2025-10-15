<script lang="ts">
	import { fly } from 'svelte/transition';
	import { Avatar } from './avatar';

	export let message: string;
	export let timestamp: string;
	export let isOwn: boolean;
	export let avatar: string | undefined = undefined;
	export let name: string | undefined = undefined;
	export let index: number;
</script>

<div
	in:fly={{ y: 20, duration: 300, delay: index * 50 }}
	class="mb-4 flex gap-2 {isOwn ? 'flex-row-reverse' : 'flex-row'}"
>
	{#if !isOwn}
		<Avatar src={avatar} alt={name} fallback={name?.charAt(0) || '?'} class="mt-auto h-8 w-8" />
	{/if}

	<div class="max-w-[70%] {isOwn ? 'items-end' : 'items-start'} flex flex-col">
		<div
			class="rounded-2xl px-4 py-2 {isOwn
				? 'rounded-br-md bg-[#2C6BED] text-white'
				: 'rounded-bl-md bg-[#2D2D2F] text-white'}"
		>
			<p class="break-words">{message}</p>
		</div>
		<span class="mt-1 px-2 text-xs text-gray-500">{timestamp}</span>
	</div>
</div>
