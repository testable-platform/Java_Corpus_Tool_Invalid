# PMD

Inverse-corpus counterpart to the sibling Java-Tools-Clean's **PMD**
folder. Stays **NOT_INSTALLED** here too -- the blocker is
infrastructure (GitHub Releases blocked, no Debian package), not
anything about this corpus being Clean or Invalid, so there is nothing
to invert.

Package: PMD 7.x (GitHub Release zip)

Domain: conveyor-belt speed auditing (ConveyorAudit)

**Not installed here**: see Notes for why, and what was checked instead.

## What a result would look like

A real PMD rule-set run against ConveyorAudit would report any
matching rule violations. Not measurable here: see Notes.

## Command

```bash
pmd check -d src/main/java -R rulesets/java/quickstart.xml
```

## Notes

PMD's own distribution is a GitHub Release zip; there is no Debian
package for it, and the release channel returns 403 at this sandbox's
egress proxy. Identical to the sibling Clean corpus's own finding
(distinct from this corpus's CPD folder, which stands PMD's own
copy-paste detector, CPD, in for separately with jscpd -- PMD's full
rule-based checker is its own, equally unreachable, tool).

## Per-family results

| Family | Host JDK | Result |
|---|---|---|
| `java8` | JDK 8 (native) | NOT INSTALLED |

This repo carries only the `java8` family.
