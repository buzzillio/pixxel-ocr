# Pixxel‑OCR: Checkbox Detection Without AI

### 💡 Overview
**Pixxel‑OCR** is a lightweight, deterministic checkbox‑detection method for structured legal forms such as the **California Civil Jury Instructions (LACIV‑244)**.  
It replaces costly and inconsistent vision‑enabled LLM/OCR pipelines with a **simple NumPy‑based pixel arithmetic approach**.

This project originated at the **Stanford Legal Design Lab**, where we needed a reliable way to extract “checked vs. unchecked” states from standardized PDFs.  
By rendering each page at 300 DPI and analyzing 20×20 pixel crops at known coordinates, we achieved **100% accuracy** on our LACIV‑244 test set, **~300× speedup**, and **zero API cost**.

---

### 🚀 Features
- **100% local & offline** — no API calls or data sharing
- **~300× faster** than typical OCR/LLM solutions (≈0.1–0.2 s vs 30–60 s per doc)
- **Deterministic & reproducible** — same input, same output
- **Zero marginal cost** — CPU + NumPy only
- **Extensible** — add Otsu, morphology, or PDF vector parsing if needed

---

### 🧠 How It Works
1. **Render PDF at 300 DPI** using [PyMuPDF (fitz)].
2. **Crop 20×20 px** windows where checkboxes live (fixed coordinates per page).
3. **Convert to grayscale**, invert (`black = 1`), **sum** pixel intensities.
4. **Square the sum** to amplify separation and classify via a simple threshold.


---


Sample JSON‑like output:
```json
[
  {"label": "4301-VF", "value": 3260, "checked": true},
  {"label": "4302-VF", "value": 0, "checked": false}
]
```

---

### 📊 Results (on our LACIV‑244 test set)
| Metric              | Vision LLMs / OCR      | Pixxel‑OCR         |
|---------------------|------------------------|--------------------|
| Speed per document  | 30–60 s                | **0.1–0.2 s**      |
| Cost per 1M pages   | \$6,000–\$10,000       | **\$0**            |
| Accuracy            | 70–95 %                | **100 %**          |
| Offline capability  | ❌                     | ✅                 |

> Costs are typical public price points for document analysis APIs; your region/tier may vary.

---

### ⚖️ Limitations (MECE)
- **Layout Dependence** — assumes a fixed coordinate template (e.g., LACIV‑244).  
- **Image/Rendering Quality** — sensitive to DPI, compression, noise, and grayscale conversion.  
- **Mark Interpretation** — classifies based on interior density (faint ticks or border‑hugging ✓/✗ may need review).  
- **Operational Scope** — requires threshold calibration and versioned configs; provides no semantic understanding.


### 🔗 Medium Article
Read the full story and implementation context:  
**How a Simple Python Script Outran GPT‑5, Claude‑4.5 and Other SOTA Vision LLMs on a Legal OCR Task**  
(Link to be added ...)


### 👤 Author
**Vasyl Rakivnenko** — AI Technical Lead, Stanford Legal Design Lab  
LinkedIn: https://www.linkedin.com/in/vasylrakivnenko