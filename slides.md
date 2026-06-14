---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Dasar Teori RAG
info: |
  ## Konsep Dasar RAG
  Presentasi dasar teori Retrieval-Augmented Generation.

  Oleh Benny L.E.P — 063251008
# apply UnoCSS classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable Comark Syntax: https://comark.dev/syntax/markdown
comark: true
# duration of the presentation
duration: 35min
---

# Konsep Dasar RAG

Benny L.E.P 063251008

<div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  Press Space for next page <carbon:arrow-right />
</div>

<div class="abs-br m-6 text-xl">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="slidev-icon-btn">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

---
transition: fade-out
---

# Pengertian RAG

**Retrieval-Augmented Generation** — teknik untuk memperkuat jawaban LLM dengan data eksternal.

<div class="grid grid-cols-3 gap-6 mt-6">
<div>

### 🔍 Retrieval
*Pengambilan*

Pertanyaan pengguna diubah menjadi *vector embedding*, lalu dicocokkan dengan dokumen di **database vektor** melalui pencarian semantik.

</div>
<div>

### ⚡ Augmentation
*Peningkatan*

Dokumen relevan yang ditemukan **disuntikkan** bersama pertanyaan asli untuk membentuk prompt yang lebih kaya konteks.

</div>
<div>

### 🤖 Generation
*Pembuatan*

Prompt yang diperkaya diberikan kepada **LLM**, sehingga AI menjawab berdasarkan bukti spesifik — bukan sekadar menebak.

</div>
</div>

---
transition: slide-left
---

# Arsitektur & Alur RAG

Alur lengkap proses *Retrieval-Augmented Generation* (RAG):

### 📂 1. Fase Ingesti Data (Offline)
Proses pencacahan dan penyimpanan dokumen secara offline.

```mermaid
graph LR
    A[Dokumen] --> B[Chunking]
    B --> C[Embedding]
    C -->|Vektor| D[(DB Vektor)]
    
    classDef database fill:#f9f,stroke:#333,stroke-width:2px;
    class D database;
```

### 🔄 2. Fase Proses RAG (Real-Time)
Alur tanya-jawab teraugmentasi secara real-time.

```mermaid
graph LR
    U((Pengguna)) -->|1. Kueri| EM[Kueri & Embedding]
    EM -->|2. Pencarian| D[(DB Vektor)]
    D -->|3. Konteks| L[LLM + Prompt]
    EM -->|4. Kueri Asli| L
    L -->|5. Respons| R[Jawaban Akhir] --> U

    classDef database fill:#f9f,stroke:#333,stroke-width:2px;
    class D database;
```

<style>
.mermaid {
  zoom: 0.85;
}
.mermaid:nth-of-type(2) {
  zoom: 0.65;
}
</style>

---
transition: slide-left
---

# Proses Penyimpanan Data: Teks ke Vektor

Mengubah teks menjadi representasi angka (vektor) melalui konsep **"dimensi"** untuk menjembatani teks mentah ke pemahaman semantik.

<div class="grid grid-cols-2 gap-6 mt-6">
<div>

### 📂 1. Persiapan Teks (Chunking)

* **Pemotongan Teks**
  Dokumen dipecah menjadi potongan kecil (*chunks*).
* **Chunk Overlap**
  Potongan dibuat sedikit tumpang tindih agar makna/konteks tidak terputus.

</div>
<div>

### 🔢 2. Tokenisasi

* **Tokenisasi & Token ID**
  Potongan teks dipecah menjadi unit dasar bernama *token* (kata/sub-kata), lalu diberi nomor identitas unik (*token ID*).
* **Belum Ada Makna**
  Pada tahap ini, angka-angka tersebut murni pengenal — belum memiliki makna semantik.

</div>
</div>

---
transition: slide-left
---

# Membangun Vektor & Dimensi Semantik

Di tahap inilah teks benar-benar diubah menjadi representasi makna matematis yang mendalam.

