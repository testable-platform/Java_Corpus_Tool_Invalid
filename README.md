# Testable Java corpus — JV_V8_ANTIVY_SHADEDUBERJARRELOCATED_MONO

Grid cell `ANT-SHADE-M` of the 24-cell Java grid.

## Project type

Order pricing and risk domain. The domain layer is byte-identical across all 24 branches
in this family, so any difference in tool output is attributable to the branch variables
below and not to the code the tool was pointed at.

## Branches

This repository holds 432 orphan branches, one per grid cell across every Java version, build system and packaging combination in this corpus. `main` carries the title
only. See `dataset.json` for the machine-readable description of this branch.

## Branch variables

| Variable | Value |
|---|---|
| Java version | 8 |
| Host JDK | 8 |
| Build system | Ant + Ivy |
| Packaging | Shaded uber-jar (relocated) |
| Architecture | Monolith |

## Build

```
ant clean package
```

Main and test sources both compile at Java 8 (bytecode major version 52). The code
uses lambdas, method references, streams, `Optional`, `java.time` and default interface
methods, so it fails to compile under `-source 7` — the version differentiation is real,
not declared.

Produces: `jv-021-all.jar (shaded, packages relocated)`

## Run

```
java -jar <artifact> O-1234
```

## Test

```
ant test
```

## Workspace projects

- `src/main/java/` (single module)


## `Tool Invalid (Synthetic Data)/`
Same shape as Clean -- carrying only this branch's own `java8` family -- but engineered so
every fixture makes the tool flag or fail rather than pass: the
**Tool Invalid** condition. `diff-cover`, `git-churn` and `pydriller` each carry one
real git repository's worth of history in both Clean and Invalid rather than 18
per-version copies, restored from `_git-bundles/` via `restore-git.ps1`
rather than kept as a live `.git` folder, so a plain file copy never
silently drops their content.

## Planted CVE pins

| Dependency | Version |
|---|---|
| `commons-collections:commons-collections` | 3.2.1 |
| `org.apache.commons:commons-text` | 1.9 |
| `com.fasterxml.jackson.core:jackson-databind` | 2.9.10.1 |
| `log4j:log4j` | 1.2.17 |
| `org.yaml:snakeyaml` | 1.30 |

Deliberately vulnerable versions, so the SCA metrics have known true positives. Do not
upgrade them.

