# Serve Catalogs using Configuration Files

For all but simplest deployments, a configuration file is needed to spell out
all the options.


## Where?

The path to the configuration file (or directory of multiple configuration files)
is always specified when the server is started, as in:

```
tiled serve config my_config_file.yml
```

or

```
tiled serve config my_config_directory/
```

There is no automatic search path or standard location for server-side
configuration.

## Simple examples

The simple deployment

```
tiled serve directory path/to/files
```

is equavalent to

```yaml
catalogs:
    - path: /
      catalog: tiled.catalogs.files:Catalog.from_directory
      args:
          directory: "path/to/files"
```

And the simple deployment

```
tiled serve pyobject tiled.examples.generated_minimal:catalog
```

is equivalent to

```yaml
catalogs:
    - path: /
      catalog: tiled.examples.generated_minimal:catalog
```

## Less simple examples

### Serve two different directories at different sub-paths

```yaml
catalogs:
    - path: /a
      catalog: tiled.catalogs.files:Catalog.from_directory
      args:
          directory: "path/to/files"
    - path: /b
      catalog: tiled.catalogs.files:Catalog.from_directory
      args:
          directory: "path/to/other/files"
```

### Serve two different catalog objects are different sub-paths

```yaml
catalogs:
    - path: /a
      catalog: tiled.examples.generated_minimal:catalog
    - path: /b
      catalog: tiled.examples.generated:demo
```

See {doc}`../explanations/security` and {doc}`../explanations/access-control` for examples addressing
authentication and authorization.

## Configuration merging rules

If there are multiple configuration files:

* At most one may contain an ``authentication:`` section.
* More than one may contain a ``catalog:`` section, but if the same ``path``
  occurs in more than one file, or if colliding paths like ``/a`` and ``/a/b``
  are specified, an exception will be raised.