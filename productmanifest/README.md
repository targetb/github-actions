Product Manifest
----------------
This GitHub action is intended to be used with ArgoCD and Helm charts to help faciliate an easier GitOps experience.

This GitHub action currently has only been validated against the following CR.IO...
- docker.io

Build Status
------------
[![GitHub CR Build and Push](https://github.com/targetb/github-actions/actions/workflows/main-build.yaml/badge.svg)](https://github.com/targetb/github-actions/actions/workflows/main-build.yaml)

Documented Parameters
---------------------
The following are the documented parameters for this action...


>| Argument | Description | Mandatory |
>| -------- | ----------- | --------- | 
>| `gitops-repo-url` | The URL of the GitOps repo to clone and update | True |
>| `manifest-file` | The relative location of the Helm Values chart to update | True |
>| `github-username` | Name of the destination username/organization | True |
>| `github-email` | Name of the destination username/organization email | True |
>| `image-list` | Image list to process. This is a CSV and takes the form of a string like (helmChartName):(dockerImage):(tag) | False |
>| `image-list-file` | Image list to process. This is a CSV and takes the form of a string like (helmChartName):(dockerImage):(tag). This will override `image-list` | False |
>| `git-token` | Value of the Git Token to use for the commit - usually a GITHUB PAT | True |
>| `registry-server` | Docker registry server where images are held | True |
>| `docker-username` | Docker username to login as | True |
>| `docker-passwd` | Docker password to use for the login | True |

You must specify either `image-list` or `image-list-file`.

Example usage
-------------
The following are some samples of usage.
   
```yaml
   - name: GitOps Helm Update
     uses: targetb/github-actions/productmanifest
     env:
      API_TOKEN_GITHUB: ${{ secrets.API_TOKEN_GITHUB }}
     with:
      gitops-repo-url: https://github.com/targetb/gitops-repo
      manifest-file: helm/dev/values.yaml
      github-username: ${{ secrets.GIT_USER }}
      github-email: ${{ secrets.GIT_EMAIL }}
      image-list: wscs-deployment:devops/nodejsvc:master,wsnodejs-b-deployment:devops/nodejsvc:1.0 
      git-token: ${{ secrets.API_TOKEN_GITHUB }}
      registry-server: docker.io
      docker-username: ${{ secrets.DOCKER_USER }}
      docker-passwd: ${{ secrets.DOCKER_PWD }}
```

Notes
-----
- Make sure you have NO spaces in the dockerlist you submit. If you do, then it will not be parsed correctly - use `chartname:image:tag,chartname:image:tag,chartname:image:tag`, NOT `chartname:image:tag, chartname:image:tag, chartname:image:tag`
- This GitHub action has only been tested against `docker.io` using public repos. Additional support may need to be added for other repo types. This can be done best by using their REST API as `docker` is flakey 
- Currently, this action uses Docker REST APIs rather than docker itself for the work. This is because it is generally quicker, but not so portable. If portability becomes an issue, I will make this addon work as a `dnd` system

References
----------
- https://hub.docker.com/_/docker/?tab=tags (DnD)
- https://docs.github.com/en/rest/reference/packages
- https://docs.docker.com/registry/spec/api/#listing-repositories
- https://github.community/t/ghcr-io-docker-http-api/130121
- https://docs.docker.com/registry/spec/api/#:~:text=The%20Docker-Content-Digest%20header%20returns%20the%20canonical%20digest%20of,verify%20the%20value%20against%20the%20uploaded%20blob%20data.
- https://www.deployhub.com/docker-image-digest-using-v2-registry-api/
- https://docs.docker.com/engine/api/v1.41/#section/Versioning