<div class="grid grid-cols-2 gap-6 mt-6">
<div>

### 🧠 Proses Embedding
Transformasi kata ke dalam matematika:

* Token dilewatkan ke **Embedding Model** (model AI khusus).
* Diproses melalui beberapa lapisan (*layers*) untuk memahami konteks dan makna keseluruhan.
* Menghasilkan **vector embedding** — deretan angka padat (*dense vector*).

</div>
<div>

### 📐 Peran Dimensi & Kedalaman
Representasi abstrak dari fitur bahasa:

* Posisi setiap angka di dalam vektor disebut **dimensi**.
* Setiap dimensi mewakili "fitur" abstrak teks (misal: formalitas, topik, atau sentimen).
* Membutuhkan **ratusan hingga ribuan dimensi** (384 atau 1.536) untuk merepresentasikan makna secara utuh.

</div>
</div>

---
transition: slide-left
---

# Penyimpanan & Pengindeksan Vektor

Setelah teks diubah menjadi vektor berdimensi tinggi, data harus disimpan dan diindeks secara efisien.

<div class="grid grid-cols-2 gap-6 mt-6">
<div>

### 💾 1. Penyimpanan Data
Menyimpan vektor beserta konteks aslinya:

* Vektor disimpan bersama teks asli (*chunks*) dan metadata (judul, halaman).
* Penyimpanan teks asli krusial agar database dapat mengembalikan teks yang terbaca saat pencarian berhasil.

</div>
<div>

### ⚡ 2. Pengindeksan & Kecepatan
Optimalisasi pencarian kemiripan makna:

* Mencocokkan kueri dengan jutaan vektor satu per satu terlalu lambat.
* Database menggunakan algoritma **ANN (*Approximate Nearest Neighbor*)** untuk mengelompokkan vektor berdekatan.
* Menjadikan pencarian semantik sangat cepat dan efisien.

</div>
</div>

---
transition: slide-left
---

# Word Embedding: Pengantar & Urgensi

Teknik dasar dalam NLP (*Natural Language Processing*) untuk merepresentasikan kata menjadi vektor angka padat.

<div class="grid grid-cols-2 gap-6 mt-6">
<div>

### ❓ Apa itu Word Embedding?
* **Representasi Vektor**
  Teks diubah menjadi koordinat angka padat di dalam ruang vektor kontinu.
* **Menangkap Konteks**
  Memetakan hubungan semantik dan konteks antar kata secara matematis.

</div>
<div>

### 💡 Mengapa Dibutuhkan?
* **Kebutuhan Numerik**
  Jaringan saraf tidak bisa membaca teks mentah — mereka butuh input angka.
* **Mengatasi Metode Lama**
  *One-hot encoding* boros memori (mayoritas nol) dan menganggap semua kata tidak berelasi (misal: "baik" vs "hebat" sejauh "baik" vs "kucing").

</div>
</div>

---
transition: slide-left
---

# Word Embedding: Mekanisme & Cara Kerja

Kata-kata bermakna serupa otomatis diposisikan berdekatan di dalam ruang vektor.

<div class="grid grid-cols-2 gap-6 mt-6">
<div>

### 📍 Kedekatan Semantik
* **Jarak & Arah Vektor**
  Jarak geometris dan arah antar vektor menunjukkan tingkat kemiripan makna (*Semantic Similarity*).
* **Posisi Berdekatan**
  Model meletakkan kata berkonteks mirip (seperti "anjing" dan "kucing") di posisi berdekatan.

</div>
<div>

### 🧮 Aritmatika Vektor
* **Operasi Matematika Kata**
  Karena direpresentasikan sebagai koordinat, kita bisa melakukan operasi untuk menguji relasi makna.
* **Contoh Klasik**
  
  $$\text{King} - \text{Man} + \text{Woman} \approx \text{Queen}$$

