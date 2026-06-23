Dobra ideja — evo svega na jednom mjestu. Organizirao sam po dijelovima da lako nalaziš.

## Postavke analize

Podaci: top-5 europskih liga, prva polovica sezone 2025/26, izvor FBref/Opta. Lijevak filtara: 2447 redaka u sirovom CSV-u → 2379 nakon filtra na top-5 liga → 1278 nakon filtra na minute (Min ≥ 450) → 1243 nakon isključenja golmana.

Uzorak po pozicijskim grupama: DEF 492, MID 441, FWD 310 (ukupno 1243).

Prostor značajki: 9 značajki, sve per-90, ofenzivno/progresivno orijentirane plus kartoni — G-PK_90, Ast_90, xAG_90, npxG_90, PrgC_90, PrgP_90, PrgR_90, CrdY_90, CrdR_90. Sve standardizirane (z-score) unutar pozicijske grupe. Baseline PCA komponenti uz 95% varijance: FWD 8, MID 7, DEF 7.

Četiri uspoređivane mjere: kosinusna sličnost, euklidska udaljenost, skalarni produkt, soft-kosinusna sličnost (s M = Pearsonova korelacijska matrica značajki, računata unutar grupe; provjereno pozitivno semidefinitna).

## 3A — Slaganje metoda

Jaccard@10 (preklapanje top-10 susjeda), ključne vrijednosti: cosine↔euclidean 0.57, cosine↔soft 0.60, cosine↔dot 0.20, euclidean↔dot 0.10, soft↔dot 0.18, euclidean↔soft 0.46.

Spearman (cijelo rangiranje): cosine↔dot 0.95, cosine↔soft 0.94, cosine↔euclidean 0.78, dot↔soft 0.89, euclidean↔dot 0.68, euclidean↔soft 0.74.

Glavni nalaz — inverzija: na vrhu liste (Jaccard@10) tri stilske metrike (cosine, euclidean, soft) čine klaster (0.46–0.60), a dot_product je autsajder (0.10–0.20). Na cijelom poretku (Spearman) obrnuto — dot_product je vrlo blizak kosinusu (0.95), a euclidean je relativni autsajder (0.68–0.78).

Objašnjenje inverzije: dot_product = cosine × magnituda. Množenje pozitivnom normom čuva grubi poredak (visok Spearman), ali volumne igrače gura na sam vrh liste (nizak Jaccard). Euclidean radi obrnuto: lokalno se slaže s kosinusom (slični najbliži susjedi), ali globalno odudara (udaljenost i kut se razilaze u repovima, kod ekstremnih profila).

Konceptualni okvir (dvije osi slaganja): LOKALNO (Jaccard@10, vrh liste) vs GLOBALNO (Spearman, cijeli poredak). dot_product: globalno DA, lokalno NE. euclidean: lokalno DA, globalno NE. cosine↔soft: slični na obje skale (jedini takav par).

Poruka za skauting: za praktičnu upotrebu Jaccard@10 je relevantnija mjera od Spearmana jer skaut gleda vrh liste preporuka, a ne cijeli rep. Visok Spearman uz nizak Jaccard znači "slažu se u sredini/dnu, razilaze na vrhu".

## 3B — Studije slučaja

Četiri ciljana igrača i njihovi profili (z-score): Haaland (FWD, čisti strijelac — G-PK i npxG ≈ +5.3 SD, ostalo oko nule ili negativno), De Bruyne (MID, kreator — xAG +1.51, PrgP +1.77, golovi niski), Hakimi (DEF, ofenzivni bek — PrgR +3.80, PrgP +1.71), Curtis Jones (MID, box-to-box progresor — PrgP +3.61, PrgC +1.23, ali xAG −0.19).

