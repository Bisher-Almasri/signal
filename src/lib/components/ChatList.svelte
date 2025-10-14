<script lang="ts">
	import { MessageSquarePlus, Search, UsersRound } from 'lucide-svelte';
	import Button from './ui/button/button.svelte';
	import { Input } from './ui/input';
	import ScrollArea from './ui/scroll-area/scroll-area.svelte';

	let searchQuery = '';

	export let chats: Chat[];
	export let activeChat: number;
	export let onSelectChat: (id: number) => void;
	export let onOpenSettings: () => void;
	export let onNewMessage: () => void;
	export let onNewGroup: () => void;

</script>

<div class="flex h-full flex-col border-r border-[#2D2D2F] bg-[#1B1B1D]">
	<!-- header htat i spent too long on -->
  <div class="p-4 border-b border-[#2D2D2F]">
    <div class="flex items-center justify-between mb-3">
      <h1 class="text-white">Signal</h1>
      <div class="flex gap-2">
        <div class="hover-scale">
          <Button
            onclick={() => {}}
            variant="ghost"
            size="icon"
            class="text-white hover:bg-[#2D2D2F]"
          >
            <MessageSquarePlus class="h-5 w-5" />
          </Button>
        </div>
        <div class="hover-scale">
          <Button
            onclick={() => {}}
            variant="ghost"
            size="icon"
            class="text-white hover:bg-[#2D2D2F]"
          >
            <UsersRound class="h-5 w-5" />
          </Button>
        </div>
      </div>
    </div>
    <div class="relative">
      <Search class="absolute left-3 top-1/2 transform -translate-y-1/2 h-4 w-4 text-gray-500" />
      <Input
        bind:value={searchQuery}
        placeholder="Search conversations & messages..."
        class="pl-10 bg-[#2D2D2F] border-none text-white placeholder:text-gray-500 focus-visible:ring-[#2C6BED]"
      />
    </div>
  </div>

  <!-- user pciker thingy -->
   <ScrollArea class="flex-1">

   </ScrollArea>

  <!-- sttings/accnt (idea stolen from discord and slack 🤣🤣🤣🤣🤣🤣🤣🤣🤣🤣🤣🤣🤣🤣🤣🤣) -->

</div>
