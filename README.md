# Notes API

A small notes service built for Week 3.

## Features
- Create, read, update, and delete notes
- Standardized REST API endpoints

## How to run
./scripts/run.sh

## How to test
./scripts/test.sh

## Configuration
Uses $PORT environment variable, defaults to 8080.

## Deployment

Argo CD deploys this service from Git (`argocd/application.yaml`): it follows the `main`
branch and applies the `k8s/` folder to the `default` namespace, with `prune` and
`selfHeal` on. CI only builds, scans and publishes the image; it never deploys.
To deploy a new version, change the image tag in `k8s/deployment.yaml` to the full commit
SHA in a pull request and merge it.

## Rollback

Argo CD deploys what is on the `main` branch, from the `k8s` folder.
To roll back, undo the commit that deployed the bad version, with a new commit:

    git log --oneline -- k8s
    git revert <that commit>          # a merge commit: git revert -m 1 <that merge commit>
    git push

Argo CD deploys the previous version within three minutes. Do not use
`kubectl rollout undo`: self-heal would put the bad version back.
