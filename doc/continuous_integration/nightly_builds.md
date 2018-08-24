# Nightly builds in Windows CI description
This document describes how nightly builds are done in Contrail Windows CI.
# Jenkins jobs: ci-contrail-windows-nightly-*
* There are two jobs responsible for the builds in CI:
  * `ci-contrail-windows-nightly-release` - builds Windows compute nodes in `release`/`production` mode.
  * `ci-contrail-windows-nightly-debug`- builds in `debug` mode.
  * Both of them have parameter `UPLOAD_ARTIFACTS` set to 1.
  * The difference between them is that `ci-contrail-windows-nightly-release` has an additional parameter `BUILD_IN_RELEASE_MODE` defined and set to _1_.
* These jobs are proxy jobs. They execute `winci-server2016-devel` with additional parameters mentioned above.
# Specific nightly builds artifacts location
To find artifacts from a specific nightly build do the following:
  * Enter to console logs from the build.
  * There is a line containing build ID of `winci-server2016-devel`:
    * Initial part of the line looks like this:
    `WinContrail » winci-server2016-devel <BUILD_NUMBER> completed`.
    * Note the `BUILD_NUMBER`.
  * Use `BUILD_NUMBER` to find desired nightly artifacts:
    * Uploaded artifacts are located in `\\<WINCI_DRIVE>\SharedFiles\WindowsCI-UploadedArtifacts\WinContrail\<winci-server2016-devel\<BUILD_MODE>` for now
      * `<WINCI_DRIVE>` is an IP to Contrail Windows CI drive
      * `<BUILD_MODE>` is a folder named by either `production` or `debug` build mode
    * There are folders named by build numbers. Folder matching with `BUILD_NUMBER` contains desired artifacts.