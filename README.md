# Inventory Open Lag Reducer for G.A.M.M.A.

This is a mod for [S.T.A.L.K.E.R. GAMMA](https://www.stalkergamma.com/) which reduces the UI freeze lag which happens when opening a large inventory. For large inventories, reduces freeze time by up to 700ms. It does not completely remove the freeze, as there are still many other parts to optimize.

## Installing

Download [the latest release](https://github.com/sea-ex/inventory-open-lag-reducer-for-gamma/releases). Don't unzip it. Open Mod Organizer 2 and install the ZIP. Activate the mod. Done.

Mod loading order is not that important, just place it AFTER (below) G.A.M.M.A.'s mods.

## What it does

The mod currently optimizes only the sorting of inventory items, originally provided by RavenAscendant's SortingPlus mod and later modified by many other mods in G.A.M.M.A.

#### Problem description

The SortingPlus mod makes the utils_ui.sort_by_kind function
perform a LOT of work on each comparator call, for both a and b always again.

#### Fix description

This module optimizes the situation wrapping the sorter to a factory
which first does an O(n) pass on the items-to-be-sorted to precompute certain values
that are used in the comparison.
Then, we also patch `utils_ui.UICellContainer.FindFreeCell` for the duration of
`UICellContainer:Reinit` as SortingPlus' overrided version utilizes certain variables
that were computed by the last sort. This was also very slightly optimized.
As a result, **a sort for a very large inventory that previously took
500-700ms now takes 30ms (~90-95% reduction)**.

## Thank yous

Thanks to the original creators of [Sorting Plus](https://www.moddb.com/mods/stalker-anomaly/addons/sorting-plus), G.A.M.M.A. UI and especially HabgierigerWilderer for the creation of the [Anomaly DevTools profiler](https://github.com/ChristopherCapito/stalker-anomaly-devtools) which enabled this optimization work.

## License

The mod contains some scripts which are heavily modified (i.e. derived from) existing mods. Thus there can be no single license covering this entire mod.

1. `src/scripts/seax_sortingplus_opt_sort_by_kind.script`'s sorting functions are based on SortingPlus by RavenAscendant, licensed under CC BY-NC-SA 3.0. The license requires that any derivative works are also licensed under the same license. Thus those portions are licensed under the same license.
2. `src/scripts/seax_sortingplus_opt_sort_by_kind.script`'s `patched_custom_FindFreeCell` and `zzz_ray_sortingplus_mcm.script`'s overrides are based on copypasted code from G.A.M.M.A. UI mod's `utils_ui.script`. It is unclear to me what the license or who the copyright holder is for this other than that the file's header says "Tronex 2020/2/3".
3. Everything else is licensed under MIT License:

   Copyright 2026 sea-ex

   Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

   The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

   THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