Glavni obrazac: za sve mete tri stilske metrike daju srodne liste najsličnijih, dok dot_product dosljedno izbacuje visoko-volumne igrače. Konkretno: kod Haalanda dot izbacuje Kanea na #1; kod De Bruynea Jeremyja Dokua; kod Hakimija Konrada Laimera; kod Jonesa Vitinhu i Ødegaarda. Lewandowski je #1 kod Haalanda u tri od četiri metrike (kod dot je #2, iza Kanea).

Ključni test De Bruyne ↔ Curtis Jones (oba visok PrgP, ali različita kreacija: xAG +1.51 vs −0.19). Rang Jonesa na listi sličnih De Bruyneu: cosine 23., euclidean 119., dot 13., soft 108. Dakle dot i cosine ih relativno zbližavaju (dijele dominantnu progresijsku os), euclidean i soft ih razdvajaju.

Važna nijansa (da se ne pripiše previše kreaciji): razlaganje euklidske udaljenosti DBV↔Jones po značajkama pokazalo je da razdvajanje kombinira razliku u kreaciji (xAG, 34% ukupne udaljenosti) i razliku u magnitudi progresije (PrgP, 41%). Dakle nije čist test "samo kreacije" — dio razdvajanja dolazi od toga što je De Bruyne "veći" vektor (bogatiji profil). Tablica je bila asimetrična (Jones kod DBV daleko po euclidean/soft, ali DBV kod Jonesa umjereno po svim metrikama), što potvrđuje udio magnitude.

Uokvirenje dot_producta: nije "lošiji", nego mjeri drugo pitanje — "slične akcije u sličnoj količini" umjesto "sličan stil". Za skauting (npr. traženje jednako produktivne zamjene) to je legitiman kriterij.

## 3C — Proxy ground-truth (precision@k)

Position-proxy (pooled, cross-group — udio najsličnijih susjeda iste pozicije), precision@5 / @10: cosine 0.613/0.604, euclidean 0.615/0.597, dot 0.566/0.569, soft 0.601/0.597, random baseline 0.345.

Cluster-proxy, dvije podjele, precision@10:

| Metrika | K-Means (euklid) | Spektral (kosinus) | Δ |
|---|---|---|---|
| cosine | 0.829 | 0.822 | −0.01 |
| euclidean | 0.861 | 0.799 | −0.06 |
| dot_product | 0.709 | 0.712 | +0.00 |
| soft_cosine | 0.848 | 0.846 | −0.00 |
| random | 0.372 | 0.302 | |

Nalaz pozicija: sve metrike ~0.57–0.62, iznad random (0.35), ali umjereno — ~40% najsličnijih je iz druge pozicije. Razlog: čisto ofenzivni prostor ne razdvaja pozicije čisto (ofenzivni bek ≈ krilo). Vezano uz limitaciju (nema obrambenih značajki).

Nalaz klaster (najvažniji metodološki dio): prividna prednost euclideana je ARTEFAKT. Euclidean vodi na K-Means podjeli (0.861), jer je K-Means sam euklidski, ali pada na 3. mjesto na ne-euklidskoj spektralnoj/kosinusnoj podjeli (0.799, Δ −0.06). cosine i soft_cosine ostaju stabilni na obje podjele (Δ ≈ 0). dot_product najslabiji na obje (~0.71), ali stabilno.

Metodološka poanta: nijedna klaster-podjela nije neutralna (svaka nastaje nekom metrikom), pa svaka metrika ima "domaći teren" na podjeli izvedenoj njome. Zaključak o kvaliteti smije se izvući samo iz onoga što vrijedi na obje podjele. Rang se mijenja s podjelom: K-Means → euclidean > soft > cosine > dot; Spektral → soft > cosine > euclidean > dot.

## 3D — Stabilnost

Jaccard@10 perturbirane vs neperturbirane top-liste (veće = stabilnije). Dvije odvojene analize:

| Metrika | A: izbačena značajka | B: Gaussov šum (σ=0.1) |
|---|---|---|
| cosine | 0.632 | 0.677 |
| euclidean | 0.637 | 0.674 |
| dot_product | 0.728 | 0.763 |
| soft_cosine | 0.595 | 0.675 |

