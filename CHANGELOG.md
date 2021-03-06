# Changes

* 2.0.next in progress
  * Fix #66 by switching from "jar" processing to "zip" processing and using `.entries` instead of calling `.getNextEntry` (the latter checks CRCs, the former doesn't apparently).

* 2.0.171 -- 2021-01-29
  * Fix #56 by requiring all of `:group-id`, `:artifact-id`, and `:version` when `:sync-pom true` and no `pom.xml` file is present (GAV are now required when you want `depstar` to create your `pom.xml` file).
  * Fix #59 by decoupling `pom.xml` file handling from `MANIFEST.MF` handling, which makes it possible to build an uberjar without a `pom.xml` file. This also allows you to specify `:aot true` and `:main-class` when building a (thin) JAR file -- but cautions you that it is not recommended!
  * Fix #60 by attempting to run `git rev-parse HEAD` (in the same directory as the `pom.xml`) and adding the output as `revision=` in `pom.properties`.
  * Address #61 by updating the **Classpath** section of the README (to clarify how the classpath is built and how to use `-Sdeps`).

* 2.0.165 -- 2020-12-28
  * Escape compile process arguments when shelling out on Windows. Fixes #57 via PR #58 (@borkdude).

* 2.0.161 -- 2020-12-23
  * Allow more formats of `<tag>` to be matched against the version, when synchronizing `:version`. Fixes #55.

* 2.0.160 -- 2020-12-22
  * `depstar` now behaves like a "tool" rather than a "library" -- you should use `:replace-deps` to specify it as a dependency rather than `:extra-deps` and it will compute the project basis from the system, user, and project `deps.edn` files using `clojure.tools.deps.alpha`. By default, it applies no aliases but you can specify an `:aliases` exec-arg with a vector of aliases to apply. By default, it behaves like the CLI's `-Srepro` option in that the user `deps.edn` file is ignored: specify `:repro false` if you want the user `deps.edn` file to be included in the basis. Fixes #47, #48, #49.
  * `:compile-ns` exec-arg supports a vector of namespaces to be compiled; this overrides `:aot` and `:main-class` and allows you to AOT-compile specific namespaces for inclusion in a thin JAR, if needed. Fixes #51.
  * The group/artifact IDs and the version can now be overridden by exec arguments (`:group-id`, `:artifact-id`, and `:version` respectively, and `depstar` will update your `pom.xml` file to match). Fixes #53.
  * `:sync-pom true` will automatically run the equivalent of `clojure -Spom`. See README for more details. Fixes #54.
  * The log4j2 plugins cache is now merged correctly. Fixes #50.
  * Supported entry points: `hf.depstar/jar` and `hf.depstar/uberjar` via `-X`, `hf.depstar.uberjar/build-jar` via REPL or library usage. The following legacy entry points are all deprecated: `hf.depstar.jar/-main`, `hf.depstar.jar/run`, `hf.depstar.uberjar/-main`, `hf.depstar.uberjar/run`, and `hf.depstar.uberjar/run*`.
  * Automated tests now exist, along with CI via GitHub Actions, against JDK versions 8, 11, 14, 15, 16-ea, and 17-ea. Fixes #26.

* 1.1.136 -- 2020-11-16
  * Fix #46 by adding `:pom-file` exec argument to specify a `pom.xml` file in a non-standard location, e.g., `:pom-file '"/tmp/pom.xml"'` -- there is no equivalent `:main-opts` flag for this, you have to use the CLI's `-X` invocation to supply it.

* 1.1.133 -- 2020-11-07
  * Fix #45 by providing a REPL-friendly/library entry point as `hf.depstar.uberjar/build-jar`. _[this was originally `hf.depstar.uberjar/run*`]_

* 1.1.132 -- 2020-10-19
  * Call `shutdown-agents` at the end of processing, to account for AOT of badly-behaved code that has side-effecting top-level forms.
  * Clarify, in the README, that Clojure CLI version 1.10.1.697 or later is assumed.

* 1.1.128 -- 2020-10-09
  * Fix #44 by changing how legal files are initially copied.

* 1.1.126 -- 2020-10-08
  * Fix #43 by changing `pom.properties` to use `=` instead of `:`.
  * Fix #42 by changing how `compile` failures are handled.
  * Fix #41 by including license and copyright notices; concatenates repeated files if they are not exact duplicates.

* 1.1.117 -- 2020-09-14
  * Fix #40 by making "suppress clash" the default; add `-D` / `--debug-clash` option to print the "clashing jar items" warnings.
  * Address #39 by issuing a warning if conflicting data readers are detected (`.cljc` data readers are not fully supported yet).
  * Address #38 by warning about multiple JAR file names (explaining which one is selected).
  * Streamline the `clojure -X` invocation entry points to `hf.depstar/jar` and `hf.depstar/uberjar`.

