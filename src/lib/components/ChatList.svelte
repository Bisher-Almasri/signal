<script lang="ts">
	import { fly, scale } from 'svelte/transition';
	import Settings from 'lucide-svelte/icons/settings';
	import Search from 'lucide-svelte/icons/search';
	import Users from 'lucide-svelte/icons/users';
	import MessageSquarePlus from 'lucide-svelte/icons/message-square-plus';
	import UsersRound from 'lucide-svelte/icons/users-round';
	import { Button } from './ui/button';
	import { Input } from './ui/input';
	import { ScrollArea } from './ui/scroll-area';
	import { Avatar } from './ui/avatar';

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

	import { createEventDispatcher } from 'svelte';

	export let chats: Chat[];
	export let activeChat: number;

	const dispatch = createEventDispatcher<{
		selectChat: number;
		openSettings: void;
		newMessage: void;
		newGroup: void;
	}>();

	let searchQuery = '';

	$: filteredChats = chats.filter((chat) => {
		const query = searchQuery.toLowerCase();
		const nameMatch = chat.name.toLowerCase().includes(query);
		const messageMatch = chat.messages.some((msg) => msg.message.toLowerCase().includes(query));
		return nameMatch || messageMatch;
	});
</script>

<div class="flex h-full flex-col border-r border-[#2D2D2F] bg-[#1B1B1D]">
	<!-- Header -->
	<div class="border-b border-[#2D2D2F] p-4">
		<div class="mb-3 flex items-center justify-between">
			<h1 class="text-white">Signal</h1>
			<div class="flex gap-2">
				<div class="hover-scale">
					<Button
						onclick={() => dispatch('newMessage')}
						variant="ghost"
						size="icon"
						class="text-white hover:bg-[#2C6BED]"
					>
						<MessageSquarePlus class="h-5 w-5" />
					</Button>
				</div>
				<div class="hover-scale">
					<Button
						onclick={() => dispatch('newGroup')}
						variant="ghost"
						size="icon"
						class="text-white hover:bg-[#2C6BED]"
					>
						<UsersRound class="h-5 w-5" />
					</Button>
				</div>
			</div>
		</div>
		<div class="relative">
			<Search class="absolute top-1/2 left-3 h-4 w-4 -translate-y-1/2 transform text-gray-500" />
			<Input
				bind:value={searchQuery}
				placeholder="Search conversations & messages..."
				class="border-none bg-[#2D2D2F] pl-10 text-white placeholder:text-gray-500 focus-visible:ring-[#2C6BED]"
			/>
		</div>
	</div>

	<!-- Chat List -->
	<ScrollArea class="flex-1">
		<div class="p-2">
			{#each filteredChats as chat, index (chat.id)}
				<div class="chat-item hover-scale" in:fly={{ x: -20, duration: 300, delay: index * 50 }}>
					<button
						onclick={() => dispatch('selectChat', chat.id)}
						class="flex w-full items-center gap-3 rounded-lg p-3 transition-colors {activeChat ===
						chat.id
							? 'bg-[#2C6BED]'
							: 'hover:bg-[#2D2D2F]'}"
					>
						<div class="relative">
							<Avatar
								src={chat.avatar}
								alt={chat.name}
								fallback={chat.name.charAt(0)}
								class="h-12 w-12"
							/>
							{#if chat.isGroup}
								<div class="absolute -right-1 -bottom-1 rounded-full bg-[#1B1B1D] p-0.5">
									<Users class="h-3 w-3 text-[#2C6BED]" />
								</div>
							{/if}
						</div>
						<div class="flex-1 overflow-hidden text-left">
							<div class="mb-1 flex items-center justify-between">
								<div class="flex items-center gap-2">
									<span class="text-white">{chat.name}</span>
									{#if chat.isGroup && chat.members}
										<span class="text-xs text-gray-500">({chat.members})</span>
									{/if}
								</div>
								<span class="text-xs text-gray-400">{chat.timestamp}</span>
							</div>
							<div class="flex items-center justify-between">
								<p class="flex-1 truncate text-sm text-gray-400">
									{chat.lastMessage}
								</p>
								{#if chat.unread > 0}
									<span
										class="ml-2 flex h-5 min-w-[20px] items-center justify-center rounded-full bg-[#2C6BED] px-1.5 text-xs text-white"
										in:scale={{ duration: 200 }}
									>
										{chat.unread}
									</span>
								{/if}
							</div>
						</div>
					</button>
				</div>
			{/each}
		</div>
	</ScrollArea>

	<div class="border-t border-[#2D2D2F] p-3">
		<div class="hover-scale">
			<Button
				onclick={() => dispatch('openSettings')}
				variant="ghost"
				class="h-auto w-full justify-start gap-3 p-3 text-white hover:bg-[#2D2D2F]"
			>
				<Avatar
					src="https://api.dicebear.com/7.x/avataaars/svg?seed=You"
					alt="You"
					fallback="Y"
					class="h-10 w-10"
				/>
				<div class="flex-1 text-left">
					<div class="text-white">Your Account</div>
					<div class="text-xs text-gray-400">View settings</div>
				</div>
				<Settings class="h-5 w-5 text-gray-400" />
			</Button>
		</div>
	</div>
</div>

<style>
	.hover-scale {
		transition: transform 0.2s ease;
	}

	.hover-scale:hover {
		transform: scale(1.1);
	}

	.hover-scale:active {
		transform: scale(0.9);
	}

	.chat-item:hover {
		transform: scale(1.02);
	}

	.chat-item:active {
		transform: scale(0.98);
	}

	.chat-item {
		transition: transform 0.2s ease;
	}
</style>
