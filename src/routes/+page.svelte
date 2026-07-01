<script lang="ts">


import ChatBubble from "$lib/components/ChatBubble.svelte";
import { onMount, tick } from "svelte";

async function regenerateMessage(index: number) {
    if (messages[index].role !== "assistant")
        return;

    let userMessage = null;

    for (let i = index - 1; i >= 0; i--) {
        if (messages[i].role === "user") {
            userMessage = messages[i];
            break;
        }
    }

    if (!userMessage) return;

messages = [
    ...messages,
    {
        role: "assistant",
        text: "Menghasilkan ulang..."
    }
];

const loadingIndex =
    messages.length - 1;

messages = [...messages];

try {
    const resp = await fetch(
        `${import.meta.env.VITE_API_URL}/chat`,
        {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({
                messages: [userMessage]
            })
        }
    );

    if (!resp.ok) {
        throw new Error(
            `Server error ${resp.status}`
        );
    }

    const data = await resp.json();

    messages[loadingIndex] = {
        role: "assistant",
        text: data.reply
    };
}
catch (err) {
    messages[loadingIndex] = {
        role: "assistant",
        text: "Gagal menghasilkan ulang."
    };
}
finally {
    messages = [...messages];
}

await tick();

const container =
    document.querySelector(
        ".chat-output"
    ) as HTMLElement;

if (container) {
    container.scrollTop =
        container.scrollHeight;
}
}

