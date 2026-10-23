# Testable Java corpus — JV_V9_MAVEN_SHADEDUBERJARRELOCATED_MONO

Grid cell `MVN-SHADE-M` of the 24-cell Java grid.

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
| Java version | 9 |
| Host JDK | 11 |
| Build system | Maven |
| Packaging | Shaded uber-jar (relocated) |
| Architecture | Monolith |

## Build

```
mvn -B clean package
```

Main and test sources both compile at Java 9 (bytecode major version 53). The code
uses private interface methods, `Set.of`, `Optional.or`, `Optional.stream`,
`Stream.takeWhile`, try-with-resources on an effectively-final variable and the diamond
operator on an anonymous class, so it fails to compile under `--release 8` — the version differentiation is real, not
declared.

Produces: `jv-123-all.jar (shaded, packages relocated)`

## Run

```
java -jar <artifact> O-1234
```

## Test

```
mvn -B test
```

## Workspace projects

- `src/main/java/` (single module)


## `Tool Invalid (Synthetic Data)/`
Same shape as Clean -- carrying only this branch's own `java9` family -- but engineered so
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

