# datasets

Reasoning datasets built from code and labelled by code. Every row is
re-derived by an independent verifier that does not import the generator, and
kept only if the verifier reproduces the label exactly.

Download them on Kaggle, or browse them on one page:
**[datasets-chi.vercel.app](https://datasets-chi.vercel.app)**

| dataset | what it trains | input (real test example, shortened) | expected output | rows (train / val / test) |
|---|---|---|---|---|
| [Contradiction Ledger](https://www.kaggle.com/datasets/muhammadhammas13/contradiction-ledger) | Long-document consistency: find facts stated one way and later another | A 5-20 page lease: *"The lease runs for 3 years from 2020-12-05 ..."* and, pages later, a different start date and rent | `{"n_contradictions": 2, "contradictions": [{"type": "temporal", "text_a": "2020-12-05", "text_b": "2020-02-05"}, {"type": "numeric", "text_a": "EUR 12k", "text_b": "EUR 5k"}]}` | 40,000 / 2,000 / 4,000 |
| [WitnessDrift](https://www.kaggle.com/datasets/muhammadhammas13/witnessdrift) | Source reliability: rebuild an event from partly-wrong accounts | Five witness statements: *"[W1] taken 180 days after ... I was on the scene between 11:51 and 11:56 ... I'm fairly sure that at about 11:38, Colin pushed past Grace"* | Each witness's reliability (`"W2": 0.78`, `"W1": 0.17`), the most and least reliable, and the recovered timeline `["11:38", "Colin", "pushed past Grace", "the pavement outside"]` | 50,000 / 2,500 / 5,000 |
| [FraudTrail](https://www.kaggle.com/datasets/muhammadhammas13/fraudtrail) | Financial investigation: follow the money, name the scheme or say there is none | A ledger of 16 entities and 45-235 transactions, with red herrings | `{"scheme": "none", "entities": [], "evidence_rows": []}` - or a named scheme (layering, round-tripping, kickback, ...) with the exact rows that prove it | 35,000 / 2,000 / 4,000 |
| [TimelineForge](https://www.kaggle.com/datasets/muhammadhammas13/timelineforge) | Temporal reasoning: absolute times from a story told out of order | *"At 2024-03-20 01:13 PST, Noor noted that they landed. 10 days before they landed, the visa arrived. 12 hours before the visa arrived, the flights were booked ..."* | Every event in UTC, in order - `"the flights were booked": "2024-03-09 21:13"`, `"the visa arrived": "2024-03-10 09:13"`, ... - and the events the text cannot place | 60,000 / 3,000 / 6,000 |
| [DeEscalate](https://www.kaggle.com/datasets/muhammadhammas13/deescalate) | Conflict dynamics: track tension turn by turn and pick the calming reply | A neighbour dispute, the stated rules (*"Tension starts at 3.38. A is sensitive to dismissal, threat; reactivity 1.04 ..."*), the turns, and three candidate replies | `{"tension": [2.97, 4.22, 6.31, ...], "peak": 10.0, "direction": "escalated", "best_option": "c"}` | 45,000 / 2,500 / 5,000 |
| [Urdu Legal Reason](https://www.kaggle.com/datasets/muhammadhammas13/urdu-legal-reason) | Statutory reasoning in Urdu, Roman Urdu or English | *"Shahid ne Yasir ko Islamabad mein kaha ke woh sarkari rate par plot dila sakta hai aur 10 August 2023 ko Rs 60,000 le liye ..."* | `{"provision": "Section 420, Pakistan Penal Code 1860", "forum": "police station, then Sessions Court"}` | 30,000 / 2,000 / 4,000 |
| [Interrogation Logic](https://www.kaggle.com/datasets/muhammadhammas13/interrogation-logic) | Checking testimony against a record | A ten-question interview (*"A2. I was with the site manager. ... A4. I got there about 7:03 pm."*) and the established facts | `{"deception_probability": 0.18, "false_claims": ["A2"]}` - the probability is a stated scoring rule, not a calibrated estimate | 40,000 / 2,500 / 5,000 |
| [ProtocolCheck](https://www.kaggle.com/datasets/muhammadhammas13/protocolcheck) | Compliance auditing: an execution log against a written procedure | *"PROCEDURE: Change release to production / 1. Raise the change record - by the engineer / 2. Attach the test evidence ..."* and a timestamped log | `{"compliant": true, "violations": []}` - or each skipped step, wrong role, wrong order or missed timing window | 45,000 / 2,500 / 5,000 |

Every name links to the data on Kaggle (JSONL splits with a dataset card, baselines and checksums). Each row's
prompt states everything needed to derive its answer, and the answer is the model's target output.

**402,000 examples** across the eight: 345,000 train, 19,000 val, 38,000 test.

## How they were built

The label is computed, never written by a model. A separate resolver re-derives
every row from the prompt text alone and the row is discarded unless it matches,
so a row that cannot be reproduced from what the reader is given does not ship.

Each dataset's Kaggle page carries its own baselines - including the shortcut
baselines that a model could exploit - so the headroom is visible before anyone
trains on it. Applying the full stated rules scores 1.000 on every one of the
eight. The strongest shortcut on the test split, lower meaning more of the task
is left to do:

| dataset | strongest shortcut (test) | what the shortcut is |
|---|---|---|
| Interrogation Logic | 0.307 | always answer "no false claims" |
| FraudTrail | 0.316 | TF-IDF plus amount magnitudes |
| WitnessDrift | 0.384 | keyword counts, ridge per witness |
| TimelineForge | 0.643 | the resolver, forgetting that chained events inherit unplaceability |
| ProtocolCheck | 0.676 | every stated rule except the timing windows |
| DeEscalate | 0.687 | phrase weights, ignoring who is listening |
| Contradiction Ledger | 0.719 | an LR on how often each date or amount appears |
| Urdu Legal Reason | 1.000 | word overlap with train templates |

Urdu Legal Reason is the exception worth stating plainly: a word-overlap
baseline that knows no law is right every time, because each provision has one
fact template per language and train and test share templates. Treat it as
trilingual parallel legal text with a fixed label set, not as a test of legal
reasoning. Its rows also carry `needs_native_review: true` - the Urdu and
Roman Urdu are template-generated by a non-native writer.

No prompt appears in two splits in any of the eight. All rows are synthetic,
with no personal data. Licence: [CC BY 4.0](LICENSE).

## Elsewhere

[Reasoning datasets, one page](https://datasets-chi.vercel.app)
· [Computer vision and image generation](https://vision-portfolio-woad.vercel.app)
· [Notebooks](https://www.kaggle.com/muhammadhammas13/code)
· [Code](https://github.com/hammasbuilds)
