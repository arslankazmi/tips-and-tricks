# How to Get Creation Date of a Public Github Repository

### Platform: Linux/Ubuntu

The Github API has an endpoint for fetching repository details, which contains this creation date as well.

So I wrote a small chain of linux commands to get the creation date:

```bash
curl -L  -H "Accept: application/vnd.github+json"  -H "X-GitHub-Api-Version: 2022-11-28" https://api.github.com/repos/<repo-owner>/<repo-name> | grep created_at
```
