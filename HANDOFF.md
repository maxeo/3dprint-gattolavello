# GattoLavello — handoff aggiornato 26 settembre 2026

## Obiettivo

Kit compatto del porta-saponi da appoggio con il gatto di `originale\21.stl`, pezzi entro 230 mm per piatto 250 mm. L'utente vuole vedere le modifiche nella sessione Blender aperta tramite MCP (verificare con screenshot del viewport). Il kit lungo non è stato iniziato.

## Stato

Kit compatto completo in digitale, 7 pezzi, STL verificati, guida rigenerata. Non ancora stampato.

**Progetto: `D:\3D Print\GattoLavello\`**, con la stessa struttura degli altri progetti in `D:\3D Print`: in cima STL, `guida.html`, blend del kit, JSON e anteprime; `script\` catena di script + blend donatore (**solo su disco, escluso da git**: se si perde, il kit resta modificabile solo a mano nel blend); `vista_guida\` mesh della guida; `originale\` gatto 21.stl, immagini `riferimento_1…5.png` e pagina di partenza. Primo kit V2, prove, varianti vecchie e salvataggi intermedi sono stati cancellati (nel Cestino).

Git: la cartella è il repo (LFS per .blend e .stl), remote privato https://github.com/maxeo/3dprint-gattolavello, **non ancora pushato** (l'utente ha chiesto di non pubblicarlo per ora). La vecchia cartella di lavoro `C:\Users\matteo\Documents\ChatGPT\3D Print` è superata: conserva solo la vecchia storia git (branch `storico-locale`, ~2,6 GB di LFS, da non pubblicare). Guida pubblica: https://maxeo.github.io/3dprint-guide/gattolavello/guida.html (repo `maxeo/3dprint-guide`, cartella `D:\3D Print\3dprint-guide`, aggiornare con `aggiorna_guide.sh`, che legge da `GattoLavello`; il blend del kit non viene pubblicato perché supera 100 MB).

File di lavoro: `D:\3D Print\GattoLavello\GattoLavello_compatto_kit.blend` (collezione `KIT_STAMPA_compatto`). Donatore di tutti gli script: `script\compatto_pre_anteriore_unico.blend` (contiene i pezzi con i **nomi vecchi** C1_Coda_e_vasca, C2_Base_e_piedi, C3_Testa_petto_zampe, S1, S2, R1 — gli script li caricano con quei nomi, non rinominarli lì).

## Pezzi

Lettere per gruppo: **G = gatto (beige), V = vasca (blu), R/S = piani (verde)**. Numerazione dalla coda alla testa.

| Pezzo | mm | Note |
|---|---|---|
| G1_Parete_e_coda | 109,3 × 133,1 × 193,5 | parete trasversale, parete verticale, mensola del vassoio a filo del retro (X−46,2…−29,5, raccordo concavo R12,5 sotto), perni Ø6 a (−37; ±48), posteriore, coda, piedi posteriori |
| V1_Vasca_posteriore | 134,3 × 144 × 24,2 | metà posteriore vasca, da X−43,35; sedi code di rondine a X85 |
| V2_Vasca_anteriore | 86,4 × 138 × 24,2 | code di rondine verso V1, tenoni verso G2 |
| G2_Testa_petto_zampe | 124,4 × 111,7 × 218,3 | testa, petto, zampe, piedi; piastra del vassoio sagomata attorno a testa e perni (a (168,4; ±48)) |
| R1_Ripiano_intero | 230 × 126,4 × 15,2 | arco sulla nuca (gioco 1,5); 4 fori passanti Ø6,3, staccati dalle feritoie Y±50 su entrambi i lati |
| S1_Grata_coda / S2_Grata_testa | 80 / 105 × 120 × 4 | piastrine/rientranze su entrambi i lati |

Montaggio: G1 → V1 (orizzontale sui tenoni) → V2 (dall'alto nelle code di rondine, scatto) → G2 (orizzontale sui tenoni di V2) → S1, S2 → R1 (perni di G1 e G2). R1 tiene insieme la catena.

## Script (in `D:\3D Print\GattoLavello\script\`)

Rieseguibili, ripartono dal donatore; trovano donatore e altri script in `ROOT / 'script'`, dove ROOT è la cartella del blend aperto. Dentro Blender via MCP: `ns={'__name__':'x'}; exec(open(path,encoding='utf-8').read(), ns); ns['run']()`. Catena completa ~100 s. Ordine obbligato:

1. `anteriore_unico.py` — zampe da `RIFERIMENTO_21_GATTO` cucite a G2, V2 tagliata a X155,98, G2 arretrato 2,8, retro lisciato (fit Z38–56, poi parabola tangente che arriva sul piano verticale X174,7 con pendenza zero; prima c'erano un solco a Z63 e poi uno spigolo a Z65), riempimento petto/vasca, tenoni V2→G2.
2. `vassoio_e_griglia.py` — S2 lato testa; R1 esteso con arco sulla nuca; fori/perni lato testa.
3. `c1_diviso.py` — V1 + G1; tenoni G1→V1; code di rondine V2→V1; piastrine griglie a −Y.
4. `supporti_vassoio.py` — mensola G1 a filo con retro della parete raccordato (curva quadratica Z106→126,8); feritoie Y±50 lato coda di R1 da X−32,75; piastra G2 ridisegnata in pianta (maschere raster 0,5 mm: sagoma testa Z130–146 + collo Z100–116,5 dilatate 8 mm + dischi perni R7,5, chiusura R14, dentro il vassoio di 0,5, apertura R3; contorno polare dal baricentro).
5. `esporta_kit.py` — STL + `geometria.json`; STL non più nel kit → `obsoleti/`.
6. Background (`blender.exe -b <blend> --python <script>`): `verifica_kit.py` → `verifica_stl.json`; `render_kit.py` → anteprime; `anteprime_guida.py` → `vista_guida/*.stl`.
7. `script/crea_guida_kit.py` (Python di Blender) → `guida_spec.json`, poi `build_guide.py <spec>` della skill `guida-stampa-3d` e controllo con `shoot.py` (da **Bash**).

Ultima verifica: 7 STL chiusi, un'isola per pezzo; interferenze solo di contatto (G1/R1 0,001 mm³, V1/G1 2e−5 mm³). Guida senza errori JS.

## Cosa ha funzionato

- Giudicare con misure (raggi BVH, sezioni, volumi d'intersezione), non con l'ombreggiatura.
- Booleane MANIFOLD; `drop_slivers()` dopo le booleane; prismi da poligoni per forme in pianta.
- Forme in pianta complesse: maschere raster + dilatazione/erosione in numpy (scipy non c'è nel Python di Blender), poi contorno.
- Cucitura manuale per lunghezza d'arco; riempimenti/tagli per regione invece di piani unici.

## Cosa non ha funzionato / da non ripetere

- `bmesh.ops.bridge_loops` con anelli di densità diversa; smooth generico sulle giunzioni (scanalature).
- Tolleranze 1e-6 su float32 (usare ≥1e-3).
- `bpy.data.libraries.load`: passare `list(names)`.
- Raggi di `_sagoma` che partono dentro un solido: perdono il primo intervallo (partire da X−150).
- Remesh globale della scultura: vietato.
- PowerShell 5.1 toglie le virgolette doppie negli argomenti.

## Prossimi passi

1. Prova di stampa degli incastri (scatto 0,2 mm, giochi 0,15–0,2) e dei perni del vassoio.
2. Eventuale: la parete di G1 dietro il vassoio (X−54…−46,2) si vede ancora dall'alto come raccordo; per nasconderla del tutto servirebbe assottigliare la parete o accorciare le corna del vassoio.
3. Kit lungo non iniziato.