Nalazi: dot_product najstabilniji na obje perturbacije (magnituda ga "sidri", otporna na izbacivanje značajke/šum). soft_cosine najosjetljiviji na izbacivanje značajke (0.595), jer se matrica M prekalibrira kad značajka nestane — ali razlika prema kosinusu je mala (0.037), i to je trade-off za bolju diskriminaciju, ne mana. cosine/euclidean u sredini (~0.63–0.68). Izbacivanje 1 od 9 značajki mijenja ~30–40% top-10 (posljedica malog prostora).

## Glavni zaključci (sinteza sva četiri sloja)

Dot_product paradoks: istodobno najstabilniji (3D) i najslabiji po proxy-kvaliteti (3C), te autsajder na vrhu liste (3A/3B). Pokazuje da stabilnost nije vrlina sama po sebi — dot stabilno vraća volumenom vođene susjede. Ne mjeri stil nego količinu; legitimno za neke namjene.

Tri stilske metrike (cosine, euclidean, soft) ponašaju se slično. Razlike među njima su nijansa i stvar konteksta, ne dramatične. cosine je najuravnoteženiji (dobar u 3C na obje podjele, razumno stabilan u 3D). soft_cosine nudi nešto bolju diskriminaciju (3B, razdvajanje DBV/Jones) uz veću osjetljivost na izbor značajki (3D). euclidean se na vrhu liste ponaša slično kosinusu, ali mu je prividna prednost u proxy-mjeri ovisila o euklidskoj prirodi K-Meansa. Iz 3C se NE smije tvrditi "soft > cosine" (razlika unutar šuma); iz cjeline NE "cosine je najbolja metrika" (samo najuravnoteženiji od tri bliske).

Glavni praktični nalaz: na interpretabilnom standardiziranom prostoru sve četiri metrike daju koristan signal (sve >> random). Izbor metrike najviše utječe na vrh liste preporuka (ono što skaut gleda), a ne na grubi poredak. Jedina jasna i robusna razlika jest da dot_product mjeri drugu dimenziju (količinu).

Veza na Steck et al. (2024): njihova kritika "proizvoljnog kosinusa" odnosi se na naučene embeddinge s regularizacijom (sloboda reskaliranja latentnih dimenzija). U ovom postavu — ručne, standardizirane, interpretabilne značajke bez te slobode — patologija se ne manifestira; sve metrike se ponašaju konzistentno. Doprinos rada je empirijska provjera dokle kritika seže, a ne njezino prepričavanje. Nalaz ne proturječi Stecku, nego razgraničava režim u kojem kritika vrijedi (naučeni embeddinzi) od onoga gdje se ne manifestira (fiksne značajke). Oba mogu biti istodobno točna.

## Limitacije

Prostor značajki je ofenzivno orijentiran (nema obrambenih/posjedovnih značajki), pa su golmani isključeni (3 značajke imaju nultu varijancu unutar GK, sličnost degenerirana, prosj. kosinus među golmanima 0.910), a sličnost među braničima slabije izražena. Samo 9 značajki — izbacivanje jedne osjetno mijenja liste. Podaci pokrivaju samo polusezonu 2025/26 (manji uzorak minuta po igraču → veća varijanca per-90 vrijednosti). Proxy ground-truth je posredan (pozicija i klaster), bez prave "zlatne" oznake sličnosti. Neki parametri su donekle proizvoljni (σ=0.1 za Gaussov šum; broj klastera FWD 4/MID 4/DEF 3).

---

To je sve na jednom mjestu. Kad budeš pisao rezultate, ovo ti je referentni list — ne moraš više prelistavati chat. Reci kad želiš skelet za prvi pododjeljak teorije (2.1, 2.2 ili 2.3) pa krećemo dalje.