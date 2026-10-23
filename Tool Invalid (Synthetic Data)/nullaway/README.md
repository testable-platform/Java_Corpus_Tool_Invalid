# nullaway

Inverse-corpus counterpart to the sibling Java-Tools-Clean's **nullaway** folder. Engineered so the tool should find something genuinely wrong.

Domain: toll-booth axle counts and pricing (`TollBooth`).

**Not installed here**: NullAway is not a standalone program. It is an Error Prone compiler plugin, resolved from Maven Central, which this sandbox's egress proxy refuses (HTTP 403 on the CONNECT, measured this session). Nothing below was produced by running NullAway or Error Prone; see Notes.

## What a failing result looks like

NullAway (annotated package `tollbooth`) should report five defects against TollBooth. They are planted by construction -- read from the source, **not measured**:

1. `toll` dereferences the result of the `@Nullable` method `axlesFor` without a check.
2. `lastPlateLength` dereferences the `@Nullable` field `lastPlate`.
3. `receipt` returns `null` from a method that is not `@Nullable`.
4. `operator` is a non-null field that no constructor or initializer ever assigns, so NullAway reports the field as not initialised (`operatorBadge` then uses it).
5. `auditStamp` passes the literal `null` to `stamp`, whose parameter is not `@Nullable`.

The unit tests only walk the happy paths, so they pass: the defects are invisible to the tests, which is exactly what a null-safety checker is for.

## Command

```bash
javac -XDcompilePolicy=simple -processorpath <error_prone_core + nullaway jars> \
    "-Xplugin:ErrorProne -XepDisableAllChecks -Xep:NullAway:ERROR -XepOpt:NullAway:AnnotatedPackages=tollbooth" \
    -d build/main java8/src/main/java/tollbooth/*.java
```

Expected: five errors, non-zero exit code (not run -- see Notes)

## Layout

```text
nullaway/
  README.md
  java8/ ... java25/        one folder per JDK family (18), identical source
    src/main/java/tollbooth/TollBooth.java
    src/main/java/tollbooth/Nullable.java   local @Nullable annotation
    src/test/java/tollbooth/TollBoothTest.java
```

## Notes

`Nullable.java` is a local annotation with the simple name `Nullable`; NullAway matches nullness annotations by simple name, so the fixture needs no third-party annotation jar to compile.

What **was** run, in every family: the source compiles with plain `javac --release N` on a JDK 25 host and the JUnit 4 tests pass, for N = 8 to 25 (18 of 18 families compile and pass).

## Per-family results

| Family | Host JDK | Result |
|---|---|---|
| `java8` | JDK 25 host, `--release 8` | NOT INSTALLED |

Every family has the same blocker (the Error Prone plugin cannot be fetched), so the column is the same for all 18. Each family's source was compiled with `javac --release N` on a JDK 25 host and its tests pass.

This repo carries only the `java8` family.
