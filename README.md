# How to make a release

## Versioning

Update libyaml version in:
* announcement.msg
* CMakeLists.txt
  * `YAML_VERSION_MAJOR`, `YAML_VERSION_MINOR`, `YAML_VERSION_PATCH`
* appveyor.yml
* configure.ac
  * `YAML_MAJOR`, `YAML_MINOR`, `YAML_PATCH`, `YAML_RELEASE`, `YAML_CURRENT`, `YAML_REVISION`

Commit and push everything to release/0.x.y

## Create dist archives

Make sure you have a clean git repository (no changed files). The following
process will clone your current git directory.

Run

    make docker-dist

in the repository root or

    make libyaml-dist

in packaging/docker.

It will create a docker image (libyaml-dev) and run `make dist` in the container
to create a tarball written to packaging/docker/output.
It will also create a zipfile.

## Update dist branch

    git worktree add dist dist
    cd dist
    rm -r *
    cp -r ../packaging/docker/output/yaml-0.x.y/* .

Check diffs and commit changes.

    git tag dist-0.x.y
    git push origin dist dist-0.x.y

## Update master

    git merge release/0.x.y
    git tag -a 0.x.y
    # <Editor opens>
    # Paste the corresponding entry from the Changes file
    # Look at an earlier release for how it should look like:
    #   git show 0.2.3
    git push origin master 0.x.y

## Update pyyaml.org

* Put the resulting tarball & zip in pyyaml.org/download/libyaml
* Update wiki/LibYAML.md with release number and date
* Update wiki/index.md

Run

    make update

Check diffs and commit and push.