let textarea: HTMLTextAreaElement;
let messages = [
  { role: "assistant", text: "Halo! Selamat Datang.\n\n Silakan Ketik Kata Kunci untuk Membuat Cerita Anda 😄" },
  { role: "user", text: "penjaga, sihir, keberanian" },
  { role: "assistant", text: `Dion Prakoso bekerja sebagai penjaga sihir muda yang selama ini menjalankan peran penting di balik keputusan dewan. Ia berada dalam posisi yang sangat terbuka, meskipun kebebasan tidak pernah boleh diabaikan. Ia juga menjaga hubungan dengan penjaga Dewan Murid dan kerajaan sihir tempat Dion tinggal. 



Aturan yang terlalu kaku membuat Dewan Murid kehilangan kebebasan. Para murid terbiasa bekerja dengan orang yang tidak memiliki otoritas dalam menjaga keseimbangan dan mempertahankan kehormatan. Dion menyimpan pandangan ini, di mana mereka menolak perubahan secara kolektif. Banyak penganut keberanian mendekati Dion melalui proses interaksi bersama dan memastikan hal-hal benar-benar terjadi. Sejak saat itu, semua keputusan dihargai oleh dewan sihir sebagai sesuatu yang adil. Ia juga memahami bahwa kebebasan adalah bentuk dari kebebasan dan kebebasan tidak boleh dihindari.



Aturan itu membuat Dion memutuskan untuk memimpin sebuah kejadian kecil yang disebut Perubahan Besar yang selama ini dijalankan oleh Dewan Murid. Ia mencoba menjaga keseimbangan agar tidak terjadi sesuatu yang bisa menjadi konflik besar dalam dunia murim. Kenyataan tersebut mencerminkan bahwa kebebasan dapat bertahan kapan saja. Tetapi, Dion menemukan bukti dari tindakan Dion dan membawanya ke lapangan baru yang tidak mengikuti keputusan. Ia mulai memahami bahwa kebebasan yang ditentukan oleh masa lalu benar-benar dibangun dalam kesadaran kolektif yang terus berkembang.



Dion menemukan cara untuk mempertahankan kebebasan yang selama ini dijaga dengan penuh ketekunan. Ia mencoba menyampaikan kebenaran dalam konteks masa lalu dan membangun kesadaran baru dalam dunia murim yang bisa mengubah hidupnya secara tidak langsung. Di sisi lain, keputusan tersebut telah memastikan bahwa semua orang benar-benar memiliki kebebasan. Para penyihir memahami bahwa kebebasan tidak bisa digantikan oleh kekuatan atau kenyataan, melainkan hanya dari tindakan mereka sendiri. Perubahan Besar terjadi ketika Dewan Murid menentukan bahwa kebebasan akan perlahan kembali kepada masyarakat.



Perlahan, Dewan Murid mengambil keputusan untuk membangun kembali kesadaran baru yang lebih baik. Ia juga berhasil membuktikan bahwa kebebasan tidak diberikan kepada semua orang. Dion memilih mengatakan bahwa kebijakan lama tidak bisa dihindari dengan menunjukkan kebenaran atau menyangkalnya. Ketika ia mengucapkannya, seluruh anggota dewan menerima penjelasan itu dan mempertahankan kebenaran selama ini. Ia menyadari bahwa perubahan besar adalah kemajuan dalam bertindak. Kebebasan tidak lagi merugikan masyarakat, melainkan akan mendorong keseimbangan baru bagi dunia murim.



Catatan tersebut membawa contoh bahwa kebebasan diberikan untuk menjaga keadilan, bukan sekadar menghentikan kebebasan. Dion menyadari bahwa kebenaran tidak akan meninggalkan hidupnya. Ia memahami bahwa kebebasan adalah sesuatu yang harus diam-diam ditampilkan, dan ia tahu bagaimana penggunaan sihir dapat memperkuat kendali atas masyarakat. Ia juga membuktikan bahwa kebebasan tidak selamanya hanya diperlakukan dengan aturan lama. Ketika kebenaran datang, suasana dunia murim berubah. Beberapa anggota dewan menemukan tanda bahwa kebebasan ada, tidak ada lagi kemungkinan untuk mengalami kerusakan atau kehancuran.



Beberapa tahun kemudian, kebijakan baru diumumkan sebagai keputusan yang memberikan keseimbangan dalam dunia murim. Dalam proses tersebut, dewan memutuskan untuk menghentikan semua pembatasan sihir dan menghilangkan penggunaan sihir terlarang. Salah satu penggunaan sihir itu adalah teknik penyeimbang yang terkuat. Ketika berbicara dengan Dion, para anggota Dewan menemukan bahwa kebebasan tidak hanya bergantung pada sistem lama, melainkan pada kebenaran yang terjadi secara diam-diam. Dion mengajak masyarakat untuk bertindak agar kebenaran bisa tercapai dengan benar.



ketika kerajaan baru dibangun, kebenaran bertahan. Dion menyadari bahwa kebebasan adalah sesuatu yang tidak boleh dihapus dari sistem lama. Ia juga melihat betapa pentingnya memperjuangkan kebebasan sebagai hal kecil. Ia akhirnya membentuk tim resmi penyihir untuk menjaga keseimbangan bagi dunia murim. Sejak saat itu, Dewan Komunitas mengumpulkan kelompok tertentu untuk mengadakan pertemuan nyata dan mendukung keputusan besar. Tekanan tersebut benar-benar menciptakan suasana kekuatan lebih baik, sehingga masyarakat dapat membantu menjaga citra yang sejati.



Setelah keputusan besar diperjuangkan, Dewan Komunitas memutuskan untuk memperbarui aturan dan mengubah sistem yang lama. Ketika kerajaan baru dibangun, banyak orang menyadari bahwa kebebasan tidak bisa dihapus dari kenyataan. Dion dipanggil oleh anggota dewan untuk berbicara secara rinci. Ia menjelaskan bagaimana ia terlibat dalam perubahan tersebut. Keputusan itu membuat Dion merasakan ketenangan dan kelegaan. Di balik kekuatan yang telah mereka cari, Dion memilih untuk membantu masyarakat untuk menemukan jalan menuju keseimbangan.



Keesokan harinya, sebuah rapat dilakukan untuk mengadakan diskusi internal di Dewan Komunitas. Ia mengumumkan bahwa kebebasan dapat dicapai melalui proses pembangunan yang lebih kompleks. Dari kesimpulan itu, masyarakat mempertanyakan apakah kekuatan yang selama ini mereka bayangkan benar-benar benar. Dion mencoba menjelaskan alasan yang tidak pernah diketahui sebelumnya tentang kenyataan. Percakapan dengan anggota dewan tidak lagi terhubung dengan pemahaman yang telah lama dipahami. Diskusi tersebut menyampaikan bahwa keputusan Dion adalah sesuatu yang memengaruhi dunia yang dulu ia cintai.` },
];
let input = "";
let loading = false;

