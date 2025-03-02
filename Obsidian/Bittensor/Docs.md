# Validator Workflow

When `validator.py` runs, it initializes and operates a validator node in a Bittensor subnet, processing miner submissions, scoring them, and updating the blockchain.

## 1. Startup
- **Entry**: Runs via `if __name__ == "__main__":` with a config from `get_config()`.
- **Initialization**:
  - Creates `Validator` instance with Bittensor config (e.g., cache dir, Hugging Face repo ID).
  - Sets up `miner_managers` for active challenges (`smooth_transition_challenge` adjusts challenges based on date, e.g., Feb 14, 2025 cutoff).
  - Initializes `StorageManager` (local cache, Hugging Face Hub, centralized DB sync).
  - Commits Hugging Face repo ID to blockchain.
  - Loads state:
    - **Challenge Records**: From centralized DB (`_init_challenge_records_from_subnet`).
    - **Miner Submissions** (`self.miner_submit`):
      - Centralized scoring: From centralized DB (`_init_miner_submit_from_subnet`).
      - Local scoring: From local cache (`_init_miner_submit_from_cache`).

## 2. Main Loop
- **Runs**: Infinite `while True` loop, logging "Validator is running..." every quarter epoch.
- **Validation Cycle**: `forward()` is called periodically:
  - ### Update Challenges
    - Transitions from old to new challenges (`smooth_transition_challenge`).
  - ### Query Miners
    - Queries miners’ axons (`update_miner_commit`):
      - Fetches encrypted commits and keys.
      - Updates `self.miner_submit` with new commits (timestamped on receipt).
      - Decrypts commits if `is_commit_on_time` and key provided.
  - ### Reveal Commits
    - Extracts revealed commits (`get_revealed_commits`): Maps challenge names to (Docker IDs, UIDs).
  - ### Score Commits
    - **Centralized Scoring**:
      - Saves commits to storage.
      - Fetches scores from central server (`get_centralized_scoring_logs`) after `SCORING_HOUR`, loops until done.
      - Updates `MinerManager` scores and logs.
    - **Local Scoring**:
      - Runs challenge controllers locally after `SCORING_HOUR` if day unscored.
      - Generates scores, updates `MinerManager`.
    - Adds today to `scoring_dates` if scored.
  - ### Store Results
    - Saves commits and logs (`store_miner_commits`) to local cache, centralized DB, Hugging Face Hub.
    - Stores challenge records (`store_challenge_records_new`) for the day.

## 3. Weight Setting
- **Periodic**: Assumes `set_weights` is called (e.g., per epoch):
  - Aggregates scores from `MinerManager`:
    - Challenge scores (decayed points).
    - Registration scores (new miners favored).
    - Stake scores (square-rooted stake).
  - Weights by challenge incentive, normalizes, sets on-chain via `subtensor`.

## 4. Storage Details
- **In-Memory**: Updates `self.miner_submit` with commits and logs.
- **Persistent**:
  - Local cache: Disk-based, 14-day TTL.
  - Centralized DB: Stores all commits and records, validator-specific.
  - Hugging Face Hub: JSON files per challenge/date, synced via `StorageManager`.
- **Async**: Queues updates for efficiency.

## 5. Key Conditions
- **Scoring Timing**: Post-`SCORING_HOUR`, only if day unscored and commits exist.
- **Commit Revelation**: Decrypts if within reveal window and key available.
- **No Validators**: Commits unqueried, scoring missed, weights static, storage dormant.

## 6. Recovery
- **Restart**: Rebuilds from centralized DB (centralized mode) or cache (local mode), queries miners for latest commits.

---
