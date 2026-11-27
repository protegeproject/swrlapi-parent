# SWRLAPI parent POM

This parent manages dependency versions and Maven plugins for the SWRLAPI sibling
repositories. The `protege-6` branch is **2.1.5-SNAPSHOT**, with **OWLAPI 5.5.1** and
**Drools 7.74.1.Final**, Java 11 bytecode and a JDK 17 / Maven 3.9 validation path.
Protégé plugin dependencies have separate host compatibility requirements.

## Build

Clone the sibling repositories on `protege-6` using the build guide below. From
the workspace root, run:

```sh
mvn -B -f swrlapi-project/pom.xml -Pcore,static-analysis clean verify
```

The reactor builds this parent before its consumers and uses their current source.
No Protégé installation or separate parent `install` is needed for the core gate.
Change shared dependency versions here, then verify the affected sibling consumers;
do not override child versions independently. The runbook records the resolved
security baseline and host-specific compatibility limits.

Release publishing follows [RELEASING.md](RELEASING.md) and the build project's
[release procedure](https://github.com/protegeproject/swrlapi-project/blob/protege-6/release/README.md).
A local snapshot build does not publish artifacts.

[Build guide](https://github.com/protegeproject/swrlapi-project/blob/protege-6/README.md) · [Runbook](https://github.com/protegeproject/swrlapi-project/blob/protege-6/ops/runbook.md) · [Roadmap](https://github.com/protegeproject/swrlapi-project/blob/protege-6/ops/roadmap.md)

## License

[BSD 2-Clause](license.txt), copyright Stanford University.
