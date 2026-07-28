# Player-similarity-football-2

Projekt za analizu sličnosti nogometnih igrača koristeći skup podataka `igraci.csv` i Jupyter bilježnicu `projekt.ipynb`.

**Svrha**
- Istražiti metrike i karakteristike igrača.
- Izračunati sličnost igrača (feature-based similarity, clustering, itd.).
- Dokumentirati nalaze u `zakljucci.md`.

**Sadržaj repozitorija**
- `igraci.csv` — izvorni skup podataka o igračima (atributi, statistike).
- `projekt.ipynb` — Jupyter bilježnica s analizom, čišćenjem podataka i modelima.

Kako koristiti
1. Otvorite projekt u Jupyter Notebook / JupyterLab ili VS Code i otvorite `projekt.ipynb`.
2. Osigurajte Python okruženje s potrebnim paketima (npr. `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`).

Preporučeni koraci za pokretanje (u virtualnom okruženju):

```bash
python -m venv .venv
.\.venv\Scripts\Activate.ps1   # PowerShell
pip install -U pip
pip install pandas numpy scikit-learn matplotlib seaborn jupyter
jupyter notebook projekt.ipynb
```

Napomene
- Provjerite encoding i separator datoteke `igraci.csv` prije učitavanja (npr. `pd.read_csv('igraci.csv', encoding='utf-8', sep=',')`).
- Ako imate dodatne zahtjeve za reprodukciju eksperimenata, javite i mogu dodati `requirements.txt` i upute za reproduciranje.

Kontakt
- Ako trebate pomoć ili imate pitanja, otvorite issue ili pošaljite poruku autoru.
