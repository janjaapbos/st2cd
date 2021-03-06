## Patch release

Following documentation is written with specific examples. Patch version we are trying to release is 0.12.2. Patch version branch is v0.12.2 and minor version branch is v0.12.

* Ensure build server has release rules for the minor version branch. See [rule for ubuntu](https://gist.github.com/lakshmi-kannan/87c14f8a3f1f2f6c14a9) and [rule for fedora](https://gist.github.com/lakshmi-kannan/bc42c649e98ad4f4044f).

* Verify that you have a patch branch for patch version you want to release in st2 repo. E.g.: https://github.com/StackStorm/st2/tree/v0.12.2

* Verify that the changelog in st2 patch branch has info about 0.12.2 release. Set the date for the release. See [PR](https://github.com/StackStorm/st2/pull/1809) for an example. ```(XXX: Automate)```

* Set st2_version in st2web package.json for the respective parent branch. (In this case v0.12). See [PR](https://github.com/StackStorm/st2web/pull/178) for an example.

* Merge st2web PR first. Never merge this without a ``+1``.

* Merge st2 PR to fix changelog date and docs now. Never merge this without a ``+1``.

* Open a PR to merge patch branch (In this case, v0.12.2) to parent branch (In this case, v0.12). See [PR](https://github.com/StackStorm/st2/pull/1810) for example. (DO NOT AUTOMATE THIS)

* Stop and take a deep breathe. Meditate for 10 mins.

* Merge st2 PR (patch version to parent merge) to kickoff patch release automation. DO NOT DELETE PATCH BRANCH. AUTOMATION WILL DO IT.

* Build flow artifact and upload it to the downloads server:

```bash
st2 run st2cd.flow_pkg version=1.1.1 build=manual branch=master dl_server=dl-origin001
```

Note: ``version`` indicates the version of flow and will be put in the final
tarball name - ``flow-<version>.tar.gz`` (e.g. ``flow-1.1.1.tar.gz``).

Once the packages are built and uploaded, they should be available at
https://<enterprise token>:@downloads.stackstorm.net/st2enterprise/apt/trusty/st2flow/flow-1.1.1.tar.gz

* Wait for slack notifications.

* Edit github releases to include info about the new release. For example, https://github.com/StackStorm/st2/releases/tag/v0.12.2

* Inform slack community about the new release.

## Running tests for the workroom and the installer

To test an installer with a specific version, you can use the following
workflows and commands.

1. General tests

```bash
st2 run st2cd.st2workroom_test hostname=st2woroom-test-manual-1 version=1.1.1 build=8 revision=d07cbe6f9def8a6f2a77b1f1af64189fa6b870be -a
```

2. Enterprise bits tests

```bash
st2 run st2cd.st2workroom_st2enterprise_test hostname=st2woroom-test-manual-2 version=1.1.1 build=8 revision=d07cbe6f9def8a6f2a77b1f1af64189fa6b870be -a
```

Note: After you are done and if the VM doesn't get destroyed, you can destroy
it using ``destroy_vm`` action. For example:

```bash
st2 run st2cd.destroy_vm hostname=st2woroom-test-manual-1
```
