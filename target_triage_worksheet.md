# Target Triage Worksheet
### Is your protein an *easy* or *hard* binder target?

**Time:** ~60–90 min per target  
**Tools:** UniProt (uniprot.org) · RCSB PDB (rcsb.org) · ChimeraX (on your machine)

**My target:** ________________________

---

## The big idea


Three questions decide everything:

1. **Can a binder physically reach the patch?**
2. **Is the surface grippable?**
3. **Do we even have a good structure of it?**

That's all you're scoring today.

---

## Part 1 — Find your target on UniProt

1. Go to **uniprot.org**, search the target name.
2. Pick the **reviewed (Swiss‑Prot — gold star)** human entry unless told otherwise.
3. Write it down:

| Field | Your answer |
|---|---|
| Protein name | |
| Accession (e.g. P00533) | |
| Length (aa) | |
| Understand the function of the protein

---

## Part 2 — Where does it live? *(Subcellular location)*

This decides **which part of the protein you're even allowed to target.**

Open the **Subcellular location** section. Tick one:

- [ ] **Secreted** - whole protein is fair game *(usually friendliest)*
- [ ] **Single‑pass membrane** - target the **extracellular domain only** 
- [ ] **Multi‑pass / GPCR** - only tiny extracellular loops *(note it*)
- [ ] **Cytoplasmic / nuclear** - harder to reach inside a cell *(note it)*

Now open **Family & Domains → Topology / Features** and record the map:

| Region | Residues |
|---|---|
| Signal peptide | – |
| Transmembrane region(s) | – |
| **Extracellular domain you'll target** | **–** |


---

## Part 3 — Surface red flags *(still in UniProt)*

Scroll **PTM / Processing** and **Features**:

- N‑linked glycosylation sites (count): ____ → sugars shield the surface; **more = harder to reach the surface**
- O‑linked glycosylation? [ ] yes [ ] no
- Disulfide bonds: ____ *(fine — just note them)*
- Disordered / low‑complexity regions? [ ] yes [ ] no 

---

## Part 4 — Find the BEST structure

1. In UniProt, open the **Structure** section - you'll see experimental PDB entries **+** an AlphaFold model.
2. Click into **RCSB PDB** for the candidates.
3. Pick the best using this ladder (top = most important):

| Want this | Because |
|---|---|
| Covers your target domain | Can't design against missing residues |
| Experimental (X‑ray / cryo‑EM) | 
| Resolution **< 2.5 Å** | Sharper = more reliable surface |


4. **No experimental structure** for your domain? → use the **AlphaFold model**, but only trust the high‑confidence (blue) regions.

| Field | Your answer |
|---|---|
| PDB ID (or AF model) | |
| Method | |
| Resolution (Å) | |
| Target chain | |

---

## Part 5 — Inspect it in ChimeraX

Open ChimeraX. Type commands in the bottom command line. Replace `1IVO` with **your** PDB ID.

**Load & clean**
```
open 1IVO          # your PDB ID
hide solvent       # remove waters
```
If there are extra chains/copies, keep just your target:
```
delete ~/A         # keeps ONLY chain A — change A to your chain
```

**① Reachability** — show the surface, spin it.
```
surface
```
Is your target patch out in the open? [ ] yes [ ] no *(buried = hard)*

**② Grippability (hydrophobicity)** — binders like a mix; all‑charged = slippery.
```
mlp
```
Goldenrod = hydrophobic, cyan = hydrophilic.
Your patch: [ ] some goldenrod *(easier)* [ ] all cyan *(harder)*

**③ Glycan check** — sugars block binders.
```
show :NAG :BMA :MAN
```
Any sugars sitting **on your patch**? [ ] yes *(harder)* [ ] no

**④ Flexibility** — rigid is good, floppy is hard.
```
color byattribute bfactor
```
Blue/low = rigid *(good)*. Red/high = floppy *(hard)*.
> *On an AlphaFold model this colors confidence (pLDDT), not motion.*

**⑤ Shape** — pockets and grooves are easier to grip than flat slabs.
Eyeball it: [ ] pocket / groove [ ] flat [ ] convex bump

---

## Part 6 — Verdict: Easy or Hard?

Based on everything we did till now lets Tally what we saw and triage weather its a easy target or hard target.
list all the properties that you will be considering.

## Part 7 — Prep the input

1. **Trim** to just the chain/domain you'll design against:
   ```
   delete ~/A         # keep only your target chain
   ```
   *(Want only a sub‑domain? Also delete residues outside your aa range.)*

2. **Glycans:** for a first run, usually **remove** sugars —
   *unless* the glycan IS the binding site. Note your choice: ____________

3. **Pick 2–4 candidate hotspot residues:** surface‑exposed, in/near your patch,
   ideally on the functional epitope.
   → Residue numbers: ______ , ______ , ______ , ______

4. **Save the cleaned structure:**
   ```
   save mytarget_clean.pdb
   ```

5. **Binder length range** you'll request (e.g. 55–120 aa): ____________

---

## Submit this

- [ ] Cleaned PDB file
- [ ] Target chain ID
- [ ] Hotspot residue list
- [ ] Easy / Medium / Hard verdict **+ one reason**
