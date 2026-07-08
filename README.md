# Image Generation with Stable Diffusion

Proyek submission **Image Generation** — implementasi Text-to-Image, Image-to-Image (Inpainting & Outpainting), dan interface interaktif menggunakan **Stable Diffusion** (`runwayml/stable-diffusion-v1-5` & `runwayml/stable-diffusion-inpainting`) yang dibungkus dalam aplikasi **Streamlit**.

## 🔗 Demo

**Live demo (Streamlit via Ngrok):**
👉 [https://eleven-stoning-upriver.ngrok-free.dev](https://eleven-stoning-upriver.ngrok-free.dev)

> **Catatan:** Link di atas bersifat **sementara**. Link ini hanya aktif selama notebook Colab/Kaggle sedang dijalankan (session-based). Jika link tidak bisa diakses, kemungkinan besar runtime sudah berhenti — jalankan ulang notebook `Pipeline`/`Streamlit` untuk mendapatkan link ngrok yang baru.

## Struktur Proyek

```
├── Pipeline_submission_BFGAI_Annisa_Yusri.ipynb     # Eksperimen: Text-to-Image & Image-to-Image
├── Streamlit_submission_BFGAI_Annisa_Yusri.ipynb    # Interface Streamlit (logic.py + app.py)
└── README.md
```

## Fitur

### 1. Text-to-Image
- `generate_simple_image()` — generate gambar dasar (prompt, negative prompt, seed)
- `generate_advanced_image()` — generate dengan kontrol tambahan (`guidance_scale`, `num_inference_steps`)
- Eksperimen pengaruh **Guidance Scale** (rendah vs tinggi) dan **Inference Steps** (rendah vs tinggi)
- Batch inference 4 gambar sekaligus, ditampilkan dalam grid 2×2
- `load_scheduler(pipe, scheduler_name)` — switching sampler tanpa reload model: **Euler A**, **DPM++**, **DDIM**

### 2. Image-to-Image
- `inpaint_engine(image, mask, prompt, ...)` — inpainting menggunakan `runwayml/stable-diffusion-inpainting`
- Manual masking (hardcoded, trial and error) & automatic masking (segmentation model)
- `prepare_outpainting()` — memperluas kanvas ke satu arah tertentu
- Outpainting bertahap ke berbagai arah (**Zoom Out**)
- Two-stage generation **Base + Refiner** (menggunakan komponen SD 1.5 yang di-reuse untuk efisiensi VRAM/RAM)

### 3. Interface Streamlit
- Input prompt & negative prompt
- Slider `guidance_scale` dan `num_inference_steps`
- Tombol **Generate**, hasil gambar tampil langsung di layar
- Input jumlah gambar (`num_images`) + tampilan grid 2×2
- Dropdown pilihan scheduler (Euler A / DPM++ / DDIM)
- Tombol **Clear Memory** (`gc.collect()` + `torch.cuda.empty_cache()`)
- Tab **Edit** untuk Inpainting & Outpainting (Zoom Out), terintegrasi dengan `streamlit-drawable-canvas` untuk menggambar mask langsung di browser

## Tech Stack

| Komponen | Library/Model |
|---|---|
| Text-to-Image | `diffusers`, `runwayml/stable-diffusion-v1-5` |
| Inpainting | `diffusers`, `runwayml/stable-diffusion-inpainting` |
| Segmentation (automask) | DETR Panoptic |
| Interface | `streamlit`, `streamlit-drawable-canvas` |
| Tunneling | `pyngrok` |
| Compute | GPU T4 (Google Colab Free Tier) |

## Cara Menjalankan

1. Buka notebook `Pipeline_submission_BFGAI_Annisa_Yusri.ipynb` di **Google Colab** (aktifkan GPU: `Runtime > Change runtime type > T4 GPU`)
2. Jalankan seluruh cell secara berurutan (`Runtime > Run all`) untuk eksperimen Text-to-Image & Image-to-Image
3. Buka notebook `Streamlit_submission_BFGAI_Annisa_Yusri.ipynb`
4. Masukkan **Ngrok Authtoken** milik sendiri pada cell yang tersedia (daftar gratis di [ngrok.com](https://ngrok.com/))
5. Jalankan seluruh cell — aplikasi akan otomatis membuat file `logic.py` & `app.py`, menjalankan Streamlit, dan menghasilkan link publik via ngrok
6. Buka link ngrok yang muncul di output untuk mengakses interface

## Contoh Hasil

Gambar astronot di permukaan Mars (Text-to-Image) dan penambahan broken satellite (Inpainting) — lihat detail lengkap beserta perbandingan eksperimen di dalam notebook `Pipeline_submission_BFGAI_Annisa_Yusri.ipynb`.

## Author

**Annisa Yusri** — Submission untuk kelas *Belajar Fundamental Generative AI* (Dicoding)
