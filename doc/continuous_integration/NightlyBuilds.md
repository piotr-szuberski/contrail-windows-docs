# Nightly builds in Windows CI description
This document describes how nightly builds are done in Contrail Windows CI.
# Jenkins jobs: ci-contrail-windows-nightly-*
* There are two jobs responsible for nightly building in CI:
  * `ci-contrail-windows-nightly-release` - builds Windows compute nodes in `release`/`production` mode.
  * `ci-contrail-windows-nightly-debug`- builds in `debug` mode.
  * Both of them have parameter `UPLOAD_ARTIFACTS` set to 1.
  * The difference between them is that `ci-contrail-windows-nightly-release` has an additional parameter `BUILD_IN_RELEASE_MODE` defined and set to _True_.
* These jobs are proxy jobs. They execute `winci-server2016-devel` with additional parameters mentioned above.
* `contrail-windows-ci` looks for the parameters which are defined as an environment variables and verifies if it is a nightly build or not.
# Specific nightly builds artifacts location
To find artifacts from a specific nightly build do the following:
  * Enter to console logs from the build.
  * There is a line containing build ID of `winci-server2016-devel`:
    * Initial part of the line looks like this:
    `WinContrail » winci-server2016-devel <BUILD_ID> completed`.
    * Note the `BUILD_ID`.
  * Use `BUILD_ID` to find desired nightly artifacts:
    * Uploaded artifacts are located in `\\10.84.12.40\SharedFiles\WindowsCI-UploadedArtifacts\WinContrail\winci-server2016-devel` for now.
    * There are folders named by build ids. Folder matching with `BUILD_ID` contains desired artifacts.