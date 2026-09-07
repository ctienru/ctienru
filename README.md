# Multilingual Podcast Search

## Find the moment, not just the episode.

- Search podcast transcripts.
- Read the matching passage.
- Jump to the audio.

- Built independently by Terri: retrieval, ML infrastructure, and the web experience.

[System architecture](#built-from-ingestion-to-interaction) · [Evaluation](#fragment-level-retrieval-quality)

---

## A passage to read. A moment to hear.

- Find a matching passage with its episode and audio timestamp.
- Explore results through English and Traditional Chinese interfaces.

![Local SAT search results for reinforcement learning, showing transcript context, timestamps, and playback controls.](assets/local-sat-search-reinforcement-learning.png)

*Local SAT search: “reinforcement learning,” with matching passages grouped by episode.*

---

## Listen from the passage.

- Start playback from a passage’s timestamp.
- Keep the episode player visible at the bottom while browsing results.
- Pause, seek, skip back or forward 10 seconds, and adjust playback speed.

![Local SAT audio player with an active transcript passage, episode timeline, and bottom playback controls.](assets/local-sat-audio-player.png)

*Local SAT player opened from 8:40, with playback controls and transcript context visible.*

---

## Built from ingestion to interaction.

- **Web app:** presents search results and audio playback.
- **API gateway:** connects the web experience to retrieval.
- **Offline jobs:** prepare the corpus, embeddings, and indexes.
- **Search service:** loads a validated generation to answer queries.

```text
ONLINE

+-----------------------+    +---------------------------+
| Web app + BFF         |--->| Public API gateway        |
| Next.js / TypeScript  |    | Django REST Framework     |
+-----------------------+    +-------------+-------------+
                                          |
                                          v
                             +--------------------------+
                             | Search API               |
                             | Python / FastAPI         |
                             +------+-------------+-----+
                                    |             |
                                    v             v
                       +-----------------+  +-----------------+
                       | Local artifacts |  | Query embedding |
                       | SQLite + NumPy  |  | Dense / Hybrid  |
                       +--------^--------+  +-----------------+
                                |
                         loaded for serving
                                |
OFFLINE                         |
                                |
                       +--------+----------------+
                       | Validated generation    |
                       | Index, vectors, metadata|
                       +------------^------------+
                                    |
+--------+    +-----------------------------+
| Ingest |--->| Corpus embeddings + indexes |
+--------+    +-----------------------------+
```

**Stack:** Next.js · React · TypeScript · Django REST Framework · FastAPI · SQLite FTS5 · NumPy · multilingual-e5 · Docker

---

## Fragment-level retrieval quality.

- **Corpus:** 589 episode groups · 91,334 fragments.
- **Queries:** 42 frozen queries · 21 Chinese + 21 English.
- **Methods:** BM25, Dense, and RRF fusion · up to 50 candidates per query.
- **Evaluation:** top five fragments · 447 query–fragment judgments graded 0–3.
- **Coverage:** all 630 top-five positions judged.
- **Evidence:** LLM-provisional development labels.

| Method | nDCG@5 | MRR@5 |
| --- | ---: | ---: |
| BM25 | 0.5748 | 0.5988 |
| Dense | 0.6596 | 0.6861 |
| RRF fusion | 0.6641 | 0.6782 |

- **Averaging:** all 42 queries. The separate 41-query conditional average is not shown here.
- **Scoring:** nDCG uses graded gains; MRR requires grade ≥ 2.
- **Verified:** protocol validation, judgment coverage, and deterministic report replay.
- **Pending:** human verification and confidence intervals; method differences remain descriptive.


---

## From a query to a passage.

- Retrieve fragments through keyword and semantic search.
- Fuse their rankings with Reciprocal Rank Fusion (RRF).
- Group matching fragments into episodes for display.

```text
                            Query
                              |
               +--------------+--------------+
               |                             |
               v                             v
         BM25 / FTS5                  Query embedding
               |                             |
               |                             v
               |                        NumPy exact
               |                             |
               +--------------+--------------+
                              |
                              v
                    Reciprocal Rank Fusion
                              |
                              v
                       Rank fragments
                              |
                              v
                       Group by episode
                              |
                              v
                Episode + Evidence + Timestamp
```

- **RRF:** combines rank positions.
- **BM25 / Dense modes:** each uses its own retrieval path.

---

## Replaying a versioned fragment benchmark.

- Run the Search retrieval implementation and save ranked fragments with their evidence.
- Evaluate saved runs against exact query–fragment judgments.
- Recompute and verify the report through doctor and replay.
- Scope: fragment retrieval quality in the bound evaluation experiment.

```text
Transcript fragments + frozen queries
                    |
                    v
       Validated search generation
                    |
                    v
      BM25 / Dense / RRF retrieval
                    |
                    v
       Saved fragment run matrix
                    |
                    v
 Exact fragment judgments --> Benchmark evaluator
                                      |
                                      v
                         Report + doctor + replay
```

- **Display unit:** episodes with matching passages.
- **Evaluation unit:** individual fragments.
- **Experiment identity:** dataset, corpus, generation, retrieval profiles, and code version.
- **Environment:** dedicated evaluation corpus and generation.
- **Language slices:** query language; episode-language filtering requires its own validation.