* 1.1.104 -- 2020-08-27
  * Fix #37 by adding `-X` / `--exclude` to provide one or more regex used to exclude files from the JAR.
  * Fix #35 by providing `hf.depstar/jar` and `hf.depstar/uberjar` as entry points that can be used by the Clojure CLI `-X` option (to execute a specific function and pass a map of arguments). _[these were originally `hf.depstar/jar` and `hf.depstar/uberjar`]_
  * Fix #34 by adding `-P` / `--classpath` option to specify a classpath to use (based on PR #36 @borkdude).
  * Fix #33 by allowing destination JAR filename to appear anywhere on the command-line as well as adding a `-J` / `--jar` option for it.
  * Address #31 by clarifying in the README that `(:gen-class)` is required for AOT.

* 1.0.97 -- 2020-08-05
  * Fix a bug in the workaround for `Log4j2Plugins.dat`.

* 1.0.96 -- 2020-07-29
  * Added a workaround for https://issues.apache.org/jira/browse/LOG4J2-954 issue with `Log4j2Plugins.dat` file conflicts.

* 1.0.94 -- 2020-04-10
  * Fix #29 by supporting data reader files with `.cljs` and `.cljc` extensions as well as `.clj`.
  * As of 1.1.117, this is the default and there is a new `-D` / `--debug-clash` option to display these warnings: _Address #28 by adding `-S` / `--suppress-clash` option to suppress the warning about clashing jar items._
  * Move to MAJOR.MINOR.COMMITS versioning scheme.

* 0.5.2 -- 2020-01-16
  * Fix NPE for uberjar when no main class specified #25 @noisesmith.
* 0.5.1 -- 2020-01-02
  * Address #24 by transforming the main class (namespace) name.
* 0.5.0 -- 2020-01-02
  * Address #23 by managing the AOT compilation folder automatically.
  * Users no longer need to create `classes` or add it to the classpath.
* 0.4.2 -- 2019-12-31
  * Address #22 by automatically setting `Multi-Release: true` in the uberjar manifest if any multi-release JAR files are consumed.
* 0.4.1 -- 2019-12-31
  * Address #21 by ignoring `.keep` files.
* 0.4.0 -- 2019-12-31
  * Address #20 by adding `-C` / `--compile` option to AOT-compile the main namespace for an uberjar.
* 0.3.4 -- 2019-10-18
  * Fix #19 by following symlinks when copying directories.
* 0.3.3 -- 2019-09-06
  * Fix #18 by using regex instead of `clojure.xml` to extract group ID, artifact ID, and version.
* 0.3.2 -- 2019-08-26
  * Fix #16 by adding `:unknown` copy. handler and checking for excluded filenames in it
  * An unknown file type is now ignored, with a warning printed if it is not an excluded filename.
* 0.3.1 -- 2019-08-05
  * Address #14 by adding `-m` / `--main` option to override `Main-Class` in the manifest.
* 0.3.0 -- 2019-07-24
  * Fix #13 by using the local `pom.xml`, if present, to generate a manifest (and copy `pom.xml` into the JAR file).
* 0.2.4 -- 2019-07-05
  * **Important bug fix for tree-walking bug introduced in 0.2.1!**
* 0.2.3 -- 2019-07-01 *(do not use)*
  * Back off Clojure version to 1.7.0 so `depstar` can be used to build JARs for older projects.
* 0.2.2 -- 2019-06-29 *(do not use)*
  * Fix #11 by adding a `-v`/`--verbose` option to display files added to the archive.
  * Fix #9 properly by creating parent directories prior to move of JAR file.
* 0.2.1 -- 2019-05-08 *(do not use)*
  * Fix #9 by creating parent directories for target JAR file (PR #10 @jarohen).
* 0.2.0 -- 2019-05-07
  * Fix #8 by switching to ZipFileSystem and performing a single copy pass (instead of copying to temporary folder tree and then building a zip file).
* 0.1.7 -- 2019-04-24
  * Fix #6 by excluding `*.pom` files.
  * Fix #7 by excluding `module-info.class` files.
  * Lists excluded files if debugging enabled.
* 0.1.6 -- 2019-03-10
  * Fix for JARs containing `data_readers.clj` (do not close input stream!).
  * Supports `-Ddepstar.debug=true` and `DEPSTAR_DEBUG=true` to be more verbose.
* 0.1.5 -- 2018-10-24
  * Fix for timestamp preservation.
* 0.1.2 -- 2018-10-23
  * Initial fork with (incorrect) fix for exception from JARs containing `data_readers.clj`.
  * `hf.depstar.jar` namespace added.