</div>
</div>

---
transition: slide-left
---

# Model Word2Vec & Arsitekturnya

Model dari Google (2013) menggunakan *shallow neural network* untuk melatih representasi kata dari korpus besar.

<div class="grid grid-cols-2 gap-6 mt-6">
<div>

### 🔄 Dua Arsitektur Utama
* **CBOW (*Continuous Bag of Words*)**
  Memprediksi kata target berdasarkan kata konteks di sekelilingnya.
  *Contoh:* Memprediksi `[film]` dari `[itu]` dan `[hebat]`.
* **Skip-gram**
  Kebalikan CBOW — menggunakan satu kata target untuk memprediksi konteks di sekitarnya.
  *Contoh:* Memprediksi `[itu]` dan `[hebat]` dari `[film]`.

</div>
<div>

### ⚡ Optimasi: Negative Sampling
* **Tantangan Komputasi**
  Melatih jutaan kata berarti memperbarui ratusan juta bobot di setiap iterasi.
* **Solusi: Negative Sampling**
  Model memilih sekelompok kecil kata negatif secara acak untuk diperbarui, dan mengabaikan sisa jaringan. Membuat pelatihan sangat cepat dan efisien.

</div>
</div>

---
transition: slide-left
---

# Neural Network & Backpropagation

Word2Vec adalah Jaringan Saraf Tiruan sederhana yang dilatih menggunakan algoritma *Backpropagation*.

<div class="grid grid-cols-2 gap-6 mt-6">
<div>

### 🧠 1. Arsitektur Neural Network
*Shallow neural network* dengan satu *hidden layer*:
* **Input Layer:** Kata dalam bentuk *one-hot encoding* (vektor 0 dan satu angka 1).
* **Hidden Layer:** Jumlah neuron = jumlah **dimensi** vektor (misal: 300).
* **Output Layer:** Memprediksi probabilitas kata konteks di sekeliling input.

</div>
<div>

### 🔄 2. Proses Backpropagation
Jaringan melatih diri secara iteratif:
* **Forward Pass & Loss:** Model menebak kata konteks, lalu menghitung tingkat kesalahan (*loss*).
* **Backpropagation:** Algoritma merambat mundur menggunakan *chain rule* untuk menghitung gradien. Bobot di *hidden layer* diperbarui (*Gradient Descent*) hingga kata bermakna mirip memiliki bobot serupa.

</div>
</div>

---
transition: slide-left
---

# Kapan Pelatihan Backpropagation Selesai?

Model tidak dilatih tanpa batas — ada kriteria berhenti yang jelas. **Epoch** = satu kali putaran penuh memproses seluruh dataset pelatihan.

<div class="grid grid-cols-2 gap-6 mt-6">
<div>

### 📉 Kriteria Berhenti (1)
* **Konvergensi Loss**
  Nilai kesalahan (*loss*) sudah sangat kecil dan tidak lagi turun signifikan antar *epoch*.
* **Jumlah Epoch Tercapai**
  Pelatihan dihentikan setelah mencapai jumlah iterasi yang ditentukan (misal: 100 epoch).

</div>
<div>

### 📉 Kriteria Berhenti (2)
* **Early Stopping**
  *Validation loss* mulai **naik** sementara *training loss* masih turun → tanda **overfitting**. Pelatihan dihentikan segera.
* **Patience**
  Loss tidak membaik selama *N* epoch berturut-turut → otomatis berhenti.

</div>
</div>

---
transition: slide-left
---

# Overfitting, Underfitting & Konteks Word2Vec

Memahami kapan model belajar cukup — atau justru terlalu banyak / terlalu sedikit.

<div class="grid grid-cols-2 gap-6 mt-6">
<div>

