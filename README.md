# helm-tricks
An explanation of some tips and tricks related to Helm and Helm Charts.

## Create an ImagePullSecret on

## Helm Charts and ImagePullSecrets

### SCENARIO #1:
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

***

### SCENARIO #2:
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