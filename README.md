# GitHub Actions

This repo is used to track any GitHub actions which have been specifically developed or customised for use within
Mortgage Hub. For information on how to develop such actions, please see the notes section below.

## Build Status

[![GitHub CR Build and Push](https://github.com/targetb/github-actions/actions/workflows/main-build.yaml/badge.svg)](https://github.com/targetb/github-actions/actions/workflows/main-build.yaml)

## GitHub Actions
The following are custom GitHub actions that have been developed for use within Mortgage Hub.

>| Action | Description |
>| -------- | ----------- |
>| [productmanifest/](https://github.com/targetb/github-actions/tree/main/productmanifest) | This GitHub action is intended to be used with Helm based GitOps repos to update manifest files with rebuilt Docker image repo digests. This will trigger builds in monitored GitOps repos |

## Notes

- https://docs.github.com/en/actions/creating-actions/creating-a-docker-container-action
- https://docs.github.com/en/rest/reference/packages
- https://docs.github.com/en/github-ae@latest/rest/reference/actions#workflow-runs
