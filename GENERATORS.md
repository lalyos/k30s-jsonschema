Its really frustrating to edit yaml files without a schema. I burned myslef by trying to create k3os config files by hand...

## Generate the schema from sample

instead of creating the schema by hand, lets generate it.

For the impatient here is a [shareble/editable version](https://app.quicktype.io?share=2nS67wXD2QjgpLW87xbL)

## Get a sample k3os config sample

There are a lot of [tools](https://json-schema.org/implementations.html#schema-generators) some of:
- web: https://www.jsonschema.net/
- web: http://app.quicktype.io
- cli: https://github.com/snowplow/schema-guru

Lets start with a sample [config.yaml from the docs](https://github.com/rancher/k3os#sample-configyaml) and turn it to jsin

```
curl -s https://raw.githubusercontent.com/rancher/k3os/master/README.md \
  | sed -n '/### Sample/,/```$/ p' \
  | sed '1,/```yaml/ d; $ d' \
    > sample.yaml
```

## Convert it to json
```
yaml2json < sample.yaml | jq . > sample.json
```

you can get [yaml2json](https://github.com/bronze1man/yaml2json) from [github relase page](https://github.com/bronze1man/yaml2json/releases)

## Generate the schema from sample


```
./schema-guru-0.6.2 schema sample.json > k3os.json
```

These generated schemas dont have min/max validations, or enums ... see the next paragraph

## DSL based generation

You can define your schema in a more compact DSL, and generate the full jsonschema from it.
See https://wryun.github.io/yajsonschema/

See my effort to define [k3os DSL](k3os-yajson.yaml)
Create a jsonschema from it (with enums, and simple min/max validation)
```
yajsonschema -s k3os-yajson.yaml > k3os-yajson.json
```

## Validation

- https://www.jsonschemavalidator.net
- https://json-schema-everywhere.github.io/yaml
