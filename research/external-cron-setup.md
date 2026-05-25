# External Cron Setup For Auto Learning

GitHub native `schedule` is best-effort and can delay or drop runs. For reliable 5-minute learning while the local PC is off, use an external scheduler to call GitHub `repository_dispatch`.

## Workflow

- Repo: `vominhthanh996-lang/story_ThanhMV`
- Workflow file: `.github/workflows/auto-webnovel-craft-learning.yml`
- Event type: `auto-webnovel-craft-learning`
- Job result: runs `scripts/craft_learning_batch.py`, commits only `research/**`, then pushes to `main`.

## Cron Expression

Run every 5 minutes:

```text
*/5 * * * *
```

## HTTP Request

Method:

```text
POST
```

URL:

```text
https://api.github.com/repos/vominhthanh996-lang/story_ThanhMV/dispatches
```

Headers:

```text
Accept: application/vnd.github+json
Authorization: Bearer YOUR_GITHUB_TOKEN
X-GitHub-Api-Version: 2022-11-28
Content-Type: application/json
```

Body:

```json
{
  "event_type": "auto-webnovel-craft-learning"
}
```

## Token

Create a GitHub fine-grained personal access token for `vominhthanh996-lang/story_ThanhMV`.

Required permission:

```text
Contents: Read and write
```

Keep the token inside the external scheduler secret/header config. Do not commit it to the repo.

## Quick Manual Test

After creating the token, send the request once manually from the scheduler. A run should appear in GitHub Actions with event:

```text
repository_dispatch
```

Expected commit message when research changes:

```text
Auto learn webnovel craft
```

## Rules Preserved

- Do not write demo.
- Do not write chapters.
- Do not edit story folders or untracked episode folders.
- Only update `research/**`.
- Do not copy source prose; learn craft patterns only.
