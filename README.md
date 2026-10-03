# datasets

Reasoning datasets built from code and labelled by code. Every row is
re-derived by an independent verifier that does not import the generator, and
kept only if the verifier reproduces the label exactly.

Each name links to the dataset on Kaggle.

| dataset | task | train / val / test |
|---|---|---|
| [Contradiction Ledger](https://www.kaggle.com/datasets/muhammadhammas13/contradiction-ledger) | Find the pair of statements that cannot both be true, in a long record | 40,000 / 2,000 / 4,000 |
| [WitnessDrift](https://www.kaggle.com/datasets/muhammadhammas13/witnessdrift) | Track what each account changes between retellings | 50,000 / 2,500 / 5,000 |
| [FraudTrail](https://www.kaggle.com/datasets/muhammadhammas13/fraudtrail) | Decide whether a transaction trail supports the alert raised against it | 35,000 / 2,000 / 4,000 |
| [TimelineForge](https://www.kaggle.com/datasets/muhammadhammas13/timelineforge) | Rebuild an absolute timeline from a story told out of order | 60,000 / 3,000 / 6,000 |
| [DeEscalate](https://www.kaggle.com/datasets/muhammadhammas13/deescalate) | Choose the reply that lowers the temperature without conceding the point | 45,000 / 2,500 / 5,000 |
| [Urdu Legal Reason](https://www.kaggle.com/datasets/muhammadhammas13/urdu-legal-reason) | Apply Pakistani statute text to a fact pattern, in Urdu | 30,000 / 2,000 / 4,000 |
| [Interrogation Logic](https://www.kaggle.com/datasets/muhammadhammas13/interrogation-logic) | Work out who is lying from a set of mutually constraining claims | 40,000 / 2,500 / 5,000 |
| [ProtocolCheck](https://www.kaggle.com/datasets/muhammadhammas13/protocolcheck) | Check whether a procedure was followed, step by step, against its written protocol | 45,000 / 2,500 / 5,000 |

**402,000 examples** across the eight.

## How they were built

The label is computed, never written by a model. A separate resolver re-derives
every row from the prompt text alone and the row is discarded unless it matches,
so a row that cannot be reproduced from what the reader is given does not ship.

Each dataset's Kaggle page carries its own baselines - including the shortcut
baselines that a model could exploit - so the headroom is visible before anyone
trains on it.

## Not here

Four further datasets stay private: Daleel, ScholarCritic, The Trial and
PsyReason. Three support unpublished academic work and one is a product.

## Elsewhere

[Computer vision and image generation](https://vision-portfolio-woad.vercel.app)
· [Notebooks](https://www.kaggle.com/muhammadhammas13/code)
· [Code](https://github.com/hammasbuilds)
