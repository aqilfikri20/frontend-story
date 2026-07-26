<script lang="ts">
    let form = {
    nama: "",
    genre: "",
    peran: "",
    sifat: "",
    elemen: ["", ""]
};

let loading = false;
let story = "";

import { tick } from "svelte";

function validateForm(): boolean {
    if (!form.genre) {
        alert("Silakan pilih genre.");
        return false;
    }

    if (!form.peran) {
        alert("Silakan pilih peran.");
        return false;
    }

    if (!form.sifat) {
        alert("Silakan masukkan sifat tokoh utama.");
        return false;
    }

    // Minimal satu elemen penting harus diisi
    const hasElement = form.elemen.some(e => e.trim() !== "");
    if (!hasElement) {
        alert("Masukkan minimal satu elemen penting.");
        return false;
    }

    return true;
}

async function generateStory() {

    if (!validateForm()) return;

    loading = true;
    story = "";

    try {

        const payload = {
            ...form,
            elemen: form.elemen
                .map(e => e.trim())
                .filter(Boolean)
        };

        const response = await fetch("http://localhost:8000/chat", {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(payload)
        });

        if (!response.ok) {
            throw new Error(`Server Error ${response.status}`);
        }

        const data = await response.json();

        story = data.reply ?? "Cerita tidak tersedia.";

        await tick();

        document
            .querySelector(".story-result")
            ?.scrollIntoView({
                behavior: "smooth"
            });

    } catch (err) {

        console.error(err);

        story = "Terjadi kesalahan saat menghasilkan cerita.";

    } finally {

        loading = false;

    }

}

function copyStory() {
    navigator.clipboard.writeText(story)
        .then(() => {
            alert("Cerita berhasil disalin!");
        })
        .catch(() => {
            alert("Gagal menyalin cerita.");
        });
}

function downloadStory() {
    const blob = new Blob([story], {
        type: "text/plain;charset=utf-8"
    });

    const url = URL.createObjectURL(blob);

    const a = document.createElement("a");
    a.href = url;
    a.download = "cerita.txt";
    a.click();

    URL.revokeObjectURL(url);
}

function regenerateStory() {
    generateStory();
}
</script>

<style>
    .generator{
            font-family: var(--app-font), sans-serif;
    font-size: var(--app-font-size);
    max-width:900px;
    margin:40px auto;
    padding:20px;
    box-sizing:border-box;
}

.header{
    text-align:center;
    margin-bottom:30px;
}

.header h1{
    margin:0;
    font-size:2rem;
    color:var(--font-color);
}

.header p{
    margin-top:10px;
    color:#888;
}

.card{
    background:var(--assistant-bubble);
    border-radius:18px;
    padding:30px;
    box-shadow:0 8px 25px rgba(0,0,0,.08);
}

.grid{
    display:grid;
    grid-template-columns:repeat(2,1fr);
    gap:20px;
}

.field{
    margin-top: 10px;
    display:flex;
    flex-direction:column;
}

.field label,
.elements label{
    font-weight:600;
    margin-bottom:8px;
    color:var(--font-color);
}

input,
select{
    width:100%;
    padding:12px 15px;

    border-radius:10px;
    border:1px solid #d7d7d7;

    background:var(--background);
    color:var(--font-color);

    font-size:15px;

    box-sizing:border-box;
    transition:.2s;
}

input:focus,
select:focus{
    outline:none;
    border-color:#4a8cff;
    box-shadow:0 0 0 3px rgba(74,140,255,.15);
}

.elements{
    margin-top:25px;

    display:flex;
    flex-direction:column;

    gap:8px;
}

.generate{
    margin-top:30px;

    width:100%;
    height:50px;

    border:none;
    border-radius:12px;

    background:#3b82f6;
    color:white;

    font-size:16px;
    font-weight:600;

    cursor:pointer;

    transition:.25s;
}

.generate:hover{
    background:#2563eb;
}

.generate:disabled{
    opacity:.6;
    cursor:not-allowed;
}

@media(max-width:768px){

    .grid{
        grid-template-columns:1fr;
    }

    .card{
        padding:20px;
    }

    .header h1{
        font-size:1.7rem;
    }

}

.story-result{
    margin-top:30px;
    padding:25px;

    border-radius:15px;

    background:var(--background);
    color:var(--font-color);

    line-height:1.8;
    text-align:justify;
}

.story-result h2{
    margin-top:0;
}

.story-result p{
    margin-bottom:16px;
}

.story-actions{
    margin-top:25px;

    display:flex;
    gap:12px;
    flex-wrap:wrap;

    justify-content:flex-end;
}

.story-actions button{

    padding:10px 18px;

    border:none;
    border-radius:10px;

    cursor:pointer;

    background:#3b82f6;
    color:white;

    font-weight:600;

    transition:.2s;
}

.story-actions button:hover{
    background:#2563eb;
}
</style>

<div class="generator">

    <div class="header">
        <h1>AI Story Generator</h1>
        <p>Buat cerita berdasarkan karakter dan kata kunci yang Anda tentukan.</p>
    </div>

    <div class="card">

        <div class="grid">

            <div class="field">
                <label>Nama Tokoh</label>
                <input bind:value={form.nama} placeholder="Contoh: Dion Prakoso">
            </div>

            <div class="field">
                <label>Genre Dunia</label>
                <select bind:value={form.genre}>
                    <option value="">Pilih Genre</option>
                    <option>Fantasi</option>
                    <option>Sci-Fi</option>
                    <option>Non-Fiksi</option>
                    <option>Kerajaan</option>
                    <option>Bela Diri</option>
                </select>
            </div>

        </div>
        <div class="field">
            <label>Peran/Pekerjaan Tokoh</label>
            <input bind:value={form.peran} placeholder="Peran/Pekerjaan Tokoh Utama">
        </div>

        <div class="field">
            <label>Sifat Tokoh Utama</label>
            <input bind:value={form.sifat} placeholder="Sifat Tokoh Utama">
        </div>

        <div class="elements">
        <label>Elemen Penting</label>

<input
    bind:value={form.elemen[0]}
    placeholder="Elemen penting pertama"
/>

<input
    bind:value={form.elemen[1]}
    placeholder="Elemen penting kedua"
/>
        </div>

<button
    class="generate"
    on:click={generateStory}
    disabled={loading}
>
    {#if loading}
        Generating...
    {:else}
        Generate Cerita
    {/if}
</button>

{#if story}
<div class="story-result">

    <h2>Hasil Cerita</h2>

    {#each story.split("\n\n") as paragraph}
        <p>{paragraph}</p>
    {/each}

    <div class="story-actions">

        <button on:click={downloadStory}>
            📥 Download
        </button>

        <button on:click={copyStory}>
            📋 Copy
        </button>

        <button on:click={regenerateStory}>
            🔄 Generate Lagi
        </button>

    </div>

</div>
{/if}

</div>

</div>