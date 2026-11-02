# RaagAI-Outputs

[![DOI](https://zenodo.org/badge/1442937365.svg)](https://doi.org/10.5281/zenodo.23885091)

Example outputs from **RaagAI**, a system that turns a photograph into Hindustani
classical music. Given an image, RaagAI infers its
time of day, mood and season, chooses suitable ragas from the RaagKG
knowledge graph, composes a new piece in the first-ranked raga, and suggests
existing recordings of the chosen ragas on YouTube and Spotify.

This repository holds the complete output of three end-to-end runs. The
RaagAI code is not part of this release.

## The pipeline

1. **Image analysis.** A language model reads the photo and returns its
   prahar (time of day), prakruti (temperament), rasa (mood) and season.
2. **Raga lookup.** These four values are matched against RaagKG
   ([doi:10.5281/zenodo.23813360](https://doi.org/10.5281/zenodo.23813360)),
   a hand-curated graph of Hindustani ragas, which returns the three best ragas.
3. **Composition.** The first-ranked raga's ascent, descent and signature
   phrase are given to GPT-5, which writes Python code that produces a MIDI
   file. The MIDI is rendered to audio with FluidSynth and a General MIDI
   soundfont, with the drum kit tuned to approach the sound of the tabla.
4. **Recommendation.** The three ragas are searched on YouTube and Spotify.

## The three runs

| Run | Image | Time of day | Temperament | Mood | Season | Ragas returned | Composed in |
|---|---|---|---|---|---|---|---|
| 01 | Lakeside at dusk, lights on the far shore | Sunset (6 PM to 9 PM) | Serene | Shanti | Autumn | Puriya Dhanashree, Poorvi, Puriya | Puriya Dhanashree |
| 02 | Sunlight through trees by a river | Early morning (6 AM to 9 AM) | Calm | Shanti | Spring | Bahar, Basant, Kafi | Bahar |
| 03 | Rainbow over green hills under monsoon cloud | Sunset (6 PM to 9 PM) | Tranquil | Shanti | Monsoon | Des, Gaud Malhar, Miyan Ki Malhar | Des |

## What is in each run folder

```
examples/example_01/
  input_image.jpg        the photo given to RaagAI
  output.json            image analysis, ragas returned, recommendations
  generated_draft.mp3    the pipeline's draft rendering (about 65 s)
  generated_final.mp3    the final rendering (about 65 s)
```

`examples/index.csv` lists all three runs in one table.

## Notes on the data

- Photos were taken by the authors. All camera metadata, including location,
  was removed before release; the image data itself is unchanged.
- The music is newly generated. No existing recording is included; YouTube
  and Spotify entries are links only, and those recordings belong to their
  owners.
- Recommendations are recorded exactly as RaagAI returned them. Where a
  YouTube video link was not preserved, the `url` opens a YouTube search for
  the exact title RaagAI returned, and `link_type` is `search`. In run 01,
  Spotify returned the same track for two different ragas; this is kept as
  recorded.

## Licence

All files in this repository are released under
[CC BY 4.0](LICENSE). Please cite this repository (see `CITATION.cff`) and
RaagKG when you use them.

## Related

- RaagKG, the knowledge graph used for the raga lookup:
  https://github.com/RinitJain/RaagKG
