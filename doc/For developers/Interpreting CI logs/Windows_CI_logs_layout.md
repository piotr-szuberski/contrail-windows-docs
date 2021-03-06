Windows CI logs layout
======================

This document describes a log structure generated by `ci-contrail-windows-production` job.
One can access these logs by following a link `ci-contrail-windows-production` in the comment posted by `Contrail Windows CI` used, after a job has finished.

## Log structure

- `log.txt.gz` - full Jenkins job log
- `unittests-logs` - logs for vRouter unit tests ran by Windows CI
- `TestReports/CISelfcheck` - test report for Windows CI unit tests suite
    - for information about more detailed logs please refer to [CI Selfcheck](#ciselfcheck)
- `TestReports/WindowsCompute` - test report for Contrail Windows integration tests suite
    - for information about more detailed logs please refer to [WindowsCompute](#windowscompute)

## CISelfcheck

`CISelfcheck` is a set of unit tests for PowerShell scripts used in Windows CI.

`TestReports/CISelfcheck` directory holds the following:

- `TestReports/CISelfcheck/pretty_test_report` - contains an HTML test report
    - [example test report](http://logs.opencontrail.org/winci/ed700a876ad74506a21d1937a26f02dc/TestReports/CISelfcheck/pretty_test_report/)
    - tests marked __green__ have passed
    - tests marked __red__ have failed
    - tests marked __orange__ are marked as inconclusive which means they are currently disabled in CI
- `TestReports/CISelfcheck/raw_NUnit` - contains NUnit XML files generated by PowerShell test runner

## WindowsCompute

`WindowsCompute` is a set of integration tests for Contrail Windows.
This test suite is run on a 3-node cluster consisting of 1 Contrail Controller node (all-in-one deployment) and 2 Contrail Windows compute nodes.
A fresh cluster is provisioned before each test.
Contrail Controller is provisioned using contrail-ansible-deployer.
In the future Contrail Windows compute nodes will also be provisioned with contrail-ansible-deployer.

`TestReports/WindowsCompute` directory holds the following:

- `TestReports/WindowsCompute/pretty_test_report` - contains an HTML test report
    - [example test report](http://logs.opencontrail.org/winci/ed700a876ad74506a21d1937a26f02dc/TestReports/WindowsCompute/pretty_test_report/)
    - tests marked __green__ have passed
    - tests marked __red__ have failed
    - tests marked __orange__ are marked as inconclusive which means they are currently disabled in CI
- `TestReports/WindowsCompute/cnm_plugin_junit_test_logs` - contains JUnit XML files generated by Contrail Windows CNM Plugin test runner
- `TestReports/WindowsCompute/detailed_logs` - contains logs dumped from test environment for each test; log file can contain logs from:
    - Contrail Windows CNM Plugin running on compute nodes
    - Windows containers ran on compute nodes
- `TestReports/WindowsCompute/raw_NUnit` - contains NUnit XML files generated by PowerShell test runner
