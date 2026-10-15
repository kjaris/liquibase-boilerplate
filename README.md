# Liquibase Secure boilerplate

Standalone Liquibase project. It does not start an application, and it is not an application schema.

The files here are the YAML example shipped inside **Liquibase Secure 6.0.0** at `liquibase/examples/yaml`. That is the tree the installer includes and the tree `liquibase init project` starts from: a defaults file, an example changelog, policy-check packages, and a flow file. They were taken from `com.liquibase:liquibase-commercial:6.0.0`.

`init project` is Liquibase's recommended way to create this layout. Secure 6.0 also writes `checks-packages/` (six themed packages and two composites, `coding-standards.pkg` and `security.pkg`).

## What is here

| File | Role |
|---|---|
| `liquibase.properties` | Defaults file. Example H2 URL, user `dbuser`, password `letmein` (Liquibase's example H2 password). |
| `example-changelog.yaml` | Modeled changelog from the product example: `person`, `company`, then `addColumn`. |
| `blank-changelog.yaml` | Empty changelog template from the same example set. |
| `checks-packages/` | Default Secure policy-check packages. |
| `liquibase.checks-package.yaml` | Package index `init project` generates. |
| `liquibase.flowfile.yaml` | Flow that runs changelog checks, `update`, then database checks. |
| `pom.xml` | `com.liquibase:liquibase-maven-plugin:6.0.0`, matching Liquibase's Maven example, with H2 2.5.252 on the plugin classpath. |

## Run

Secure commands need a license key. Export it; do not commit it.

```sh
export LIQUIBASE_LICENSE_KEY=...
export JAVA_HOME=/opt/homebrew/opt/openjdk@25
```

The example URL is an H2 server (`jdbc:h2:tcp://localhost:9090/mem:dev`). Liquibase's CLI starts that server with `liquibase init start-h2`. Then, from this directory:

```sh
mvn -B liquibase:update
mvn -B liquibase:status
```

With the Secure CLI installed, the same directory is the working directory:

```sh
liquibase update
liquibase checks run --checks-settings-file=liquibase.checks-package.yaml --checks-packages=scripting-standards.pkg
liquibase flow
```

`liquibase.oracle.properties.example` is a starting point for a scratch Oracle schema. Point it at a schema you created for this project.
