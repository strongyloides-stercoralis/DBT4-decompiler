# Budokai Tenkaichi 4 mod (SLUS_219.78) — stage 1

The mod is a **binary patch of the USA executable** (SLUS_216.78), not a European build. Compared with the USA
program: the section layout is identical, 2,258 of 523,360 words differ, spread over about 200 functions. The
overlay BIN/DBZP.BIN is 6,840 bytes longer (new code appended) with 847 changed words in the common part, and
BIN/DBZ4.BIN is new: it holds the preloaded file tables of four archives (50,503 / 3,400 / 32,774 / 30,000 files).

| | USA | mod |
|---|---|---|
| program | SLUS_216.78 | SLUS_219.78 |
| archives | pzs3us0..2.afs | pzs4us0..3.afs |
| movies | zs3usop / zs3used | zs4usop / zs4used |
| save id | BASLUS-21678DBZT3 | BASLUS-21978DBZT4 |

## What stage 1 does

- Setup (`port/setup/native.cpp`) accepts either disc, by the checksums in `kVariants`, and unpacks all four archives.
- The game picks the variant at start by which program file is in `gamedata/disc/` (`gPortVariant`, plat_mem.c).
- The data-table hash of the USA release is not required for the mod; the file check (plat_verify.c) is off for it.
- `File_OpenById` / `File_StartAdxById` (src/sys/file.c) use the mod's id -> (archive, index) rule, decoded from
  its code at 0x100954, 0x19BDC4, 0x100974, 0x19BE04.
- Movie names, the voice path of netplay and the netplay save folder follow the variant.

## What it does not do (stage 2)

The game logic the mod changed is still the USA's: about 200 functions (battle: `BtlSkill_Apply`, `BtlObj_*`,
`BtlLoad_*`, `BtlAct_SetFormRandom`, `BtlSuper_GetThrow*`; HUD: `HudGauge_*`; effects: `EftBladeMgr_Init`; and the
appended code of DBZP.BIN). Until each is ported, the mod's new characters and moves will be wrong or missing.
`exe_diff_report.md` lists the changed functions. Stages and songs added from outside the disc (`stages/`, `songs/`)
and the source build with `install.py` are not adapted to the mod. Nothing here was run: this was written without a
compiler for the whole port.
