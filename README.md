# AFIB-RAG — Clinical Decision Support (versi dibetulkan)

RAG-based Clinical Decision Support System untuk keputusan anticoagulation
dalam Atrial Fibrillation, merentasi 10 guideline antarabangsa (ESC, ACC/AHA,
NICE, MOH Malaysia, KDIGO, APHRS, Canada, Brazil, EHRA, ISTH).

## Apa yang dibetulkan dari repo asal (`AFIB-RAG-1-`)

1. **Retrieval re-ranking** — sebelum ni, top-3 chunk per guideline diambil lalu
   dipotong ke top-5 secara urutan iterasi (bukan skor kesamaan). Sekarang guna
   `similarity_search_with_score` + `sort()` global merentasi semua 10 store,
   supaya chunk paling relevan sebenar yang dihantar ke LLM — bukan sekadar
   yang kebetulan berada dalam store pertama.
2. **Metadata key untuk citation** — `get_guideline_name()` sekarang cuba
   beberapa nama key (`source_guideline`, `source`, `guideline`, `guideline_id`)
   supaya citation `[Guideline_ID | Section]` dalam jawapan tak silently
   fallback kepada label generik "Guideline".
3. **Expander Debug** — buka "🔍 Debug" selepas jana jawapan untuk lihat
   metadata sebenar bagi setiap chunk yang diambil. **Guna ni untuk sahkan**
   nama key metadata anda betul-betul sepadan dengan yang di-assume dalam
   `get_guideline_name()` / `get_section_name()` — kalau tidak, tambah nama
   key sebenar ke dalam list tersebut.
4. **`requirements.txt` dikunci** — versi langchain/langchain-community
   disamakan dengan sempadan yang sudah terbukti berfungsi dalam Colab anda,
   supaya deploy di Streamlit Cloud tak pecah bila langchain keluarkan versi
   baharu.

## Cara upload repo baharu ke GitHub (tanpa command line)

1. Pergi ke **https://github.com/new/import**
2. Pada "Your old repository's clone URL", tampal:
   `https://github.com/nuhaheq/AFIB-RAG-1-`
3. Beri nama repo baharu (contoh `AFIB-RAG-v2`), pilih Public/Private, klik
   **Begin import**. Ini akan clone semua fail termasuk 10 zip FAISS anda —
   anda tak perlu re-upload fail besar tu satu-satu.
4. Bila import siap, buka repo baharu → klik `app.py` → klik ikon pensel
   (Edit) → select-all & delete kandungan lama → paste kandungan `app.py`
   dari sini → **Commit changes**.
5. Ulang langkah sama untuk `requirements.txt`.
6. Klik **Add file → Create new file**, taip `README.md`, paste kandungan
   fail ni → **Commit changes**.

## Deploy ke Streamlit Cloud

1. Pergi ke **share.streamlit.io** → **New app** → pilih repo baharu anda →
   fail utama `app.py`.
2. Dalam **App settings → Secrets**, tambah:
   ```
   GEMINI_API_KEY = "key_sebenar_anda"
   ```
3. Deploy. Bila app dah jalan, cuba satu query dan buka expander Debug untuk
   sahkan citation keluar dengan nama guideline sebenar (bukan "Guideline"
   generik).
