# THUG2 THPS2 Level Converter

> **Not affiliated with or endorsed by Activision, Neversoft or any other rights holder.** A non-commercial fan project
> for preservation, interoperability and education. No game files are included: the tools work only on copies of the
> games you own. Game names are trademarks of their owners. See LICENSE (MIT for the project's own work; no licence for material
> from the games); third-party parts (downloaded tools) are listed in THIRD_PARTY_NOTICES.md. Rights holders: open an
> issue and it will be dealt with promptly.

Converts the ten levels of your THPS2 install (its ALL.PKR) from your own copy of Tony Hawk's Pro Skater 2 (PC) to the formats of Tony Hawk's Underground 2 (PC), for its level
select. Nothing of either game is in this repository: the files are made from your copies each time. How it works,
what is converted and what isn't: [thps2_levels/README.md](thps2_levels/README.md).

- `thps2_levels/` - the converter (`thps2_levels.ts`: `convert`, `selfcheck`, `list`) and its readers of THPS2's files (`pkr.ts`: the ALL.PKR archive; `thps2.ts`: its level formats).

## Dependencies

Each pinned to a tagged commit in package.json (`git+https://git@github.com/...#<commit>`; one copy of each: every repository that uses one pins the same commit):

- [thug2-level-converter-core](https://github.com/CanuckAussieKev/thug2-level-converter-core): THUG2's formats (scenes, collision, textures, archives, scripts), the level port's common steps and the THUGPRO compatibility register. THPS2's own formats are read here: no other converter's code is needed.

## Public API

What other repositories may import - package.json's `exports`, nothing else (`import ... from "thug2-thps2-level-converter/thps2_levels/pkr.ts"`):

- `thps2_levels/pkr.ts` - THPS2's ALL.PKR archive, read (the content pipeline's loading pictures come from it).

Commands (package.json's `commands`: the tools with a command line - importing a module never runs one): `thps2_levels/thps2_levels.ts`.

## Usage

Needs Node 24 and NeverScript's `ns.exe` (QB scripts compiled): the
[THUG2 decompilation](https://github.com/CanuckAussieKev/thug2-decomp) builds it (`tools\build_neverscript.ps1` ->
`out\neverscript\ns.exe`, found there when this converter is used inside it) - or pass `--ns <ns.exe>`.

    git clone https://github.com/CanuckAussieKev/thug2-thps2-level-converter
    cd thug2-thps2-level-converter
    npm ci
    node thps2_levels/thps2_levels.ts convert thps2_hangar --thps2 "C:\Games\THPS2" --thug2 C:\Games\THUG2 --ns <ns.exe>
    node thps2_levels/thps2_levels.ts selfcheck --thug2 C:\Games\THUG2

`convert` writes `<thug2>\thug2-decomp-thps2\` (not read by the game); without level names it converts the default
set (thps2_levels/README.md). `--hd-textures` upscales the textures with Real-ESRGAN (optional: the content pipeline's `hd_textures/fetch.ts` downloads it). The [THUG2 content pipeline](https://github.com/CanuckAussieKev/thug2-content-pipeline)'s
`menu_level/make_menu_level.ts` then installs every converted game's levels into the game's data overlay and level
select. In the decomp, where the pipeline installs this converter, the same commands are

    node tools/pipeline/run.ts thps2_levels/thps2_levels.ts convert --thug2 C:\Games\THUG2
    node tools/pipeline/menu_level/make_menu_level.ts --game C:\Games\THUG2

Checks: `npm run typecheck`, `npm run lint`, `npm test` (the tests that need a game install skip without it;
`package.test.ts` checks package.json's exports, commands and pins). GitHub Actions runs them on every push
(`.github/workflows/ci.yml`).

## Versions

Releases are tagged `v<major>.<minor>.<patch>`; this is `v1.0.0`. The repositories that use it pin the tagged commit.