### 🎯 Dalam Konteks Word2Vec
* Biasanya menggunakan jumlah iterasi tetap atas seluruh korpus (misal: **5–15 epoch**).
* Pelatihan selesai ketika **bobot di hidden layer sudah stabil** — kata bermakna mirip memiliki vektor berdekatan.
* Tidak menggunakan *validation set* seperti model supervised — cukup memantau konvergensi loss.

</div>
<div>

### ⚖️ Overfitting vs Underfitting
* **Overfitting:** Model terlalu "menghafal" data pelatihan → performa buruk pada data baru.
* **Underfitting:** Model belum cukup belajar → loss masih tinggi. Perlu lebih banyak epoch atau arsitektur lebih besar.
* **Titik ideal:** Model cukup belajar pola umum tanpa menghafal detail spesifik data pelatihan.

</div>
</div>

---
transition: slide-left
---

# Lahirnya Word Embeddings

Tujuan melatih jaringan saraf bukan untuk melakukan tebakan — melainkan melahirkan vektor representasi makna.

### 🎯 Ekstraksi Matriks Bobot (Word Embeddings)
Setelah *backpropagation* selesai, lapisan output dibuang. Matriks bobot di **hidden layer** diekstrak sebagai **Word Embeddings** — vektor padat berisi makna semantik.

```mermaid
graph LR
    %% Arsitektur Jaringan Saraf (Neural Network)
    subgraph Word2Vec_Model [1. Arsitektur Neural Network]
        direction LR
        A[Input Layer<br>One-Hot Encoding] -->|Forward Pass<br>Tebakan Awal| B(Hidden Layer<br>Matriks Bobot Acak)
        B -->|Forward Pass| C[Output Layer<br>Prediksi Konteks]
    end

    %% Evaluasi dan Pelatihan (Backpropagation)
    subgraph Training_Process [2. Pelatihan]
        direction TB
        C -.->|Bandingkan dengan Asli| D{Hitung Loss<br>Error}
        D -.->|Backpropagation<br>Koreksi Bobot| B
    end

    %% Hasil Akhir (Word Embeddings)
    subgraph Final_Result [3. Hasil Akhir]
        B ==>|Matriks Bobot Diekstrak| E[(Word Embeddings<br>Vektor Makna)]
    end

    classDef focus fill:#ffe8a1,stroke:#d39e00,stroke-width:2px;
    classDef result fill:#d4edda,stroke:#28a745,stroke-width:2px;
    class B focus;
    class E result;
```

<style>
.mermaid {
  zoom: 0.72;
}
</style>

---
transition: slide-left
---

# Embedding Matrix: Tabel Pencarian Makna

Produk akhir pelatihan Word2Vec — tabel raksasa **V × d** berisi vektor makna setiap kata.

<div class="grid grid-cols-2 gap-6 mt-4">
<div>

### 📊 Struktur Matriks
* **V** = ukuran kosakata (misal 50.000), **d** = dimensi (misal 300)
* Setiap **baris** = vektor makna satu kata

| Kata | Dim 1 | Dim 2 | Dim d |
|---|---|---|---|
| kucing | 0.21 | -0.58 | 0.73 |
| anjing | 0.19 | -0.61 | 0.69 |
| mobil | -0.82 | 0.34 | -0.15 |

</div>
<div>

### 🔍 Cara Kerja Lookup

Pencarian vektor dilakukan melalui perkalian matriks:

$$\vec{e}_{\text{kucing}} = \mathbf{x}_{\text{one-hot}} \times \mathbf{W}_{V \times d}$$

One-hot (satu angka `1`) × matriks → **mengekstrak tepat satu baris** vektor embedding.

</div>
</div>

---
transition: slide-left
---

# Embedding Matrix: Statis vs Kontekstual

Matriks embedding berperan berbeda pada model klasik dan model modern.

<div class="grid grid-cols-2 gap-6 mt-4">
<div>

