# act-and-github

Environment variable use in github workflows and act.

Run with `act -e <( echo '{"push": { "ref": "refs/tags/prerelease" } }' )` locally.

## Result with github

Note the pattern which tests did run and which didn't run:

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

The pattern of tests that ran and that did not run, is the same.

## Define environment variable in container

Create a docker container that defines ENV2.

`act/Dockerfile`:

```
FROM catthehacker/ubuntu:act-20.04

ENV ENV2=true
```

```
docker build -t act-and-github:act ./act
``` 

Run act with runner image:

```
act -P ubuntu-latest=act-and-github:act -e <( echo '{"push": { "ref": "refs/tags/prerelease" } }' )
```

Check `env` output to see that `ENV2` is defined now:

```
[CI/build] 🧪  Matrix: map[os:ubuntu-latest]
[CI/build] 🚀  Start image=act-and-github:act
[CI/build]   🐳  docker run image=act-and-github:act platform= entrypoint=["/usr/bin/tail" "-f" "/dev/null"] cmd=[]
[CI/build]   🐳  docker exec cmd=[mkdir -m 0777 -p /var/run/act] user=root
[CI/build] ⭐  Run Dump Environment
[CI/build]   🐳  docker exec cmd=[bash --noprofile --norc -e -o pipefail /Volumes/Source/srcM/act-and-github/workflow/0] user=
| ACT=true
| AGENT_TOOLSDIRECTORY=/opt/hostedtoolcache
| CI=true
| DEBIAN_FRONTEND=noninteractive
| DEPLOYMENT_BASEPATH=/opt/runner
| ENV1=true
| ENV2=true
| GITHUB_ACTION=0
| GITHUB_ACTIONS=true
| GITHUB_ACTION_REF=
| GITHUB_ACTION_REPOSITORY=
| GITHUB_ACTOR=nektos/act
| GITHUB_API_URL=https://api.github.com
| GITHUB_BASE_REF=
| GITHUB_ENV=/var/run/act/workflow/envs.txt
| GITHUB_EVENT_NAME=push
| GITHUB_EVENT_PATH=/var/run/act/workflow/event.json
| GITHUB_GRAPHQL_URL=https://api.github.com/graphql
| GITHUB_HEAD_REF=
| GITHUB_JOB=build
| GITHUB_PATH=/var/run/act/workflow/paths.txt
| GITHUB_REF=refs/tags/prerelease
| GITHUB_REPOSITORY=mulle-nat/act-and-github
| GITHUB_REPOSITORY_OWNER=mulle-nat
| GITHUB_RETENTION_DAYS=0
| GITHUB_RUN_ID=1
| GITHUB_RUN_NUMBER=1
| GITHUB_SERVER_URL=https://github.com
| GITHUB_SHA=14add200eea6b04fad04e0af1553a644b45a688d
| GITHUB_TOKEN=
| GITHUB_WORKFLOW=CI
| GITHUB_WORKSPACE=/Volumes/Source/srcM/act-and-github
| HOME=/root
| HOSTNAME=docker-desktop
| IMAGE_OS=ubuntu20
| ImageOS=ubuntu20
| LSB_RELEASE=20.04
| PATH=/opt/hostedtoolcache/node/14.17.1/x64/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
| PWD=/Volumes/Source/srcM/act-and-github
| RUNNER_OS=Linux
| RUNNER_PERFLOG=/dev/null
| RUNNER_TEMP=/tmp
| RUNNER_TOOL_CACHE=/opt/hostedtoolcache
| RUNNER_TRACKING_ID=
| RUNNER_USER=root
| RUN_TOOL_CACHE=/opt/hostedtoolcache
| SHLVL=0
| TERM=xterm
| USER=root
| _=/usr/bin/env
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

Still same output though as if `ENV2` was undefined.

