First I hoped I can create a JSON schema by using some tool
which analyses a sample document, and generates the schema.
There are a few such tools (both gui and cli) but at the end I
wasn't satisfied wuth their result.

Anyway here is what I found:

## Generate the schema from sample

There are a lot of [tools](https://json-schema.org/implementations.html#schema-generators) some of:
- web: https://www.jsonschema.net/
- web: http://app.quicktype.io
- cli: https://github.com/snowplow/schema-guru

### Get a k3os config sample in json

Lets start with a sample [config.yaml from the docs](https://github.com/rancher/k3os#sample-configyaml) and turn it to json.

The result is in git: [sample.json](sample.json), but here is the process:

```
curl -s https://raw.githubusercontent.com/rancher/k3os/master/README.md \
  | sed -n '/### Sample/,/```$/ p' \
  | sed '1,/```yaml/ d; $ d' \
    > sample.yaml
```

convert it to json
```
yaml2json < sample.yaml | jq . > sample.json
```
you can get [yaml2json](https://github.com/bronze1man/yaml2json) from [github relase page](https://github.com/bronze1man/yaml2json/releases)

## Online Schema generator

Just go to quictype.io, and paste your sample ...
I've already did it: here is a [shareble/editable version](https://app.quicktype.io?share=2nS67wXD2QjgpLW87xbL)

## CLI schema generator


```
./schema-guru-0.6.2 schema sample.json > k3os.json
```

These generated schemas dont have min/max validations, or enums ... 

## Validation

- https://www.jsonschemavalidator.net
- https://json-schema-everywhere.github.io/yaml