### 🔄 Statis vs Kontekstual
* **Word2Vec:** Matriks **statis** — vektor kata tetap sama di semua konteks. Kata "bank" selalu memiliki vektor yang identik.
* **Transformer (LLM):** Matriks hanya titik awal (*learnable parameter*). Setelah lookup, vektor dimodifikasi oleh **Self-Attention** sehingga makna berubah sesuai konteks kalimat.

</div>
<div>

### 💡 Peran dalam LLM Modern
* Matriks embedding adalah **lapisan pertama** setiap LLM — mengubah token ID menjadi vektor padat.
* Ukurannya sangat besar: GPT-3 memiliki matriks **50.257 × 12.288** (~617 juta parameter hanya di embedding saja).
* Sering kali matriks yang sama dipakai ulang (*weight tying*) di lapisan output untuk memprediksi token berikutnya.

</div>
</div>

---
transition: slide-left
---

# Cara Kerja LLM: Evolusi ke Transformer

Dari jaringan saraf sederhana menuju arsitektur revolusioner yang mengubah dunia AI.

<div class="grid grid-cols-2 gap-6 mt-6">
<div>

### 🔙 Keterbatasan Pendahulu
* **Word2Vec** menghasilkan vektor statis — kata "bank" selalu sama di semua konteks.
* **RNN (*Recurrent Neural Network*) & LSTM (*Long Short-Term Memory*)** mampu memproses urutan kata, tetapi lambat (sekuensial) dan sulit mengingat konteks jarak jauh.

</div>
<div>

### ⚡ Terobosan Transformer (2017)
* Diperkenalkan oleh Google: paper ***"Attention Is All You Need"***.
* Memproses **seluruh kalimat secara paralel**, jauh lebih cepat.
* Menggunakan **Self-Attention** untuk menangkap hubungan makna antar kata sejauh apa pun.
* Fondasi semua LLM modern: **GPT, Gemini, LLaMA, Claude**.

</div>
</div>

---
transition: slide-left
---

# Mekanisme Self-Attention

Inti kecerdasan LLM: setiap kata "memperhatikan" semua kata lain dalam kalimat secara bersamaan.

<div class="grid grid-cols-2 gap-6 mt-4">
<div>

### 🎯 Konsep Inti
* Setiap token menghasilkan tiga vektor: **Query (Q)**, **Key (K)**, **Value (V)**.
* **Query** = "Informasi apa yang saya cari?"
* **Key** = "Informasi apa yang saya tawarkan?"
* **Value** = "Konten informasi aktual saya."

### 📐 Rumus Attention

$$\text{Attention}(Q, K, V) = \text{softmax}\!\left(\frac{QK^T}{\sqrt{d_k}}\right)\!V$$

</div>
<div>

### 💡 Contoh Intuitif
Kalimat: *"Kucing itu memakan ikan **karena ia** lapar"*

Kata **"ia"** harus merujuk ke **"kucing"**, bukan "ikan". Self-Attention menghitung skor tinggi antara "ia" → "kucing", sehingga model paham konteksnya.

### 🔀 Multi-Head Attention
* Beberapa mekanisme attention jalan **paralel** (*heads*), masing-masing fokus pada aspek berbeda (sintaksis, semantik, referensi).
* Hasilnya digabungkan untuk pemahaman lebih menyeluruh.

</div>
</div>

---
transition: slide-left
---

# Arsitektur Transformer untuk LLM

LLM modern menggunakan bagian **Decoder** dari Transformer secara bertumpuk (*stacked layers*).

```mermaid
graph LR
    A["Token Input"] --> B["Token Embedding"] --> C["+ Positional Encoding"]
    style A fill:#e8f4fd,stroke:#2196F3
    style B fill:#e8f4fd,stroke:#2196F3
    style C fill:#e8f4fd,stroke:#2196F3
```

```mermaid
graph LR
    D["Masked Self-Attention"] --> E["Add & Norm"] --> F["Feed-Forward"] --> G["Add & Norm"]
    classDef focus fill:#ffe8a1,stroke:#d39e00,stroke-width:2px;
    class D,E,F,G focus;
```

