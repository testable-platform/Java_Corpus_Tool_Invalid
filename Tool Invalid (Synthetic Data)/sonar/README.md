# sonar

Inverse-corpus counterpart to the sibling Java-Tools-Clean's **sonar** folder. Engineered so the tool should find something genuinely wrong.

Domain: shipping-office ledger desk (`LedgerDesk`).

**Not installed here**: Sonar analysis needs a running SonarQube server plus the scanner, and the scanner distribution (binaries.sonarsource.com, Maven Central) is refused by this sandbox's egress proxy (HTTP 403 on the CONNECT, measured this session). Nothing below was produced by running Sonar; see Notes.

## What a failing result looks like

LedgerDesk plants eleven ordinary, undisguised code smells. The rule keys are the expected Sonar Java rules, listed from the source -- **planted by construction, not measured**:

| Planted in | Expected rule |
|---|---|
| `PASSWORD` constant | java:S2068 hard-coded credentials |
| `unusedCounter` | java:S1068 unused private field |
| `add` -> `System.out.println` | java:S106 standard output |
| `classify` (deeply nested ifs) | java:S3776 cognitive complexity |
| `describe` (literal `"pending"` four times) | java:S1192 duplicated string literal |
| `readFirst` (reader never closed) | java:S2095 resource leak |
| `readFirst` (empty catch) | java:S108 empty block |
| `recompute` (`temp = 42; temp = 43;`) | java:S1481 unused local, java:S1854 dead store |
| `convert` | java:S1135 TODO tag |
| `labelA` / `labelB` | java:S4144 identical method bodies |
| `sameAccount` (`a == b` on strings) | java:S4973 strings compared by reference |

The unit tests exercise only the plain paths, so they pass: the smells are invisible to the tests.

## Command

```bash
sonar-scanner -Dsonar.projectKey=sonar-invalid-ledgerdesk -Dsonar.sources=java8/src/main/java \
    -Dsonar.tests=java8/src/test/java -Dsonar.java.binaries=build/main -Dsonar.host.url=$SONAR_HOST_URL
```

Expected: the issues in the table above (not run -- see Notes)

## Layout

```text
sonar/
  README.md
  java8/ ... java25/        one folder per JDK family (18), identical source
    src/main/java/ledgerdesk/LedgerDesk.java
    src/test/java/ledgerdesk/LedgerDeskTest.java
```

## Notes

What **was** run, in every family: the source compiles with plain `javac --release N` on a JDK 25 host and the JUnit 4 tests pass, for N = 8 to 25 (18 of 18 families compile and pass). No suppression comments or annotations anywhere, so a scanner sees exactly what is there.

## Per-family results

| Family | Host JDK | Result |
|---|---|---|
| `java24` | JDK 25 host, `--release 24` | NOT INSTALLED |

Every family has the same blocker (no server, no scanner), so the column is the same for all 18. Each family's source was compiled with `javac --release N` on a JDK 25 host and its tests pass.

This repo carries only the `java24` family.
