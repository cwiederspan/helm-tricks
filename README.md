## Helm-related Workshops

* <https://github.com/deislabs/helm-workshop>
* https://github.com/Microsoft/MCW-Containers-and-DevOps
* https://github.com/Azure/kubernetes-hackfest


## Create an ImagePullSecret on the Kubernetes Cluster

```bash
kubectl -n dev create secret docker-registry myacrname \
  --docker-server=myacrname.azurecr.io \
  --docker-username=*** \
  --docker-password=*** \
  --docker-email=ServicePrincipal@AzureRM
```

## Helm Charts and ImagePullSecrets

### SCENARIO #1
If your chart's deployment.yaml file looks like this...

```yaml
...
    {{- with .Values.imagePullSecrets }}
      imagePullSecrets:
{{ toYaml . | indent 8 }}
    {{- end }}
...
```
...then your helm command should look like this...

```bash
helm template ./mychart --set imagePullSecrets={name:myimagepullsecret}
```
> NOTE: Watch out for spaces after the <i>imagePullSecretes=</i> as they will cause problems

<br/>

### SCENARIO #2
If your chart's deployment.yaml file looks like this...

```yaml
...
    {{- if .Values.imagePullSecrets }}
      imagePullSecrets:
      {{- range .Values.imagePullSecrets }}
        - name: {{ . }}
      {{- end}}
    {{- end }}
...
```
...then your helm command should look like this...

```bash
helm template ./mychart --set imagePullSecrets={myimagepullsecret}
```
> NOTE: Don't include the name attribute inside the array
