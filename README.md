The following command makes ct install the charts but it also tests crdb which fails in secure mode

```bash
kubectl delete namespace zitadel-test || kubectl create namespace zitadel-test && helm template --namespace=zitadel-test charts/zitadel-v2/ --set=zitadel.masterkey=x123456789012345678901234567891y --set=zitadel.secretConfig.Database.User.Password=xy --set=zitadel.configmapConfig.ExternalDomain=test.domain | kubectl apply -f - --selector="app.kubernetes.io/name=cockroachdb,app.kubernetes.io/component=init" && docker run -it --network host --volume ~/.kube/config:/root/.kube/config:ro --volume ~/.helm:/root/.helm:ro --volume ~/.config/helm:/root/.config/helm:ro --volume ~/.cache/helm:/root/.cache/helm:ro --volume $(pwd):/wd -w /wd --rm quay.io/helmpack/chart-testing:v3.6.0 ct install --namespace=zitadel-test --all --helm-extra-set-args "--set=zitadel.masterkey=x123456789012345678901234567891y --set=zitadel.secretConfig.Database.User.Password=xy --set=zitadel.configmapConfig.ExternalDomain=test.domain" --print-config --debug
```

Maybe its better to switch to terratest?
https://camunda.com/blog/2022/03/test/

