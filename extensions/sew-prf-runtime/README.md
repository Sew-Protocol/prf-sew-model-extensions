# Sew PRF runtime composition

This code-free package resolves the complete Sew runtime for PRF's runner and
CLI aliases. It composes the focused Sew Core, model, yield, financial,
pro-rata, bounty, analysis, research, and reference-validation packages at
immutable revisions.

The package also supplies Sew-owned runtime defaults as
`META-INF/prf/defaults.edn`. PRF's generic defaults loader discovers these
resources from the classpath; PRF core does not contain Sew scenario defaults.

Package profile and component metadata is supplied in
`META-INF/prf/profile-bundle.edn`. To describe a package profile from the PRF
checkout, resolve this aggregate package at an immutable Sew revision and pass
the resulting classpath to Babashka:

```sh
BABASHKA_CLASSPATH="$(clojure -Spath -Sdeps '{:deps {org.protocol-robustness/prf-sew-runtime {:git/url "https://github.com/Sew-Protocol/prf-sew-model-extensions.git" :git/sha "f5d8a00c8503e361451de8bee3179cd92b8a466e" :deps/root "extensions/sew-prf-runtime"}}}')" \
  bb profile:describe sew-financial-lifecycle
```

The profile viewer links only namespaces in the selected component closure
from those classpath roots. Package test suites remain qualified by the
package's own test commands; the profile view checks the composed source and
loads its declared entry namespaces.

Profile views should depend on the individual package components they select.
Full PRF runner aliases can depend on this composition to avoid repeating the
package list.