```mermaid
graph LR
    H["Linear Layer"] --> I["Softmax"] --> J["Probabilitas Token Berikutnya"]
    classDef result fill:#d4edda,stroke:#28a745,stroke-width:2px;
    class H,I,J result;
```

**1. Pemrosesan Input** → **2. Blok Transformer (×N)** → **3. Lapisan Output**

<style>
.mermaid {
  zoom: 0.75;
}</style>

---
transition: slide-left
---

# Tokenisasi pada LLM Modern

Teks dipecah menjadi unit dasar (*token*) menggunakan algoritma tokenisasi sub-kata sebelum masuk Transformer.

<div class="grid grid-cols-2 gap-6 mt-4">
<div>

### 🔤 Byte Pair Encoding (BPE)
Algoritma tokenisasi paling populer (GPT, LLaMA):
* Dimulai dari **karakter individual**, lalu menggabungkan pasangan paling sering secara iteratif.
* Menghasilkan campuran sub-kata pendek dan kata utuh.

| Teks Asli | Token |
|---|---|
| `"pembelajaran"` | `["pembel", "ajaran"]` |
| `"the"` | `["the"]` |
| `"unhappiness"` | `["un", "happiness"]` |

</div>
<div>

### 🎯 Mengapa Sub-Kata?
* **Efisien**: Kosakata terbatas (32K–100K) tetapi bisa merepresentasikan kata apa pun.
* **Kata baru**: Dipecah menjadi sub-kata yang dikenali.
* **Multibahasa**: Satu tokenizer untuk banyak bahasa.

### 🔢 Positional Encoding
* Transformer memproses paralel → tidak tahu urutan kata.
* **Positional Encoding** menyuntikkan informasi posisi ke setiap token embedding.
* Menggunakan fungsi sinusoidal atau embedding posisi yang dipelajari.

</div>
</div>

---
transition: slide-left
---

# Pelatihan LLM: Pre-Training & Fine-Tuning

LLM dilatih dalam dua fase besar yang membutuhkan sumber daya komputasi luar biasa.

<div class="grid grid-cols-2 gap-6 mt-6">
<div>

### 📚 Fase 1: Pre-Training
Melatih model dari nol pada data skala masif:
* **Tugas:** *Next Token Prediction* — prediksi token berikutnya dari konteks.
* **Data:** Triliunan token dari buku, web, kode, Wikipedia.
* **Komputasi:** Ribuan GPU, berminggu-minggu hingga berbulan-bulan.
* **Hasil:** *Base Model* yang memahami bahasa, logika, dan fakta umum.

</div>
<div>

### 🎯 Fase 2: Fine-Tuning & Alignment
Menyetel model agar berguna, aman, dan patuh instruksi:

* **SFT (*Supervised Fine-Tuning*)**
  Melatih ulang dengan pasangan instruksi-jawaban berkualitas tinggi buatan manusia.

* **RLHF (*Reinforcement Learning from Human Feedback*)**
  Manusia menilai jawaban, lalu model dioptimasi agar menghasilkan respons yang lebih disukai.

* **Hasil:** Model yang mampu berdialog, mengikuti instruksi, dan menolak permintaan berbahaya.

</div>
</div>

---
transition: slide-left
---

# Cara LLM Menghasilkan Teks (Inferensi)

LLM menghasilkan teks secara **autoregresif** — memprediksi **satu token per langkah**, lalu menggunakannya sebagai input berikutnya.

<div class="grid grid-cols-2 gap-6 mt-4">
<div>

### 🔄 Proses Autoregresif

```mermaid
graph LR
    A["Input: Ibu kota Indonesia"] --> B["Prediksi: adalah"]
    B --> C["Prediksi: Jakarta"]
    C --> D["Prediksi: ."]
    D --> E["Selesai EOS"]

    classDef step fill:#e8f4fd,stroke:#2196F3,stroke-width:2px;
    class A,B,C,D,E step;
```

