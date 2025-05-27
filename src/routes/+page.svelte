<script lang="ts">
    import { Textarea } from "$lib/components/ui/textarea/index.js";
    import { Chat } from "@ai-sdk/svelte";
    import {Tipex} from '@friendofsvelte/tipex';
    import "@friendofsvelte/tipex/styles/Tipex.css";
    import "@friendofsvelte/tipex/styles/ProseMirror.css";
    import "@friendofsvelte/tipex/styles/Controls.css";
    import "@friendofsvelte/tipex/styles/EditLink.css";
    import "@friendofsvelte/tipex/styles/CodeBlock.css";
    import { Button } from "$lib/components/ui/button/index.js";

    const chat = new Chat();

    let body = `<p>The initial html content.</p>`;
    let isGenerating = false;
    let isGenerationComplete = false;

    function handleGenerate() {
        isGenerating = true;
        // Simulate API call for now
        setTimeout(() => {
            isGenerating = false;
            isGenerationComplete = true;
        }, 2000);
    }
</script>


<div class="flex flex-col items-center">
    <h1 class="text-2xl text-slate-800 font-bold mt-8 flex items-center gap-4">
        <span class="flex items-center justify-center w-8 h-8 rounded-full bg-slate-800 text-white text-l font-bold">1</span>
        Describe the quote
    </h1>
    <div class="grid grid-cols-2 gap-4 relative mt-6 w-full px-32">
        <div class="bg-white p-6 rounded-lg shadow-lg w-full">
            <h2 class="text-2xl text-slate-800 font-bold">🗒️ Chat</h2>
            <p class="text-sm text-slate-600">Use the chat to interactively describe the quote</p>
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
            <form onsubmit={chat.handleSubmit} class="p-4 rounded-lg mt-4 shadow-sm">
                <div class="flex gap-2">
                    <Textarea bind:value={chat.input} placeholder="Type your quote description here..." class="flex-1" />
                    <button type="submit" class="px-4 py-2 bg-slate-800 text-white rounded-lg hover:bg-slate-700 transition-colors self-center">
                        Send
                    </button>
                </div>
            </form>
        </div>
        <div class="bg-white p-6 rounded-lg shadow-lg flex flex-col">
            <h2 class="text-2xl text-slate-800 font-bold">💬 Summary</h2>
            <p class="text-sm text-slate-600">A summary of how the system understand the job</p>
            <div class="border border-gray-200 rounded-lg mt-4 p-4 flex flex-col space-y-4 overflow-y-auto bg-gray-50 flex-1">
                <Tipex {body} controls floating focal
                    style="margin-top: 1rem; margin-bottom: 0;" 
                    class="h-full border border-neutral-200"/>
            </div>
        </div>
    </div>
    <div class="mt-8 mb-8 flex flex-col items-center gap-4">
        {#if isGenerating}
            <h1 class="text-2xl text-slate-800 font-bold flex items-center gap-4">
                <span class="flex items-center justify-center w-8 h-8 rounded-full bg-slate-800 text-white text-l font-bold">2</span>
                Generating
                <span class="animate-spin">⚙️</span>
            </h1>
        <!-- && chat.messages.length > 0 -->
        {:else if !isGenerating} 
            <h1 class="text-2xl text-slate-800 font-bold flex items-center gap-4">
                <span class="flex items-center justify-center w-8 h-8 rounded-full bg-slate-800 text-white text-l font-bold">2</span>
                Review your quote
            </h1>
        {/if}
        {#if !isGenerationComplete}
            <Button type="button" onclick={handleGenerate} disabled={isGenerating}>
                {isGenerating ? 'Generating...' : 'Generate quote'}
            </Button>
        {/if}
    </div>

</div>