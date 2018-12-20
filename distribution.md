# Distribution

This section describes how to build and deploy publicly available releases of
the Step CLI.

## Creating A New Release

New releases are (almost) entirely built and deployed by Travis-CI. Creating a new
release is as simple as pushing a new github tag.

**Definitions**:

* **Standard Release**: ready for public use. no `-rc*` suffix on the version.
e.g. `v1.0.2`
* **Release Candidate**: not ready for public use, still testing. must have a
`-rc*` suffix. e.g. `v1.0.2-rc` or `v1.0.2-rc.4`

1. [Optiona] Bump component versions in [.COMPONENT_VERSIONS](.COMPONENT_VERSIONS).

    The versions from this file are used to build a Homebrew bundle that
    simultaneously delivers executables from multiple Step repositories.

2. Find the most recent.

    ```
    git fetch --tags
    git tag
    ```

    The new tag needs to be the logical successor of the most recent existing tag.
    See [versioning](./README.md#versioning) section for more information on version numbers.

3. Select the next tag (but don't actually tag anything yet!!).

    Is the new release a *release candidate* or a *standard release*?

    1. Release Candidate

        If the most recent tag is a standard release, say `v1.0.2`, then the version
        of the next release candidate should be `v1.0.3-rc.1`. If the most recent tag
        is a release candidate, say `v1.0.2-rc.3`, then the version of the next
        release candidate should be `v1.0.2-rc.4`.

    2. Standard Release

        If the most recent tag is a standard release, say `v1.0.2`, then the version
        of the next standard release should be `v1.0.3`. If the most recent tag
        is a release candidate, say `v1.0.2-rc.3`, then the version of the next
        standard release should be `v1.0.3`.

4. Update the remote origin with your commits.

    Make sure that the local checkout is up to date with the remote origin and
    that all local changes have been pushed.

    ```
    git pull --rebase origin master
    git push
    ```

5. Create a local tag.

    ```
    git tag v1.0.3   # standard release
    ...or
    git tag v1.0.3-rc.1  # release candidate
    ```

6. Push the new tag to the remote origin.

    ```
    git push origin tag v1.0.3   # standard release
    ...or
    git push origin tag v1.0.3-rc.1  # release candidate
    ```

7. Check the build status at
[Travis-CI](https://travis-ci.com/smallstep/cli/builds/).

    Travis will begin by verifying that there are no compilation or linting errors
    and then run the unit tests. Assuming all the checks have passed, Travis will
    build Darwin and Linux artifacts (for easily installing `step`) and upload them
    as part of the [Github Release](https://github.com/smallstep/cli/releases).

    Travis will build and upload the following artifacts:

    * **brew_step_1.0.3.tar.gz**: tarball used by the brew formula.
    * **step-cli_1.0.3_amd64.deb**: debian package for installation on linux.
    * **step_1.0.3_linux_amd64.tar.gz**: tarball containing a statically compiled linux binary.
    * **step_1.0.3_darwin_amd64.tar.gz**: tarball containing a statically compiled darwin binary.

8. Update the Homebrew formula.

    **NOTE**: this only needs to be done for standard releases.

    Follow the steps [here](https://github.com/smallstep/homebrew-smallstep#how-to-update-the-formula).

*All Done!*