Setiap langkah:
1. Seluruh konteks masuk ke Transformer
2. Model menghitung **distribusi probabilitas** atas kosakata
3. **Satu token** dipilih berdasarkan strategi sampling
4. Token ditambahkan ke konteks, ulangi

</div>
<div>

### 🎲 Strategi Pemilihan Token
* **Greedy:** Selalu pilih probabilitas tertinggi. Cepat, tapi monoton.
* **Temperature:** Nilai rendah → deterministik, tinggi → kreatif.
* **Top-k:** Pertimbangkan hanya *k* token teratas.
* **Top-p (Nucleus):** Kumpulan token terkecil yang totalnya mencapai ambang *p*.

$$P_{\text{adjusted}}(x_i) = \frac{\exp(z_i / T)}{\sum_j \exp(z_j / T)}$$

*T = temperature, z = logits*

</div>
</div>

<style>
.mermaid {
  zoom: 0.75;
}
</style>

---
transition: slide-left
---

# Keterbatasan LLM & Mengapa RAG Diperlukan

Meskipun canggih, LLM memiliki keterbatasan fundamental yang menjadi motivasi utama RAG.

<div class="grid grid-cols-2 gap-6 mt-6">
<div>

### ⚠️ Keterbatasan Utama LLM
* **Halusinasi (*Hallucination*)**
  Jawaban terdengar meyakinkan tetapi bisa sepenuhnya salah — karena hanya prediksi statistik.

* **Batas Pengetahuan (*Knowledge Cutoff*)**
  Pengetahuan berhenti pada tanggal pelatihan terakhir. Tidak tahu peristiwa terkini.

* **Tanpa Sumber Referensi**
  LLM murni tidak bisa mengutip dokumen sumber, sehingga sulit diverifikasi.

</div>
<div>

### ✅ RAG sebagai Solusi
* **Grounding dengan Fakta**
  Dokumen nyata disuntikkan ke prompt — jawaban **berbasis bukti**.

* **Pengetahuan Terkini**
  Database vektor diperbarui tanpa melatih ulang model.

* **Transparansi & Verifikasi**
  Jawaban bisa dilacak ke dokumen sumber aslinya.

* **Domain-Specific**
  LLM menjawab tentang data internal organisasi tanpa *fine-tuning*.

</div>
</div>

---
transition: slide-left
layout: center
class: text-center
---

# Ringkasan: Dari Teks ke Jawaban Cerdas

```mermaid
graph LR
    subgraph Fondasi ["Fondasi"]
        A["Word Embedding"] --> B["Neural Network"]
    end

    subgraph LLM_Core ["Inti LLM"]
        B --> C["Transformer & Self-Attention"]
        C --> D["Pre-Training pada Data Masif"]
        D --> E["Fine-Tuning & Alignment"]
    end

    subgraph RAG_System ["Sistem RAG"]
        F["Dokumen → Vektor DB"] --> G["Pencarian Semantik"]
        G --> H["Konteks + Kueri"]
        H --> I["LLM Menghasilkan Jawaban"]
    end

    E --> I

    classDef foundation fill:#e8d5f5,stroke:#7b1fa2,stroke-width:2px;
    classDef llm fill:#ffe8a1,stroke:#d39e00,stroke-width:2px;
    classDef rag fill:#d4edda,stroke:#28a745,stroke-width:2px;
    class A,B foundation;
    class C,D,E llm;
    class F,G,H,I rag;
```

**Word Embedding** → **Neural Network** → **Transformer** → **LLM** → **RAG**

Setiap konsep membangun di atas fondasi sebelumnya untuk menciptakan sistem AI yang cerdas, akurat, dan dapat dipercaya.

<style>
.mermaid {
  zoom: 0.65;
}
</style>