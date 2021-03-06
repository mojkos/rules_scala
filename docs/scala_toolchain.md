# scala_toolchain

`scala_toolchain` allows you to define global configuration to all Scala targets.

**Some scala_toolchain must be registered!**

### Several options to configure `scala_toolchain`:

#### A) Use the default `scala_toolchain`:

In your workspace file add the following lines:

```python
# WORKSPACE
# register default scala toolchain
load("@io_bazel_rules_scala//scala:toolchains.bzl", "scala_register_toolchains")
scala_register_toolchains()
```

#### B) Defining your own `scala_toolchain` requires 2 steps:

1. Add your own definition of `scala_toolchain` to a `BUILD` file:
    ```python
    # //toolchains/BUILD
    load("@io_bazel_rules_scala//scala:scala_toolchain.bzl", "scala_toolchain")

    scala_toolchain(
        name = "my_toolchain_impl",
        scalacopts = ["-Ywarn-unused"],
        unused_dependency_checker_mode = "off",
        visibility = ["//visibility:public"]
    )

    toolchain(
        name = "my_scala_toolchain",
        toolchain_type = "@io_bazel_rules_scala//scala:toolchain_type",
        toolchain = "my_toolchain_impl",
        visibility = ["//visibility:public"]
    )
    ```

2. Register your custom toolchain from `WORKSPACE`:
    ```python
    # WORKSPACE
    register_toolchains("//toolchains:my_scala_toolchain")
    ```

<table class="table table-condensed table-bordered table-params">
  <colgroup>
    <col class="col-param" />
    <col class="param-description" />
  </colgroup>
  <thead>
    <tr>
      <th colspan="2">Attributes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>scalacopts</code></td>
      <td>
        <p><code>List of strings; optional</code></p>
        <p>
          Extra compiler options for this binary to be passed to scalac. 
        </p>
      </td>
    </tr>
    <tr>
      <td><code>scalac_jvm_flags</code></td>
      <td>
        <p><code>List of strings; optional</code></p>
        <p>
          List of JVM flags to be passed to scalac. For example <code>["-Xmx5G"]</code> could be passed to control memory usage of Scalac.
        </p>
        <p>
          This is overridden by the <code>scalac_jvm_flags</code> attribute on individual targets.
        </p>
      </td>
    </tr>
    <tr>
      <td><code>scala_test_jvm_flags</code></td>
      <td>
        <p><code>List of strings; optional</code></p>
        <p>
          List of JVM flags to be passed to the ScalaTest runner. For example <code>["-Xmx5G"]</code> could be passed to control memory usage of the ScalaTest runner.
        </p>
        <p>
          This is overridden by the <code>jvm_flags</code> attribute on individual targets.
        </p>
      </td>
    </tr>
    <tr>
      <td><code>unused_dependency_checker_mode</code></td>
      <td>
        <p><code>String; optional</code></p>
        <p>
          Enable unused dependency checking (see <a href="https://github.com/bazelbuild/rules_scala#experimental-unused-dependency-checking">Unused dependency checking</a>).
          Possible values are: <code>off</code>, <code>warn</code> and <code>error</code>.
        </p>
      </td>
    </tr>
    <tr>
      <td><code>enable_code_coverage_aspect</code></td>
      <td>
        <p><code>"on" or "off"; optional; defaults to "off"</code></p>
        <p>
            This enables instrumenting tests with jacoco code coverage.
        </p>
      </td>
    </tr>
  </tbody>
</table>