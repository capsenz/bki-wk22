<script lang="ts">
  import { Textarea } from "$lib/components/ui/textarea/index.js";
  import { Chat } from "@ai-sdk/svelte";

  const chat = new Chat();
</script>


<div class="flex flex-col items-center">
    <h1 class="text-4xl text-slate-800 font-bold mt-8 flex items-center gap-4">
        <span class="flex items-center justify-center w-8 h-8 rounded-full bg-slate-800 text-white text-xl font-bold">1</span>
        Describe the job
    </h1>
    <div class="grid grid-cols-2 gap-4 relative mt-6 w-full px-32">
        <div class="bg-white p-6 rounded-lg shadow-lg w-full">
            <h2 class="text-2xl text-slate-800 font-bold">🗒️ Describe the job</h2>
            <div class="p-4 rounded-lg mt-4 shadow-sm">
                <Textarea />
            </div>
        </div>
        <div class="bg-white p-6 rounded-lg shadow-lg">
            <h2 class="text-2xl text-slate-800 font-bold">💬 Job Summary</h2>
        </div>
    </div>  
</div>