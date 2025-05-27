<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	import { Editor } from '@tiptap/core';
	import StarterKit from '@tiptap/starter-kit';

	let element: HTMLElement;
	let editor: Editor;

	export let content: string = '<p>Hello World! 🌍️ </p>';

	onMount(() => {
		editor = new Editor({
			element: element,
			extensions: [StarterKit],
			content: content,
			onUpdate: ({ editor }) => {
				content = editor.getHTML();
			},
			onTransaction: () => {
				// force re-render so `editor.isActive` works as expected
				editor = editor;
			}
		});
	});

	onDestroy(() => {
		if (editor) {
			editor.destroy();
		}
	});
</script>

<div class="mb-4 flex gap-2">
	{#if editor}
		<button
			on:click={() => editor.chain().focus().toggleHeading({ level: 1 }).run()}
			class:active={editor.isActive('heading', { level: 1 })}
			class="rounded px-2 py-1 hover:bg-gray-100"
		>
			H1
		</button>
		<button
			on:click={() => editor.chain().focus().toggleHeading({ level: 2 }).run()}
			class:active={editor.isActive('heading', { level: 2 })}
			class="rounded px-2 py-1 hover:bg-gray-100"
		>
			H2
		</button>
		<button
			on:click={() => editor.chain().focus().setParagraph().run()}
			class:active={editor.isActive('paragraph')}
			class="rounded px-2 py-1 hover:bg-gray-100"
		>
			P
		</button>
	{/if}
</div>

<div bind:this={element} class="prose max-w-none"></div>

<style>
	button.active {
		background: black;
		color: white;
	}
</style>
