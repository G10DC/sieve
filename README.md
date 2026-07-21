# sieve

> Idempotent, schema-validated data pipelines that fail loud and never duplicate on re-run.

`sieve` is a Agent skill that enforces a disciplined shape on data pipelines —
ingest → validate → transform → persist — with raw/staged/curated separation, provenance,
and fail-as-value error handling. The rule above all: a pipeline you can't safely re-run is a bug.

## Install

Copy into `~/.gemini/config/skills/sieve/`. Agent environment auto-loads any folder that contains a `SKILL.md`.

## Usage

Opt-in, per turn. Activates when you build or touch an ETL, scraping-to-storage, or batch
transform flow. See `SKILL.md` for the golden rules and when (not) to use it.

## License

MIT — see [LICENSE](./LICENSE).
