# vcpkg

This repository is a fork of https://github.com/microsoft/vcpkg.  

The default branch is "main" which is empty except for a github action that prebuilds various ports for the [HPCC-Platform](https://github.com/hpcc-systems/HPCC-Platform) repository.

## Active Branches

The following branches are active and correspond to the HPCC-Platform development branches:

* [hpcc-platform-9.8.x](https://github.com/hpcc-systems/vcpkg/tree/hpcc-platform-9.8.x)
* [hpcc-platform-9.6.x](https://github.com/hpcc-systems/vcpkg/tree/hpcc-platform-9.6.x)
* [hpcc-platform-9.4.x](https://github.com/hpcc-systems/vcpkg/tree/hpcc-platform-9.4.x)
* [hpcc-platform-9.2.x](https://github.com/hpcc-systems/vcpkg/tree/hpcc-platform-9.2.x)
* [hpcc-platform-8.12.x](https://github.com/hpcc-systems/vcpkg/tree/hpcc-platform-8.12.x)
* [hpcc-platform-8.8.x](https://github.com/hpcc-systems/vcpkg/tree/hpcc-platform-8.8.x)

## Development Notes:

### Creating a branch for new HPCC-Platform releases

To create a new branch for a new HPCC-Platform release, follow these steps:

Prerequisites:
1. git clone this repository (e.g. `git clone git@github.com:hpcc-systems/vcpkg.git`)
2. Add the official vcpkg repository as another remote called "microsoft" (e.g. `git remote add microsoft git@github.com:microsoft/vcpkg.git`)

In https://github.com/hpcc-systems/vcpkg:

1. `git fetch --all --tags` to ensure you have all the latest changes
2. Check https://github.com/microsoft/vcpkg/releases to locate the latest release (e.g. 2024.05.24)
3. Checkout the latest HPCC-Platform release branch (e.g. hpcc-platform-9.6.x)
4. Create a new branch for the new HPCC-Platform release (e.g. hpcc-platform-9.8.x)
5. Update the vcpkg.json file to include the new HPCC-Platform release version (e.g. "version": "9.8.0")
6. Commit the changes
7. Rebase the branch on the latest version as identified in step 2 above ensuring you squash the rebased commits with a suitable comment (e.g. "chore:  Squash commits for 9.8.0 gold release
")
8. In the HPCC-Platform repository, check the "vcpkg_overlays" folder for any new overlays that need to be added to the "overlays" folder in this repository.  If found, copy them to the "overlays" folder in this repository
9. Compare the contents of the "overlays" folder with the "vcpkg" folder.  If any of the overlays match or are older than the vcpkg folder, delete them.
10. Commit the changes with the `--amend` option to update the previous commit
11. Push the new branch to https://github.com/hpcc-systems/vcpkg 
12. Checkout the "main" branch
13. Update this README.md file to include a link to the new branch in the "Active Branches" section
14. Update the .github/workflows/prebuild.yml file to include two new jobs corrisponding to the new branch:
    * prebuild-docker-9-8
    * prebuild-gh_envs-9-8
15. Commit the changes and issue a PR to the vcpkg repository

**Note:**  Step 11 above will trigger several github actions to prebuild the new branch.  Once the prebuilds are complete, the new branch can be used in the HPCC-Platform repository.

In https://github.com/hpcc-systems/HPCC-Platform:

1. Create a new branch based on the "latest" candidate branch (e.g. candidate-9.8.x)
2. Delete the contents of the "vcpkg_overlays" folder (as they have been relocated with step 8 above).
3. Fetch the latest vcpkg submodule repository changes (e.g. `cd vcpkg && git fetch --all`)
4. Checkout the new branch created in step 4 above (e.g. `cd vcpkg && git checkout hpcc-platform-9.8.x`)
5. Commit the changes and issue a PR to the HPCC-Platform repository

