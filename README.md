# subdomain-monitoring
Dynamic subdomain monitoring(DSM) with Subfinder and Httpx




[Subdomain Action](https://github.com/Loneyers/subdomain-action) is a [GitHub Action](https://github.com/features/actions) to create subdomain scan workflows with [Subfinder](https://github.com/projectdiscovery/subfinder) and [Httpx](https://github.com/projectdiscovery/httpx).

Usage
-----

*.github/workflows/subdomain.yml*
```
on:
  workflow_dispatch:
  schedule:
    - cron: "0 0 * * *"

jobs:
  worker:
    runs-on: ubuntu-latest
    steps:      
      - uses: actions/checkout@v2      

      - uses: Loneyers/subdomain-action@main
        with:
          domain: "domain.txt"
          output: "output.txt"
          
      - name: upload
        run: |
          git config --local user.email "loneyershi@gmail.com"
          git config --local user.name "Loneyers"
          git remote set-url origin https://${{ github.actor }}:${{ secrets.GITHUB_TOKEN }}@github.com/${{ github.repository }}
          git pull --rebase
          git add output.txt
          git commit -m "upload subdomain"
          git push

```
Inputs
------

| Key  | Description | Required |
| :---:     |     :---:   |    :---:   |
| `domain`  | File containing list of domains to enumerate | true
| `output`  | File to save output result | true
