# Values Files

���� �忡���� Helm ���ø��� ��ü������ �����ϴ� ������Ʈ�鿡 ���ؼ� ���캸�ҽ��ϴ�. `Values` ���� ��ü������ ����Ǿ� �ִ� ������Ʈ �� �ϳ��Դϴ�. �� ������Ʈ�� ��Ʈ�� ���޵Ǵ� ���鿡 �����ϱ� ���� �뵵�� ���˴ϴ�. `Values` �� ������ �� 4���� ����� ���� ������ �� �ֽ��ϴ�.

- ��Ʈ ���ο� ��ġ�� `values.yaml` ����
- ���� ��Ʈ�� ������ ���, ���� ��Ʈ�� `values.yaml` ����
- `helm install` ��ɾ `helm upgrade` ��ɾ�� `-f` �ɼ��� ���� ���޵� values ���� (ex: `helm install -f myvals.yaml ./mychart`)
- `--set` �ɼ��� ���� ������ �������� ���ڵ� (`helm install --set foo=bar ./mychart` �� ���� ��� ����)

�� ����� ���� �켱������� ������ ���Դϴ�. ��, [1. values.yaml < 2. ���� ��Ʈ�� values.yaml < 3. helm install -f �� ������ ���� < 4. --set �ɼǿ� �����Ǵ� ��] ������� �켱������ �����ϴ�. ���� `--set` �ɼ��� ���� ū ȿ���� ������, `--set` �� ���� ���� �����ϸ� �켱������ ���� ������ ��� ���� �������ϴ�.

��·��, Values ������ �⺻������ �ܼ��� YAML ���Ͽ� �Ұ��մϴ�. �̹����� `mychart/values.yaml` ������ ���� �����غ� ��, ConfigMap�� ���ø��� ������ ���ڽ��ϴ�.

`values.yaml` ���Ͽ� �⺻������ ������ ������ ��� ������ ��, �ϳ��� ������ ������ ���ڽ��ϴ�.

```yaml
favoriteDrink: coffee
```

�� ��, ���ø� ���ο��� �Ʒ��� ���� ����� �� �ֽ��ϴ�.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favoriteDrink }}
```

������ �ٿ��� `favoriteDrink` ��� ���� `Values`: `{{ .Values.favoriteDrink }}` ��� �Ӽ��� ���� �����ߴٴ� �Ϳ� �ָ��մϴ�.

��, �׷��ٸ� ���������� ��� ���� �������Ǵ��� Ȯ���� ���ڽ��ϴ�.

```console
$ helm install --dry-run --debug ./mychart
SERVER: "localhost:44134"
CHART PATH: /Users/mattbutcher/Code/Go/src/k8s.io/helm/_scratch/mychart
NAME:   geared-marsupi
TARGET NAMESPACE:   default
CHART:  mychart 0.1.0
MANIFEST:
---
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: geared-marsupi-configmap
data:
  myvalue: "Hello World"
  drink: coffee
```

`favoriteDrink` ��� ���� `values.yaml` ���Ͽ� `coffee` ��� ������ ���ǵǾ��� ������, �� ���� ���ø��� �� �� �ֽ��ϴ�.
�̷��� ���� ���� `helm install` ��ɾ�� `--set` �ɼ��� ���ؼ��� ���� ������ �� �ֽ��ϴ�. ����, values.yaml ���� ��� ������ ������ ��� ���õ˴ϴ�.

```console
$ helm install --dry-run --debug --set favoriteDrink=slurm ./mychart
SERVER: "localhost:44134"
CHART PATH: /Users/mattbutcher/Code/Go/src/k8s.io/helm/_scratch/mychart
NAME:   solid-vulture
TARGET NAMESPACE:   default
CHART:  mychart 0.1.0
MANIFEST:
---
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: solid-vulture-configmap
data:
  myvalue: "Hello World"
  drink: slurm
```

`--set` �ɼ��� `values.yaml` ���Ϻ��� �켱������ ���� ������, ���� ���ÿ��� ���ø��� ������ `drink: slurm` ��� ���� �����ϴ�.

Values ���Ͽ��� �� �� ����ȭ�� �����͸� ���� ���� �ֽ��ϴ�. ���� ���, `favorite` �׸��� ������ ������ ��, ���� �׸����� ���� ������ ������ ���� �ֽ��ϴ�.

```yaml
favorite:
  drink: coffee
  food: pizza
```

��, �׷��ٸ� ���ø��� ������ ��¦ �ٲ㺸���� ����. .Values.favorite.drink �� ���� ������� �������� ������ �����͸� ����� �� �ֽ��ϴ�.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink }}
  food: {{ .Values.favorite.food }}
```

����ȭ�� �����͸� ����� ���� �����ϴٸ� values tree (�� ���� ����) �� ���� ����, �׷��ϱ� �ʹ� ������ ������ ������ �ʴ� ���� �����ϴ�. 

## Deleting a default key

�������� �⺻ ���� �߿��� Ư�� Ű�� �ش��ϴ� ���� �����ؾ� �Ѵٸ�, �ش� Ű�� ���� `null` �� �����ϸ� �˴ϴ�. �׷��� �ϸ� Helm�� �ش� Ű�� ���� �����մϴ�.

���� ���, stable/Drupal ��� ��Ʈ�� liveness probe�� ������ �� �ֵ��� �ɼ��� �����ϰ� �ֽ��ϴ�. �Ʒ��� �ش� ��Ʈ�� �⺻ �����Դϴ�.

```yaml
livenessProbe:
  httpGet:
    path: /user/login
    port: http
  initialDelaySeconds: 120
```

���� �������� `--set livenessProbe.exec.command=[cat,docroot/CHANGELOG.txt]` �ɼ��� ���� livenessProbe �ڵ鷯�� `httpGet` ��� `exec` �� �ٲٷ��� �õ��� ���, Helm�� �⺻ ���� ���� �������� ���� ��Ĩ�ϴ�. ��, �Ʒ��� ���� YAML�� �����˴ϴ�. httpGet�� exec�� �����ϰ� �ֽ��ϴ�.

```yaml
livenessProbe:
  httpGet:
    path: /user/login
    port: http
  exec:
    command:
    - cat
    - docroot/CHANGELOG.txt
  initialDelaySeconds: 120
```

�׷���, �����Ƽ���� 1������ ���� livenessProbe �ڵ鷯�� �����ϴ� ���� ������� �ʽ��ϴ�. �̸� �ذ��ϱ� ���ؼ�, Helm���� `livenessProbe.httpGet` �� ���� �����϶�� ���Ƿ� ������ ���� �ֽ��ϴ�. �ٷ� null�� �����ϴ� ���Դϴ�.

```sh
helm install stable/drupal --set image=my-registry/drupal:0.1.0 --set livenessProbe.exec.command=[cat,docroot/CHANGELOG.txt] --set livenessProbe.httpGet=null
```

���ݱ��� Helm ���ø��� ����� �� ���� ������Ʈ�鿡 ���ؼ� �˾ƺ��ҽ��ϴ�. �׸��� �׷��� ������ ���ø����� �������� ����� �˾ƺ�����. ���� �忡���� ���ø� ������ ���� �Լ��� ������������ ����ϴ� ����� ���ؼ� �˾ƺ��ڽ��ϴ�.

�� ������ ���� �������͸� Ŀ�� : [40bc1b1](https://github.com/helm/helm/commit/40bc1b173d2d20591c0d9bf9d690e06bc0de7c55) (on 4 Jun 2019)
