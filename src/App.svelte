<script lang="ts">
	import ChatList from './lib/components/ChatList.svelteold';

	const sampleChats = [
		{
			id: 1,
			name: 'Alice Johnson',
			avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Alice',
			lastMessage: 'Hey, how are you doing?',
			timestamp: '2m',
			unread: 2,
			isGroup: false,
			messages: [
				{ id: 1, message: 'Hey, how are you doing?', timestamp: '2m', isOwn: false, name: 'Alice' }
			]
		},
		{
			id: 2,
			name: 'Team Project',
			avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Team',
			lastMessage: 'Meeting at 3 PM today',
			timestamp: '1h',
			unread: 0,
			isGroup: true,
			members: 5,
			messages: [
				{ id: 2, message: 'Meeting at 3 PM today', timestamp: '1h', isOwn: false, name: 'Bob' }
			]
		},
		{
			id: 3,
			name: 'Bob Smith',
			avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Bob',
			lastMessage: 'Thanks for the help!',
			timestamp: '3h',
			unread: 1,
			isGroup: false,
			messages: [
				{ id: 3, message: 'Thanks for the help!', timestamp: '3h', isOwn: false, name: 'Bob' }
			]
		}
	];

	let activeChat = 1;

	function handleSelectChat(id: number) {
		activeChat = id;
		console.log('Selected chat:', id);
	}

	function handleOpenSettings() {
		console.log('Opening settings...');
	}

	function handleNewMessage() {
		console.log('Creating new message...');
	}

	function handleNewGroup() {
		console.log('Creating new group...');
	}
</script>

<main class="h-screen w-full">
	<ChatList
		chats={sampleChats}
		{activeChat}
		onSelectChat={handleSelectChat}
		onOpenSettings={handleOpenSettings}
		onNewMessage={handleNewMessage}
		onNewGroup={handleNewGroup}
	/>
</main>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
		font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
	}
</style>
