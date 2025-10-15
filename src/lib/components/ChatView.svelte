<script lang="ts">
	import { createEventDispatcher } from 'svelte';
	import { Avatar } from './ui/avatar';
	import { Button } from './ui/button';
	import { Input } from './ui/input';
	import MessageBubble from './ui/MessageBubble.svelte';
	import WaveLengthTransition from './ui/WaveLengthTransition.svelte';
	import Phone from 'lucide-svelte/icons/phone';
	import Video from 'lucide-svelte/icons/video';
	import MoreVertical from 'lucide-svelte/icons/more-vertical';
	import Send from 'lucide-svelte/icons/send';
	import MessageSquarePlus from 'lucide-svelte/icons/message-square-plus';

	interface Message {
		id: number;
		message: string;
		timestamp: string;
		isOwn: boolean;
		avatar?: string;
		name?: string;
	}

	interface Chat {
		id: number;
		name: string;
		avatar: string;
		lastMessage: string;
		timestamp: string;
		unread: number;
		isGroup?: boolean;
		members?: number;
		messages: Message[];
	}

	export let chat: Chat | null = null;
	export let showWaveTransition: boolean = false;

	const dispatch = createEventDispatcher<{
		sendMessage: string;
	}>();

	let newMessage = '';

	function handleSendMessage() {
		if (!newMessage.trim() || !chat) return;

		dispatch('sendMessage', newMessage.trim());
		newMessage = '';
	}

	function handleKeyPress(event: KeyboardEvent) {
		if (event.key === 'Enter' && !event.shiftKey) {
			event.preventDefault();
			handleSendMessage();
		}
	}
</script>

<div class="flex flex-1 flex-col">
	{#if chat}
		<div class="flex items-center justify-between border-b border-[#2D2D2F] bg-[#1B1B1D] p-4">
			<div class="flex items-center gap-3">
				<Avatar
					src={chat.avatar}
					alt={chat.name}
					fallback={chat.name.charAt(0)}
					class="h-10 w-10"
				/>
				<div>
					<h2 class="font-semibold text-white">{chat.name}</h2>
					{#if chat.isGroup && chat.members}
						<p class="text-sm text-gray-400">{chat.members} members</p>
					{:else}
						<p class="text-sm text-gray-400">Online</p>
					{/if}
				</div>
			</div>
			<div class="flex gap-2">
				<Button variant="ghost" size="icon" class="text-white hover:bg-[#2C6BED]">
					<Phone class="h-5 w-5" />
				</Button>
				<Button variant="ghost" size="icon" class="text-white hover:bg-[#2C6BED]">
					<Video class="h-5 w-5" />
				</Button>
				<Button variant="ghost" size="icon" class="text-white hover:bg-[#2C6BED]">
					<MoreVertical class="h-5 w-5" />
				</Button>
			</div>
		</div>

		<div class="relative flex-1 overflow-y-auto bg-[#0D1117] p-4">
			{#if showWaveTransition}
				<WaveLengthTransition />
			{/if}

			<div class="relative z-10 mx-auto max-w-4xl">
				{#each chat.messages as message, index (message.id)}
					<MessageBubble
						message={message.message}
						timestamp={message.timestamp}
						isOwn={message.isOwn}
						avatar={message.avatar}
						name={message.name}
						{index}
					/>
				{/each}
			</div>
		</div>

		<div class="border-t border-[#2D2D2F] bg-[#1B1B1D] p-4">
			<div class="mx-auto flex max-w-4xl items-end gap-3">
				<div class="relative flex-1">
					<Input
						bind:value={newMessage}
						onkeydown={handleKeyPress}
						placeholder="Type a message..."
						class="border-none bg-[#2D2D2F] pr-12 text-white placeholder:text-gray-500 focus-visible:ring-[#2C6BED]"
					/>
				</div>
				<Button
					onclick={handleSendMessage}
					disabled={!newMessage.trim()}
					class="bg-[#2C6BED] text-white hover:bg-[#2558CC] disabled:cursor-not-allowed disabled:opacity-50"
				>
					<Send class="h-4 w-4" />
				</Button>
			</div>
		</div>
	{:else}
		<div class="flex flex-1 items-center justify-center bg-[#0D1117]">
			<div class="text-center">
				<div
					class="mx-auto mb-4 flex h-24 w-24 items-center justify-center rounded-full bg-[#2D2D2F]"
				>
					<MessageSquarePlus class="h-12 w-12 text-gray-400" />
				</div>
				<h2 class="mb-2 text-xl text-white">Select a chat to start messaging</h2>
				<p class="text-gray-400">Choose from your existing conversations or start a new one</p>
			</div>
		</div>
	{/if}
</div>
