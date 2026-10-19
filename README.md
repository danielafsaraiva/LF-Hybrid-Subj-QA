# Repository for "Light Field Subjective Quality Assessment with Pairwise Refinement"

This repository contains the supplementary data for the paper:

> D. Saraiva, J. Prazeres, M. Pereira, A. M. G. Pinheiro, "Light Field Subjective Quality Assessment with Pairwise Refinement," submitted to IEEE ISCAS 2027.

The test material is the light field benchmark of the JPEG Pleno quality assessment activity: 8 light fields and 144 distorted stimuli (coding-only with JPLM and x265, view interpolation, and 3D Gaussian Splatting reconstruction).

The test has two stages, each with its own group of 16 subjects (no subject took part in both):

1. **DSIS rating stage.** Every stimulus is rated against its reference on the 5-level impairment scale of ITU-R BT.500-15 (1 = Very annoying, ..., 5 = Imperceptible).
2. **Pairwise comparison (PC) stage.** The 155 within-scene pairs with |MOS_i − MOS_j| ≤ 3/16 are compared. Subjects choose one of the two stimuli or "same".

Both stages are combined into the **hybrid quality scale** with Thurstone Case V ([pwcmp](https://github.com/mantiuk/pwcmp)), per light field.

## Files

| File | Content |
|---|---|
| `data/dsis_ratings.csv` | Raw DSIS ratings: one row per stimulus, one column per subject (`S01`–`S16`). |
| `data/pc_votes.csv` | Raw PC votes: one row per subject and pair. `vote` is `A` (stimulus_A preferred), `B` (stimulus_B preferred) or `same`. |
| `data/quality_scores.csv` | Processed scores per stimulus: DSIS MOS and hybrid quality. |

### Columns

- `scene`: light field (Bartender, Bookshelf, Cinema, Glossyshop1, Glossyshop2, Lounge, RuziNiu, Square)
- `stimulus`: stimulus identifier, `<codec>_<scene>_<views>_<rate>` with suffix `_int` (view interpolation) or `_rec` (3DGS reconstruction)
- `codec`, `category`, `bpp`: codec (JPLM or x265), category (coding-only, interpolation or 3DGS) and bitrate in bits per pixel
- `MOS`: mean of the 16 DSIS ratings
- `hybrid_quality`: quality on the hybrid scale (Thurstone Case V, JOD units)

**Note:** hybrid quality values are estimated separately for each light field.

## How the hybrid scale was built

- **PC pairs:** the 16 PC votes are used directly. "Same" votes are split evenly between the two stimuli; an odd remaining vote goes to the stimulus with more decisive votes.
- **All other within-scene pairs (1081):** each DSIS subject contributes one vote, for the stimulus they rated higher. Equal ratings are treated with the same rule as the "same" votes.
- **Scaling:** the resulting comparison matrices are scaled per light field with `pw_scale_table` (pwcmp), using 1000 bootstrap samples.


## License

The data are released under the MIT License (see `LICENSE`). If you use them, please cite the paper above.
