<script lang="ts">
  import { Textarea } from "$lib/components/ui/textarea/index.js";
  import { Chat } from "@ai-sdk/svelte";

  const chat = new Chat();
</script>


<div class="flex flex-col items-center">
    <h1 class="text-4xl text-slate-800 font-bold mt-8 flex items-center gap-4">
        <span class="flex items-center justify-center w-8 h-8 rounded-full bg-slate-800 text-white text-xl font-bold">1</span>
        Describe the quote
    </h1>
    <div class="grid grid-cols-2 gap-4 relative mt-6 w-full px-32">
        <div class="bg-white p-6 rounded-lg shadow-lg w-full">
            <h2 class="text-2xl text-slate-800 font-bold">🗒️ Use the chat to interactively describe the quote</h2>
            <div class="h-96 border border-gray-200 rounded-lg mt-4 p-4 flex flex-col space-y-4 overflow-y-auto bg-gray-50">
                {#each chat.messages as message}
                    <div class="flex flex-col space-y-2">
                        <div class="text-sm font-semibold text-slate-600">
                            {message.role === 'user' ? 'You' : 'Assistant'}
                        </div>
                        <div class="bg-white p-3 rounded-lg shadow-sm">
                            {#each message.parts as part}
                                {#if part.type === 'text'}
                                    <div>{part.text}</div>
                                {:else if part.type === 'tool-invocation'}
                                    <pre class="text-xs">{JSON.stringify(part.toolInvocation, null, 2)}</pre>
                                {/if}
                            {/each}
                        </div>
                    </div>
                {/each}
            </div>
            <form on:submit|preventDefault={chat.handleSubmit} class="p-4 rounded-lg mt-4 shadow-sm">
                <div class="flex gap-2">
                    <Textarea bind:value={chat.input} placeholder="Type your quote description here..." class="flex-1" />
                    <button type="submit" class="px-4 py-2 bg-slate-800 text-white rounded-lg hover:bg-slate-700 transition-colors self-center">
                        Send
                    </button>
                </div>
            </form>
        </div>
        <div class="bg-white p-6 rounded-lg shadow-lg">
            <h2 class="text-2xl text-slate-800 font-bold">💬 Quote Summary</h2>
        </div>
    </div>  
</div>