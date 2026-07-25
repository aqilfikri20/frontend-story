<script lang="ts">
let pdfElement: HTMLDivElement | null = null;

  export let role; // "user" atau "assistant"
  export let text:any;
import { createEventDispatcher } from "svelte";

const dispatch = createEventDispatcher();

function regenerate() {
    dispatch("regenerate");
}

async function copyText() {
    await navigator.clipboard.writeText(
    capitalizeSentences(text)
);
}

function capitalizeSentences(text: string): string {
    return text.replace(
        /(^\s*[a-zà-ÿ]|[.!?]\s+[a-zà-ÿ])/g,
        (match: string) => match.toUpperCase()
    );
}
async function downloadPdf() {
    if (!pdfElement) return;
    const html2pdf =
        (await import("html2pdf.js")).default;


    await html2pdf()
        .set({
            margin: 0,
            filename: "story.pdf",

            image: {
                type: "jpeg",
                quality: 1
            },

            html2canvas: {
                scale: 2,
                useCORS: true
            },

            jsPDF: {
                unit: "mm",
                format: "a4",
                orientation: "portrait"
            }
        })
        .from(pdfElement)
        .save();
}

const fontMap: Record<string, string> = {
    "Arial": "helvetica",
    "Times New Roman": "times",
    "Courier New": "courier"
};


</script>

<style>
.bubble {
    position: relative;
    width: fit-content;
    max-width: min(85%, 700px);

    padding: 1rem 1rem;
    border-radius: 12px;

    word-wrap: break-word;
    overflow-wrap: anywhere;

    line-height: 1.6;
    font-size: clamp(0.9rem, 2vw, 1rem);

    box-sizing: border-box;
}

  .user {
    background: var(--user-bubble);
    color: var(--font-color);
    align-self: flex-end;
    border-bottom-right-radius: 0;
    margin-bottom: 10px;
  }

.story {
    width: 100%;
    max-width: 700px;

    font-family: var(--app-font), sans-serif;
    font-size: var(--app-font-size);
    line-height: 1.7;

    text-align: justify;
    word-break: break-word;
}

.assistant p {
    margin: 0 0 1em 0;
}
  .assistant {
    background: var(--assistant-bubble);
    color: var(--font-color);
    align-self: flex-start;
    border-bottom-left-radius: 0;
   margin: 0;
  }

.message-actions {
    display: flex;
    flex-wrap: wrap;

    gap: 4px;
    margin-left: 4px;
    margin-top: 6px;

    opacity: .9;
}
.action-btn {
    width: 28px;
    height: 28px;

    display: flex;
    align-items: center;
    justify-content: center;

    border: none;
    border-radius: 10px;

    background: transparent;
    color: var(--font-color);

    cursor: pointer;

    transition: all 0.15s ease;
}
.action-btn:hover {
    background: rgba(255,255,255,0.1);
}

.assistant-wrapper {
    margin-top: 20px;

    display: flex;
    flex-direction: column;

    width: 100%;
    max-width: 100%;
}

.message-actions {
  display: flex;
  gap: 2px;
  margin-left: 4px;   /* sedikit indent dari tepi bubble */
  margin-top: 2px;    /* jarak dari bubble ke tombol */
  opacity: 0.8;
}

.pdf-hidden {
    position: fixed;
    left: -99999px;
    top: 0;
}

.pdf-container {
    width: 210mm;
    min-height: 296.8mm;

    box-sizing: border-box;

    background: var(--assistant-bubble);
    color: var(--font-color);

    padding: 20mm;

    border-radius: 0;
}

.pdf-story {
    font-family: var(--app-font), sans-serif;

    font-size: var(--app-font-size);

    line-height: 1.8;

    text-align: justify;

    word-break: break-word;
}

.pdf-story p {
    margin-bottom: 1em;

    @media (max-width: 768px) {

    .bubble {
        max-width: 95%;
        padding: 0.9rem;
        font-size: 0.92rem;
    }

    .story {
        font-size: 15px;
        line-height: 1.6;
    }

    .assistant-wrapper {
        margin-top: 16px;
    }

    .action-btn {
        width: 34px;
        height: 34px;
        font-size: 18px;
    }
}

@media (max-width: 480px) {

    .bubble {
        max-width: 100%;
        border-radius: 10px;
        padding: 0.8rem;
    }

    .story {
        font-size: 14px;
        line-height: 1.6;
    }

    .message-actions {
        gap: 2px;
    }

    .action-btn {
        width: 36px;
        height: 36px;
    }
}
}
</style>

{#if role === "assistant"}
<div class="assistant-wrapper">

    <!-- Bubble chat normal -->
    <div class="bubble assistant">
        <div class="story">
            {#each capitalizeSentences(text).split('\n\n') as paragraph}
                <p>{paragraph}</p>
            {/each}
        </div>
    </div>

    <div class="message-actions">
        <button
            class="action-btn"
            on:click={copyText}
            title="Salin"
        >
            📋
        </button>

        <button
            class="action-btn"
            on:click={downloadPdf}
            title="Download PDF"
        >
            📥
        </button>

        <button
            class="action-btn"
            on:click={regenerate}
            title="Ulangi"
        >
            🔄
        </button>
    </div>

</div>

<!-- Template khusus PDF -->
<div class="pdf-hidden">
    <div
        bind:this={pdfElement}
        class="pdf-container"
    >
        <div class="pdf-story">
            {#each capitalizeSentences(text).split('\n\n') as paragraph}
                <p>{paragraph}</p>
            {/each}
        </div>
    </div>
</div>
{:else}
  <div class="bubble {role}">
    <div class="story">
      {#each capitalizeSentences(text).split('\n\n') as paragraph}
        <p>{paragraph}</p>
      {/each}
    </div>
  </div>
{/if}