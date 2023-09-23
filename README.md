# terraform-license-change

## Issues and PRs data

1) I used the following script to pull the data from the GitHub API (credit to [@ebinnion](https://eric.blog/2022/08/12/how-to-export-github-pull-requests/)):
```sh
#!/bin/bash

# This script requires jq to be installed and available in the path.

TOKEN=$1
ORG=$2
REPO=$3
OUTPUT_RAW=""

get_pull_requests() {
	curl -s --location --request GET "https://api.github.com/repos/$ORG/$REPO/issues?state=all&per_page=40&page=$1" \
		--header "Authorization: token $TOKEN" \
		--header "Accept: application/vnd.github+json"
}

get_raw_output() {
	printf '%s' "$1" | jq -r
}

i=1
while [ "$OUTPUT_RAW" != "[]" ] ; do
	OUTPUT=$( get_pull_requests "$i" )
	OUTPUT_RAW=$( get_raw_output "$OUTPUT" )

	i=$((i+1))

	printf '%s' "$OUTPUT" | jq -r '.[] | [ .created_at, .html_url, .user.login, .title ] | @csv'
done
```
2) Then I exported the data to  CSV and filtered it by type (issues or PRs)
3) I manually checked each user's GitHub/LinkedIn profile to mark HashiCorp employee and community users

## Strarganzers data

I used this cool [online tool](https://seladb.github.io/StarTrack-js/#/) that also provides CSV

