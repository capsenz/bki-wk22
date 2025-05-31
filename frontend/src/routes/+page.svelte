<script lang="ts">
	import { Textarea } from '$lib/components/ui/textarea/index.js';
	import { Chat } from '@ai-sdk/svelte';
	import { Tipex } from '@friendofsvelte/tipex';
	import '@friendofsvelte/tipex/styles/Tipex.css';
	import '@friendofsvelte/tipex/styles/ProseMirror.css';
	import '@friendofsvelte/tipex/styles/Controls.css';
	import '@friendofsvelte/tipex/styles/EditLink.css';
	import '@friendofsvelte/tipex/styles/CodeBlock.css';
	import { Button } from '$lib/components/ui/button/index.js';
	import * as Table from "$lib/components/ui/table/index.js";

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
	<h1 class="mt-8 flex items-center gap-4 text-2xl font-bold text-slate-800">
		<span
			class="text-l flex h-8 w-8 items-center justify-center rounded-full bg-slate-800 font-bold text-white"
			>1</span
		>
		Describe the quote
	</h1>
	<div class="relative mt-6 grid w-full grid-cols-2 gap-4 px-32">
		<div class="w-full rounded-lg bg-white p-6 shadow-lg">
			<h2 class="text-2xl font-bold text-slate-800">🗒️ Chat</h2>
			<p class="text-sm text-slate-600">Use the chat to interactively describe the quote</p>
			<div
				class="mt-4 flex h-96 flex-col space-y-4 overflow-y-auto rounded-lg border border-gray-200 bg-gray-50 p-4"
			>
				{#each chat.messages as message}
					<div class="flex flex-col space-y-2">
						<div class="text-sm font-semibold text-slate-600">
							{message.role === 'user' ? 'You' : 'Assistant'}
						</div>
						<div class="rounded-lg bg-white p-3 shadow-sm">
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
			<form onsubmit={chat.handleSubmit} class="mt-4 rounded-lg p-4 shadow-sm">
				<div class="flex gap-2">
					<Textarea
						bind:value={chat.input}
						placeholder="Type your quote description here..."
						class="flex-1"
					/>
					<button
						type="submit"
						class="self-center rounded-lg bg-slate-800 px-4 py-2 text-white transition-colors hover:bg-slate-700"
					>
						Send
					</button>
				</div>
			</form>
		</div>
		<div class="flex flex-col rounded-lg bg-white p-6 shadow-lg">
			<h2 class="text-2xl font-bold text-slate-800">💬 Summary</h2>
			<p class="text-sm text-slate-600">A summary of how the system understand the job</p>
			<div
				class="mt-4 flex flex-1 flex-col space-y-4 overflow-y-auto rounded-lg border border-gray-200 bg-gray-50 p-4"
			>
				<Tipex
					{body}
					controls
					floating
					focal
					style="margin-top: 1rem; margin-bottom: 0;"
					class="h-full border border-neutral-200"
				/>
			</div>
		</div>
	</div>
	<div class="mt-8 mb-8 flex flex-col items-center gap-4">
		{#if isGenerating}
			<h1 class="flex items-center gap-4 text-2xl font-bold text-slate-800">
				<span
					class="text-l flex h-8 w-8 items-center justify-center rounded-full bg-slate-800 font-bold text-white"
					>2</span
				>
				Generating
				<span class="animate-spin">⚙️</span>
			</h1>
			<!-- && chat.messages.length > 0 -->
		{:else if !isGenerating}
			<h1 class="flex items-center gap-4 text-2xl font-bold text-slate-800">
				<span
					class="text-l flex h-8 w-8 items-center justify-center rounded-full bg-slate-800 font-bold text-white"
					>2</span
				>
				Review your quote
			</h1>
		{/if}
		{#if !isGenerationComplete}
			<Button type="button" class="self-center rounded-lg bg-slate-800 px-4 py-2 text-white transition-colors hover:bg-slate-700" onclick={handleGenerate} disabled={isGenerating}>
				{isGenerating ? 'Generating...' : 'Generate quote'}
				<!-- Here we then run the api call -->
			</Button>	
		{/if}
	</div>
	{#if isGenerationComplete}
	<div class="relative mt-6 grid w-full grid-cols-3 gap-4 px-32">
		<div class="col-span-1 w-full rounded-lg bg-white p-6 shadow-lg">
			<h2 class="text-2xl font-bold text-slate-800">🗒️ Chat</h2>
			<p class="text-sm text-slate-600">Use the chat to interactively edit the quote</p>
			<div
				class="mt-4 flex h-96 flex-col space-y-4 overflow-y-auto rounded-lg border border-gray-200 bg-gray-50 p-4"
			>
				{#each chat.messages as message}
					<div class="flex flex-col space-y-2">
						<div class="text-sm font-semibold text-slate-600">
							{message.role === 'user' ? 'You' : 'Assistant'}
						</div>
						<div class="rounded-lg bg-white p-3 shadow-sm">
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
			<form onsubmit={chat.handleSubmit} class="mt-4 rounded-lg p-4 shadow-sm">
				<div class="flex gap-2">
					<Textarea
						bind:value={chat.input}
						placeholder="Type your quote description here..."
						class="flex-1"
					/>
					<button
						type="submit"
						class="self-center rounded-lg bg-slate-800 px-4 py-2 text-white transition-colors hover:bg-slate-700"
					>
						Send
					</button>
				</div>
			</form>
		</div>
		<div class="col-span-2 w-full rounded-lg bg-white p-6 shadow-lg">
			<!-- TODO: In the future the table needs to be editable -->
			<h2 class="text-2xl font-bold text-slate-800">💬 Generated quote</h2>
			<p class="text-sm text-slate-600">Review and edit the generated quote</p>
			<Table.Root>
				<Table.Header>
				 <Table.Row>
				  <Table.Head class="w-[100px]">Invoice</Table.Head>
				  <Table.Head>Status</Table.Head>
				  <Table.Head>Method</Table.Head>
				  <Table.Head class="text-right">Amount</Table.Head>
				 </Table.Row>
				</Table.Header>
				<Table.Body>
				 <Table.Row>
				  <Table.Cell class="font-medium text-center">INV001</Table.Cell>
				  <Table.Cell class="text-center">Paid</Table.Cell>
				  <Table.Cell class="text-center">Credit Card</Table.Cell>
				  <Table.Cell class="text-right">$250.00</Table.Cell>
				 </Table.Row>
				</Table.Body>
			   </Table.Root>

		</div>
	</div>
	{/if}
</div>
