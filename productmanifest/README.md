# Product Manifest

This GitHub action is intended to be used with ArgoCD and Helm charts to help facilitate an easier
GitOps experience.

## Build Status

[![GitHub CR Build and Push](https://github.com/targetb/github-actions/actions/workflows/main-build.yaml/badge.svg)](https://github.com/targetb/github-actions/actions/workflows/main-build.yaml)

## Documented Parameters

The following are the documented parameters for this action...

| Argument          | Description                                                                                                                                    | Mandatory |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------|-----------|
| `gitops-repo-url` | The URL of the GitOps repo to clone and update                                                                                                 | True      |
| `manifest-file`   | The relative location of the Helm chart to update                                                                                              | True      |
| `github-username` | Name of the destination username/organization                                                                                                  | True      |
| `github-email`    | Name of the destination username/organization email                                                                                            | True      |
| `github-token`    | Value of the GitHub Token to use for the commit - usually a GITHUB PAT                                                                         | True      |
| `image-list`      | Image list to process. This is a CSV and takes the form of a string like (helmChartName):(dockerImage):(tag)                                   | False     |
| `image-list-file` | Image list to process. This is a CSV and takes the form of a string like (helmChartName):(dockerImage):(tag). This will override `image-list`. | False     |
| `registry-server` | Docker registry server where images are held. Currently, only `docker.io` and `ghcr.io` are supported. Default value: `ghcr.io`                | False     |
| `docker-username` | Docker username to login as                                                                                                                    | True      |
| `docker-password` | Docker password to use for the login                                                                                                           | True      |
| `debug`           | [Optional] Debug flag to pass into the command. Valid value: `--debug`                                                                         | False     |
| `tag-string`      | [Optional] Tag string to use in the Helm values file. Example: `.containerImage.tag`                                                           | False     |
| `image-tag`       | [Optional] Specify a tag string which will be used instead of dynamically calculating the SHA                                                  | False     |

You must specify either `image-list` or `image-list-file`.

## Example usage

The following are some samples of usage...

Standard usage:

```yaml
   - name: GitOps Helm Chart Update
     uses: targetb/github-actions/productmanifest@main
     env:
       GITHUB_TOKEN: ${{ secrets.GITHUB_PAT }}
     with:
       gitops-repo-url: https://github.com/targetb/gitops-controller
       manifest-file: helm/dev/values.yaml
       image-list: helm-chart-1:targetb/service-example-1:latest,helm-chart-2:targetb/service-example-2:1.0
       github-username: ${{ secrets.GITHUB_USERNAME }}
       github-email: ${{ secrets.GITHUB_EMAIL }}
       github-token: ${{ env.GITHUB_TOKEN }}
       registry-server: docker.io
       docker-username: ${{ secrets.DOCKERHUB_USERNAME }}
       docker-password: ${{ secrets.DOCKERHUB_PASSWORD }}
```

Image list file sample:

```yaml
   - name: GitOps Helm Chart Update(s)
     uses: targetb/github-actions/productmanifest@main
     env:
       GITHUB_TOKEN: ${{ secrets.GITHUB_PAT }}
     with:
       gitops-repo-url: https://github.com/targetb/gitops-controller
       manifest-file: helm/dev/values.yaml
       image-list-file: helm/imagelist.txt
       github-username: ${{ secrets.GITHUB_USERNAME }}
       github-email: ${{ secrets.GITHUB_EMAIL }}
       github-token: ${{ env.GITHUB_TOKEN }}
       registry-server: docker.io
       docker-username: ${{ secrets.DOCKERHUB_USERNAME }}
       docker-password: ${{ secrets.DOCKERHUB_PASSWORD }}
```

Some environment substitution sample using `ghcr.io`

```yaml
   - name: GitOps Helm Chart Update(s)
     uses: targetb/github-actions/productmanifest@main
     env:
       GITHUB_TOKEN: ${{ secrets.GITHUB_PAT }}
       REGISTRY: ghcr.io
     with:
       gitops-repo-url: https://github.com/targetb/gitops-controller
       manifest-file: helm/dev/values.yaml
       github-username: ${{ secrets.GITHUB_USERNAME }}
       github-email: ${{ secrets.GITHUB_EMAIL }}
       github-token: ${{ env.GITHUB_TOKEN }}
       image-list: helm-chart:${{ env.REGISTRY }}/${{ github.actor }}/example-service:latest
       registry-server: ${{ env.REGISTRY }}
       docker-username: ${{ github.actor }}
       docker-password: ${{ env.GITHUB_TOKEN }}
```

## Notes

- Make sure you have NO spaces in the dockerlist you submit. If you do, then it will not be parsed
  correctly - use `chartname:image:tag,chartname:image:tag,chartname:image:tag`,
  NOT `chartname:image:tag, chartname:image:tag, chartname:image:tag`
- This GitHub action has only been tested against `docker.io` and `ghcr.io`. Additional support may
  need to be added for other repo types. This can be done best by using their REST API as `docker`
  is flaky.
- Currently, this action uses Docker REST APIs rather than docker itself for the work. This is
  because it is generally quicker, but not so portable.
- If using `docker.io` you will need to specify your DockerHub user password as
  the `docker-password`value.
- If using `ghcr.io` you will need to use your GitHub PAT token as the `docker-password` value.