const adjustHeight = () => {
  if (textarea) {
    textarea.style.height = "auto";
    textarea.style.height = `${textarea.scrollHeight}px`;
  }
};

onMount(() => {
  adjustHeight();
});

const handleInput = () => {
  adjustHeight();
  // optional: limit input length
  if (input.length > 4000) input = input.slice(0, 4000);
};

async function sendMessage() {
  const trimmed = input.trim();
  if (!trimmed || loading) return;

  // tambah pesan user dulu ke UI
  messages = [...messages, { role: "user", text: trimmed }];
  input = "";
  adjustHeight();
  loading = true;

  const context = messages.slice(-8); 

  try {
    const resp = await fetch(`${import.meta.env.VITE_API_URL}/chat`, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ messages: context })
    });

    if (!resp.ok) {
      const txt = await resp.text();
      throw new Error(`Server error: ${resp.status} ${txt}`);
    }

    const data = await resp.json();
    // backend mengembalikan { reply: "..." }
    if (data && data.reply) {
      messages = [...messages, { role: "assistant", text: data.reply }];
   
      await tick();
      const container = document.querySelector('.chat-output') as HTMLElement;
      if (container) container.scrollTop = container.scrollHeight;
    } else {
      throw new Error("Invalid response from server");
    }
  } catch (err) {
    console.error(err);
    messages = [...messages, { role: "assistant", text: "Terjadi kesalahan saat menghubungi server." }];
  } finally {
    loading = false;
  }
}

</script>
<style>
    .chat-container{
        background: var(--background);
        display: flex;
        flex-direction: column;
        align-items: center;
        height: 95vh;
    }
    .chat-output{ 
        display: flex;
        flex-direction: column;
        align-items: center;
        height: 90%;
        width: 80%;
        overflow-y:auto;
    }

    .chat-input{
        bottom: 5vh;
        display: flex;
        flex-direction: row;
        width: 80%;
        height: 5vh;
        align-items: flex-end;
    }

    .chat-output::-webkit-scrollbar {
        display: none; /* Chrome, Safari */
    }

    .chat-output {
        -ms-overflow-style: none;  /* Internet Explorer dan Edge lama */
        scrollbar-width: none;     /* Firefox */
    }

    .text-input{
        flex: 1;
        width: 90%;
        height: 100%;
        display: flex;
        align-items: flex-end;
    }

    textarea{
        min-height: 5vh;
        margin: 0;
        padding-top: 1vh;
        padding-bottom: 1vh;
        padding-left: 10px;
        width: 100%;
        border-bottom-left-radius: 10px;
        border-top-left-radius: 10px;
        font-size: 16px;
        resize: none;
        overflow: hidden;
        transform-origin: bottom;
        transition: height 0.1s;
        box-sizing: border-box;
    }

    .chat-input button{
        width: 10%;
        height: 100%;
        align-self: flex-end;
    }

    button{
        margin: 0;
        border-radius: 0px 10px 10px 0px;
        border: none;
        font-size: 16px;
        cursor: pointer;
        background-color: rgb(25, 25, 25);
        color: white;
        border: white 3px solid;
        font-family:var(--app-font), sans-serif;
         transition: background-color 0.2s, color 0.2s;
    }

    button[disabled] { opacity: 0.6; cursor: not-allowed; }
</style>

<div class="chat-container">
    <div class="chat-output">
        {#each messages as m, index}
             <ChatBubble role={m.role} text={m.text} on:regenerate={() => regenerateMessage(index)}
 />
        {/each}
    </div>

    <div class="chat-input">
        <div class="text-input">
            <textarea
            id="chatInput"
            bind:this={textarea}
            bind:value={input}
            on:input={handleInput}
            rows="1"
            placeholder="Tulis Pesan Anda Disini.."></textarea>
        </div>
        <button on:click={sendMessage} disabled={loading}>
                {#if loading}
                    Sending...
                {:else}
                    Send
                {/if}
        </button>
    </div>
</div>