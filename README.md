# 🍝 Meatball Tracker

Sdílená **myšlenková mapa + checklist** pro co-op hraní MeatballCraftu (a cokoliv dalšího).
Žije na **https://dukrcz.github.io/meatball-tracker/** — otevřou ji dva lidi a vidí totéž živě.

---

## Základní koncept

- **Položka = bublina na mapě = řádek v levém seznamu.** Jedna věc, dva pohledy.
- **Vztahy = strom**: bublina může mít rodiče (vstupy) a potomky. Druhý rodič jde přidat tažením z modré tečky.
- **Sekce** = barevné štítky (Main quest, Magie, Energie…). Položka jich může mít víc — barvy se míchají jako pigmenty a dělají barevné „biomy" na pozadí. Sekce se dědí na potomky.
- Vše se **ukládá samo** a synchronizuje přes Supabase (~2 s). Sync je **merge po položkách** — když oba upraví naráz, nic se neztratí.

## 🎯 Focus bar (kotva)

Velký svítící pruh nahoře: **co právě děláš + jak dlouho**.
- Klik na prázdný bar → vyber úkol. Nebo pravý klik na bunku → **„🎯 Dělám tohle"** (rovnou tě i přiřadí).
- **✓ Hotovo** přímo z baru. **⏱ Historie** = co jsi dělal naposled (klik = skok na položku).
- Per osoba — brácha má svůj.

## Mapa — ovládání

| Akce | Jak |
|---|---|
| Nová položka | 2× klik do prázdna / pravý klik → Nová položka |
| Přejmenovat | 2× klik na název (Tab = skoč na další prázdnou) |
| Poznámky/detail | 2× klik na tělo bubliny, nebo 📋 |
| Potomek / rodič | ＋ vpravo dole (výstup) / ＋ vlevo dole (vstup); Tab/Enter na vybrané |
| Napojit existující | táhni z modré tečky rodiče na dítě |
| Stav | klik na čtvereček (volný → dělá se → hotovo → stopped) |
| Posun jedné bubliny | táhni; **Shift+táhni = celá propojená skupina** |
| Výběr více | levým tažením po prázdnu (gumička) → hromadný posun / pravý klik = hromadné menu / Delete |
| Zoom / posun mapy | kolečko = zoom · prostřední tlačítko nebo mezerník+tah = posun |
| Velikost bubliny | táhni za okraj/roh |
| Zpět / vpřed | Ctrl+Z / Ctrl+Y (i tlačítka ↶↷) |
| Hledání | **Ctrl+F** nebo 🔍 — Enter = další shoda |
| Sbalit větev | klik na badge „3/5" na bublině (ukáže ▸ +N), znova = rozbalit |
| Skrýt hotové | tlačítko **Skrýt ✓** v liště (pamatuje si) |
| Smazat | × na bublině, Del; **Shift+Del / „Smazat celou skupinu"** = celý strom |

**Zelený pulz + ▶ 2/2** = všechny vstupy hotové → můžeš začít. Badge „hotovo/vstupů" je na bublině i v seznamu. **Avataři** na bublině = kdo to dělá (klik = přiřadit).

## Levý panel

- Třídění: **Strom** (zanoření, klik = skok na bublinu) / **Sekce** / **Lidi**.
- **Teď** = jen rozdělané, seskupené podle lidí, v pořadí jak se začalo.

## 🐝 Včely

**Šablona → 🐝 Strom z databáze**: vyhledej cílovou včelu (305 receptů: MeatballCraft, Forestry, MagicBees, CareerBees, ExtraBees — soubor `beedb.json`).
- Náhled = **rozbalovací seznam** (klik na jméno rozbalí vstupy).
- **✓ = mám tuhle včelu** → větev se utne (strom řeší jen co chybí). Vlastněné se sdílí a pamatují.
- **Vložit na mapu** = rodokmen, cíl nahoře, vstupy dolů (duplikované = žádné křížení). Pravý klik na včelu na mapě → „🐝 Mám tuhle včelu".
- Skutečné ikony včel nejdou získat (jeden item barvený genomem za běhu) — místo nich barevná tečka podle druhu.

## 🧱 Materiál / ingredience (craftění)

V 📋 detailu: řádky materiálu (množství umí počty: `9*64`, `2*27+5`) + násobič „potřebuju ×N".
- **Checkbox ✓ „mám"** u každé ingredience — seznam je vidět **přímo v bublině** a kliká se tam (✓ přeškrtne). Dole „3/9 mám".
- **Build log** (tlačítko nahoře) = součet **chybějícího** materiálu přes celou tabuli.

## Obrázky a poznámky

- **Ctrl+V vloží obrázek** (do detailu / vybrané bubliny / jako novou položku). V detailu ＋, drag&drop, přeskládání tahem.
- V bublině mřížka (1–3 sloupce volitelně), klik = lightbox (šipky, Esc).
- Poznámky bubliny se zobrazují přímo v ní (klik = editace).

## Šablony

Tlačítko **Šablona** (nebo pravý klik): Brainstorm / Cíl→kroky / Postup / Projekt / Denní plán / Rozhodnutí / Pro-proti — **ptají se na počet kroků**; když máš vybranou bunku, pověsí se pod ni. Dále **✨ Vytvořit strom (klikací)** — formulář: jméno + „＋ vstupy" do hloubky.

## Sdílení a data

- Data: Supabase projekt `gfftiddaztxqvsaivanq`, tabulka `boards` (room `meatball`), klient = čisté REST + 2s poll, přihlášení zadrátované v `index.html` (DEFAULT_SUPA). Free tier; pozor jen na ~7 dní úplné nečinnosti (projekt usne, v dashboardu se probudí).
- **Toasty**: změny od bráchy se hlásí vpravo dole.
- Tlačítko **Sdílet** = zvací odkaz / vlastní Supabase / vypnutí.

## Technika & deploy

- Celá appka = **jeden `index.html`** (vanilla JS, žádný build). Zdroj: `/home/dukr/projects/meatball-tracker/` (git), kopie `C:\projekty\meatball-tracker\`.
- Hosting: **GitHub Pages**, repo `DukrCZ/meatball-tracker`, branch `main`, root. Deploy = přepsat `index.html` přes GitHub contents API (potřeba PAT se scope `repo`; tělo requestu posílat ze souboru — inline base64 je moc velký na argv). Rebuild ~1 min.
- Po updatu v prohlížeči **Ctrl+Shift+R** (cache).
- Stav (`boards.data`): `{sections[], items[], members[], owned[], activity[], deleted{}, waypoints{}, layoutDir, rev}`; položka: `{id,text,status,assignees[],parentId,links[],sectionIds[],x,y,w?,h?,notes,materials[{n,q,h}],mult,images[{src,cap}],imgCols,bee?,col?,mt,doingTs?}`. Merge: novější `mt` vyhrává, mazání přes `deleted` tombstony (7 dní).

*Data včel: [BeeBreeding (at-l4s)](https://github.com/at-l4s/BeeBreeding), díky.*
