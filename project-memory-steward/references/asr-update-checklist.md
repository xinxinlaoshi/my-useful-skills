# ASR project update checklist

Use this checklist when the project involves ASR, speech data, VAD, transcription, or multimodal audio.

## State to capture

- Target languages and target hours.
- Raw audio source and current retained hours.
- VAD/chunking method and output path.
- Teacher model and checker models.
- Active runs by machine, rank, shard, worker, and device.
- Input manifests and output summaries.
- Completed shards and failed shards.
- Empty outputs and duplicate processing.
- Observed throughput, RTF, and bottleneck hypothesis.
- Quality metrics and filtering thresholds.
- Storage cleanup boundaries.

## Evidence to prefer

1. Summary JSON files.
2. Manifest line counts.
3. Run logs.
4. Output JSONL line counts.
5. Code/config defaults.
6. Human memory from chat.

Do not let chat claims override artifacts unless the user explicitly says the artifacts are stale.
