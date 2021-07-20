# act-and-github

Environment variable use in github workflows and act.

Run with `act -e <( echo '{"push": { "ref": "refs/tags/prerelease" } }' )` locally.
Possibly map with `-P ubuntu-latest=mulle-objc:act` to other runner image.

