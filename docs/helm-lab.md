# Helm Lab

## Add Repo

helm repo add bitnami https://charts.bitnami.com/bitnami

helm repo update

## Install Chart

helm install my-nginx bitnami/nginx

## Verify

helm list

kubectl get pods

kubectl get svc

## Upgrade

helm upgrade my-nginx bitnami/nginx

## History

helm history my-nginx

## Rollback

helm rollback my-nginx 1

## Status

helm status my-nginx
