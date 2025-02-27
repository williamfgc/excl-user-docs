---
description: Getting started with Gitlab CI runners in code.ornl.gov running on ExCL systems.
---

# GitLab CI Runners in ExCL

## Register a Runner

Runners can be registered as either a group runner or for a single repository (also know as a project runner). Group runners are are made available to all the repositories in a group.

Send the following information to [excl-help@ornl.gov](mailto:excl-help@ornl.gov?subject=Request%20GitLab%20Runner%20Registration&body=Request%20to%20register%20an%20ExCL%20node%20as%20a%20GitLab%20Runner:%0D%0A%0D%0A-%20URL:%20https://code.ornl.gov/%0D%0A-%20Registration%20Token:%20%20%0D%0A-%20Executor:%20(choose%20shell%20or%20docker%20with%20image)%20%0D%0A-%20Project%20Name:%20%0D%0A-%20ExCL%20System:%20%0D%0A-%20Tag%20List:)

- URL
- Registration Token
- Executor (choose shell or docker with image)
- Project Name (This can be group name or repo name)
- ExCL System
- Tag List

The method for finding the registration differs depending on if you want to register a group runner or a single repository runner. See [Group Runner](#group-runner) and [Single Repo Runner (Project Runner))](#single-repo-runner-project-runner) sections below.

After the runner is added, you can edit the runner to change the tags and description.

### Group Runner

Navigate to the group page. Click on `Build` → `Runners`. Choose the vertical dots at the top of the page next to `New group runner` and copy registration token.

### Single Repo Runner (Project Runner)

Navigate to the repo page. Click on `Settings` → `CI/CD` → `Runners`. Choose the vertical dots next to `New project runner` and copy registration token.

## List of ExCL Systems with a runner

Any system can be requested as a runner. These systems are already being used as a runner. (Updated October 2023)

- docker.ftpn.ornl.gov
- explorer.ftpn.ornl.gov
- intrepid.ftpn.ornl.gov
- justify.ftpn.ornl.gov
- leconte.ftpn.ornl.gov
- lewis.ftpn.ornl.gov
- milan2.ftpn.ornl.gov
- milan3.ftpn.ornl.gov
- oswald00.ftpn.ornl.gov
- oswald02.ftpn.ornl.gov
- oswald03.ftpn.ornl.gov
- pcie.ftpn.ornl.gov
- zenith.ftpn.ornl.gov

## Using Slurm with Gitlab CI

The system slurm-gitlab-runner is setup specifically to run CI jobs that then run the execution using slurm with `sbatch --wait`.

## Spack and Conda with Gitlab-CI

```yml
.setup:
  tags: [shell]
  before_script:
    - source /auto/ciscratch/spack/share/spack/setup-env.sh
    - source /auto/ciscratch/conda/etc/profile.d/conda.sh

build:
  extends: [.setup]
  script: 
    - spack env create -d . spack.yaml
    - spack env activate .
    - spack install
    - conda create -p ./envs
    - conda activate ./envs
    - conda install pip
```
