# action-gocredits
Github Actions: Generate Credits for Golang with Songmu/gocredits


# Usage


```yaml
# .github/workflows/tagpr.yml
name: tagpr
on:
  push:
    branches: ["main","master"]

jobs:
  tagpr:
    runs-on: ubuntu-latest
    env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    steps:
      - uses: actions/checkout@v4
      - id: tagpr
        uses: Songmu/tagpr@v1
      - name: Set up Go
        uses: actions/setup-go@v5
        with:
          go-version: "1.21"
      - uses: mashiike/action-gocredits@v0
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
        if: "steps.tagpr.outputs.tag == ''"
      - name: Run Go Releaser
        uses: goreleaser/goreleaser-action@v5
        with:
          version: latest
          args: release
        if: ${{ steps.tagpr.outputs.tag != '' }}
```
