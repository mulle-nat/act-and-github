# act-and-github

Environment variable use in github workflows and act.

Run with `act -e <( echo '{"push": { "ref": "refs/tags/prerelease" } }' )` locally.
Possibly map with `-P ubuntu-latest=mulle-objc:act` to other runner image.

## Result with github

Note the tests that didn't run:

<img width="242" alt="grafik" src="https://user-images.githubusercontent.com/1381995/126327194-79c88e1d-d943-4d1b-86e4-45012ef05009.png">

## Result with act

```
[CI/build]   ✅  Success - Dump Environment
[CI/build] ⭐  Run Test1
[CI/build]   🐳  docker exec cmd=[bash --noprofile --norc -e -o pipefail /Volumes/Source/srcM/act-and-github/workflow/1] user=
| Test1
[CI/build]   ✅  Success - Test1
[CI/build] ⭐  Run Test2
[CI/build]   🐳  docker exec cmd=[bash --noprofile --norc -e -o pipefail /Volumes/Source/srcM/act-and-github/workflow/3] user=
| Test2
[CI/build]   ✅  Success - Test2
[CI/build] ⭐  Run Test3.1
[CI/build]   🐳  docker exec cmd=[bash --noprofile --norc -e -o pipefail /Volumes/Source/srcM/act-and-github/workflow/5] user=
| Test3.1
[CI/build]   ✅  Success - Test3.1
```
