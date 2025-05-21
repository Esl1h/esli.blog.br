---
title: "parse YAML  on bash script"
datePublished: Sun Jul 31 2022 21:39:21 GMT+0000 (Coordinated Universal Time)
cuid: cl69uhyft0c3qy6nvbty7a0k6
slug: parse-yaml-on-bash-script
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/NLSXFjl_nhc/upload/v1659623107769/gzBViMWx7.jpeg
tags: functions, bash, script, yaml

---

How to parse and use the YAML file in bash/shell script

Create this function:
```bash
function parse_yaml {
    local prefix=$2
    local s='[[:space:]]*' w='[a-zA-Z0-9_]*' fs=$(echo @|tr @ '\034')
    sed -ne "s|^\($s\):|\1|" \
        -e "s|^\($s\)\($w\)$s:$s[\"']\(.*\)[\"']$s\$|\1$fs\2$fs\3|p" \
        -e "s|^\($s\)\($w\)$s:$s\(.*\)$s\$|\1$fs\2$fs\3|p"  $1 |
    awk -F$fs '{
        indent = length($1)/2;
        vname[indent] = $2;
        for (i in vname) {if (i > indent) {delete vname[i]}}
        if (length($3) > 0) {
            vn=""; for (i=0; i<indent; i++) {vn=(vn)(vname[i])("_")}
            printf("%s%s%s=\"%s\"\n", "'$prefix'",vn, $2, $3);
        }
    }'
}
```

At the beginning of your script, call `eval` to concate arguments.

`eval $(parse_yaml config.yml "CONFIG_")`

This eval line will put CONFIG_ in front of each variable found in config.yml (it is the \$2 and $prefix in function parse_yaml) line 4 and line 15 from my [parse_example.sh](https://gist.github.com/Esl1h/ae6aa5262c19b4e3774d29868b76dd18) .

The config.yaml is on the same path that the .sh. Therefore, you can create or modify the function to another YAML file (or add a variable) and change the prefix.


Each value from yml will be separated by "_" on your script call.

Example:

```yaml
  keyA:
    elementB: valueB
  
```

And `echo "$CONFIG_keyA_elementB"` will return `valueB`


Repo parse_yaml-with-bash:

%[https://github.com/Esl1h/parse_yaml-with-bash]
