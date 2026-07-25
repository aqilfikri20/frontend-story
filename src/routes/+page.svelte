<script lang="ts">

import StoryGenerator from "$lib/components/StoryGenerator.svelte";
import ChatBubble from "$lib/components/ChatBubble.svelte";
import { onMount, tick } from "svelte";


let textarea: HTMLTextAreaElement;
let messages = [
  { role: "assistant", text: "Halo! Selamat Datang.\n\n Silakan Ketik Kata Kunci untuk Membuat Cerita Anda 😄" },
];
let input = "";
let loading = false;



</script>
<style>
.page{
    width:100%;
    min-height:100vh;

    background:var(--background);

    display:flex;
    justify-content:center;
    align-items:flex-start;

    padding:40px 20px;

    box-sizing:border-box;
}

</style>

<div class="page">
<StoryGenerator />
</div>