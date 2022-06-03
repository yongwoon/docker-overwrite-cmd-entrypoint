# docker-overwrite-cmd-entrypoint

```bash
❯ docker-compose up

# -------------------------------

[+] Running 4/4
 ⠿ Container both-override        Recreated                                                                                                                                 0.1s
 ⠿ Container command-override     Recreated                                                                                                                                 0.1s
 ⠿ Container entrypoint-override  Recreated                                                                                                                                 0.1s
 ⠿ Container non-override         Recreated                                                                                                                                 0.1s

Attaching to both-override, command-override, entrypoint-override,

non-override
non-override         | param1 param2 param3

entrypoint-override  | overwride entrypoint. parameters =>

both-override        | overwride entrypoint. parameters =>  foo bar baz

command-override     | foo bar baz

non-override exited with code 0
entrypoint-override exited with code 0
both-override exited with code 0
command-override exited with code 0
```

```bash
docker inspect non-override | jq '.[].Config | .Entrypoint,.Cmd'

[
  "echo"
]
[
  "param1",
  "param2",
  "param3"
]
```

```bash
# ENTRYPOINTを上書きするとDockerイメージのCMDがNULLになる
docker inspect entrypoint-override | jq '.[].Config | .Entrypoint,.Cmd'

[
  "echo",
  "overwride entrypoint. parameters =>"
]
null
```

```bash
docker inspect command-override | jq '.[].Config | .Entrypoint,.Cmd'

[
  "echo"
]
[
  "foo",
  "bar",
  "baz"
]
```

```bash
docker inspect both-override | jq '.[].Config | .Entrypoint,.Cmd'

[
  "echo",
  "overwride entrypoint. parameters => "
]
[
  "foo",
  "bar",
  "baz"
]
```
