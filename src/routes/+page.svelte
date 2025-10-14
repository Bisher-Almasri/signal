<script lang="ts">
	import ChatList from '$lib/components/ChatList.svelte';

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

	let chats: Chat[] = [
		{
			id: 1,
			name: 'Alice Johnson',
			avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Alice',
			lastMessage: 'See you tomorrow!',
			timestamp: '10:32 AM',
			unread: 2,
			messages: [
				{
					id: 1,
					message: 'Hey! How are you doing?',
					timestamp: '10:15 AM',
					isOwn: false,
					avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Alice',
					name: 'Alice Johnson'
				},
				{
					id: 2,
					message: "I'm doing great! Just working on a new project.",
					timestamp: '10:18 AM',
					isOwn: true
				},
				{
					id: 3,
					message: 'That sounds exciting! What kind of project?',
					timestamp: '10:20 AM',
					isOwn: false,
					avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Alice',
					name: 'Alice Johnson'
				},
				{
					id: 4,
					message: "It's a chat app with a Signal-inspired design. Really enjoying the process!",
					timestamp: '10:25 AM',
					isOwn: true
				},
				{
					id: 5,
					message: 'See you tomorrow!',
					timestamp: '10:32 AM',
					isOwn: false,
					avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Alice',
					name: 'Alice Johnson'
				}
			]
		},
		{
			id: 2,
			name: 'Bob Smith',
			avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Bob',
			lastMessage: 'Thanks for the help!',
			timestamp: 'Yesterday',
			unread: 0,
			messages: [
				{
					id: 1,
					message: 'Can you help me with something?',
					timestamp: 'Yesterday 3:45 PM',
					isOwn: false,
					avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Bob',
					name: 'Bob Smith'
				},
				{
					id: 2,
					message: 'Of course! What do you need?',
					timestamp: 'Yesterday 3:48 PM',
					isOwn: true
				},
				{
					id: 3,
					message: 'Thanks for the help!',
					timestamp: 'Yesterday 4:12 PM',
					isOwn: false,
					avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Bob',
					name: 'Bob Smith'
				}
			]
		},
		{
			id: 3,
			name: 'Design Team',
			avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=DesignTeam',
			lastMessage: 'Alice: The mockups look great!',
			timestamp: 'Monday',
			unread: 3,
			isGroup: true,
			members: 5,
			messages: [
				{
					id: 1,
					message: 'Hey everyone! Check out the new designs',
					timestamp: 'Monday 10:15 AM',
					isOwn: false,
					avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Bob',
					name: 'Bob'
				},
				{
					id: 2,
					message: 'Looking good! I have some feedback on the color scheme.',
					timestamp: 'Monday 10:20 AM',
					isOwn: true
				},
				{
					id: 3,
					message: 'The mockups look great!',
					timestamp: 'Monday 10:25 AM',
					isOwn: false,
					avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Alice',
					name: 'Alice'
				},
				{
					id: 4,
					message: 'Can we discuss the layout changes in the next meeting?',
					timestamp: 'Monday 10:30 AM',
					isOwn: false,
					avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Carol',
					name: 'Carol'
				}
			]
		},
		{
			id: 4,
			name: 'David Brown',
			avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=David',
			lastMessage: 'Got it, thanks!',
			timestamp: 'Sunday',
			unread: 0,
			messages: [
				{
					id: 1,
					message: 'Did you see the game last night?',
					timestamp: 'Sunday 8:15 PM',
					isOwn: false,
					avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=David',
					name: 'David Brown'
				},
				{
					id: 2,
					message: 'No, I missed it! Who won?',
					timestamp: 'Sunday 8:20 PM',
					isOwn: true
				},
				{
					id: 3,
					message: 'Got it, thanks!',
					timestamp: 'Sunday 8:25 PM',
					isOwn: false,
					avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=David',
					name: 'David Brown'
				}
			]
		},
		{
			id: 5,
			name: 'Family',
			avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Family',
			lastMessage: 'Mom: See you all on Sunday!',
			timestamp: 'Saturday',
			unread: 2,
			isGroup: true,
			members: 4,
			messages: [
				{
					id: 1,
					message: 'Who wants to come over for dinner on Sunday?',
					timestamp: 'Saturday 2:10 PM',
					isOwn: false,
					avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Mom',
					name: 'Mom'
				},
				{
					id: 2,
					message: "I'll be there!",
					timestamp: 'Saturday 2:15 PM',
					isOwn: true
				},
				{
					id: 3,
					message: 'Count me in too!',
					timestamp: 'Saturday 2:18 PM',
					isOwn: false,
					avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Sister',
					name: 'Sarah'
				},
				{
					id: 4,
					message: 'See you all on Sunday!',
					timestamp: 'Saturday 2:20 PM',
					isOwn: false,
					avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Mom',
					name: 'Mom'
				}
			]
		},
		{
			id: 6,
			name: 'Emma Davis',
			avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Emma',
			lastMessage: 'Perfect! Talk soon.',
			timestamp: 'Friday',
			unread: 0,
			messages: [
				{
					id: 1,
					message: 'Are you free this weekend?',
					timestamp: 'Friday 2:10 PM',
					isOwn: false,
					avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Emma',
					name: 'Emma Davis'
				},
				{
					id: 2,
					message: 'Yes! What did you have in mind?',
					timestamp: 'Friday 2:15 PM',
					isOwn: true
				},
				{
					id: 3,
					message: 'Perfect! Talk soon.',
					timestamp: 'Friday 2:20 PM',
					isOwn: false,
					avatar: 'https://api.dicebear.com/7.x/avataaars/svg?seed=Emma',
					name: 'Emma Davis'
				}
			]
		}
	];

	let activeChat = 1;
	$: activeChatData = chats.find((chat) => chat.id === activeChat);
</script>

<h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>

<ChatList
	{activeChat}
	{chats}
	onNewGroup={() => {}}
	onNewMessage={() => {}}
	onOpenSettings={() => {}}
	onSelectChat={() => {}}
/>
