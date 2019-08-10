# Flow Control (�帧 ����)

���ø����� ���� "actions (�׼�)" ��� �Ҹ��� ���� ���ø� ������ �帧�� ������ �� �ִ� ����� �����մϴ�. Helm ���ø� ���� ������ �帧 ���� ������ �����ϰ� �ֽ��ϴ�.

- ���ǹ� ����� ���� `if`/`else` ����
- ���� ������ ���� `with` ����
- for ����� �ݺ��� ����� ���� `range` ����

�̷��� �� �ܿ���, �̸��� �ο��� (named) ���ø� �ι��� ���ؼ� �Ʒ��� �׼��� �����մϴ�.

- ���ø� �ȿ��� �̸��� �ο��� (named) ���ø��� �������ִ� `define` ����
- �̸��� �ο��� (named) ���ø��� import�� �� �ִ� `template` ����
- ä�� ���� �� �ִ� (fillable) ���ø� ������ ���Ǹ� �ǹ��ϴ� `block` ����

�̹� ���̵忡���� `if`, `with`, `range`�� ���� ��� ����� �˾ƺ��ڽ��ϴ�. �ٸ� �����鿡 ���ؼ��� "Named Templates" ���̵� é�Ϳ��� �ٷ� �����Դϴ�.

## If/Else ����

�츮�� ���캼 ù ��° �帧 ���� ������ ���ø����� if/else ������ �̿��� �ؽ�Ʈ ���� ���ø��� ���Խ�Ű�� ���Դϴ�. ���� �⺻���� ���ǹ� ������ �Ʒ��� �����ϴ�.

```yaml
{{ if PIPELINE }}
  # ���� ���ϴ�.
{{ else if OTHER PIPELINE }}
  # ���� �ٸ� ������ ���ϴ�.
{{ else }}
  # �⺻ ���� ���ϴ�.
{{ end }}
```

���� if ���ǹ����� �ٷ�� �ִ� ���� Ư�� ���� �ƴ� _pipelines_ ��� ���� �����մϴ�. ���� �ƴ� ������������ ���� ���ǹ����� ����ϴ� ������ �帧 ���� ������ ���ؼ��� ������������ ������ �� �ִٴ� ���� �����ֱ� �����Դϴ�.

������������ �Ʒ��� ����� �򰡵Ǿ��� ��쿡 ���ؼ� _false_ �� ��޵˴ϴ�.

- �Ҹ� �� false
- ���� 0
- ��� �ִ� ���ڿ�
- `nil` (��� �ְų� null�� ���)
- ��� �ִ� �÷��� (`map`, `slice`, `tuple`, `dict`, `array`)

���� ������ ������ ��쿡�� ���� _true_ �� ��޵Ǹ�, ������������ ����� ���Դϴ�.

���Ǳ׸��� �̿��� ������ ���ǹ��� ������ ���ڽ��ϴ�. if���� �̿��� drink ������ ���� coffee�� �����Ǿ� ������ �ٸ� ���� ���� �߰��ϴ� ���ø� ���캸�ڽ��ϴ�.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink | default "tea" | quote }}
  food: {{ .Values.favorite.food | upper | quote }}
  {{ if and .Values.favorite.drink (eq .Values.favorite.drink "coffee") }}mug: true{{ end }}
```

`.Values.favorite.drink`�� ���� ��򰡿� ���ǵǾ� �־�� �մϴ�. �׷��� ������ coffee��� ���� ���� �� ������ ����� ���Դϴ�. ���� ���̵� é�Ϳ��� `drink: coffee` �� �ּ�ó���߱� ������, �������� ���� ���ø��� `mug: true` ��� �÷��׸� �����ϰ� ���� ���� ���Դϴ�. �׷����� `values.yaml` ���Ͽ��� �ٽ� �ּ�ó���� ������ �ּ� �����Ѵٸ�, ���ø��� �������� ������� �Ʒ��� ���� ���Դϴ�.

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: eyewitness-elk-configmap
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "PIZZA"
  mug: true
```

## Controlling Whitespace (���� ����ϱ�)

���ǹ��� ����Ϸ��� ���ø����� ������ ��� ���Ǵ����� �Բ� �˾Ƶδ� ���� �����ϴ�. ���� ���ø� �� �� �б� ���� �ٽ� �ۼ��� ���ڽ��ϴ�.

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink | default "tea" | quote }}
  food: {{ .Values.favorite.food | upper | quote }}
  {{if eq .Values.favorite.drink "coffee"}}
    mug: true
  {{end}}
```

���Ӱ� ��ģ ���ø��� �б⿡ ���ƺ��̱� ������, ������ ���ø� ������ ������ �����ϸ� �Ʒ��� ���� ������ ����� ���Դϴ�.

```console
$ helm install --dry-run --debug ./mychart
SERVER: "localhost:44134"
CHART PATH: /Users/mattbutcher/Code/Go/src/k8s.io/helm/_scratch/mychart
Error: YAML parse error on mychart/templates/configmap.yaml: error converting YAML to JSON: yaml: line 9: did not find expected key
```

����ü ���� �߸��� ���ϱ��? 

���� YAML ���Ͽ����� ������ �߸� ����߱� ������ �ùٸ��� ���� YAML ������ ��µǾ����ϴ�. �Ʒ��� YAML ������ ���ڽ��ϴ�.

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: eyewitness-elk-configmap
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "PIZZA"
    mug: true
```

`mug` �� ���ο��� �ε�Ʈ�� �߸� �ԷµǾ����ϴ�. �̸� �����ϱ� ���ؼ� �ܼ��� �Ʒ��� ���� �ε�Ʈ�� �����ϰ� �ٽ� ���ø� ������ ������ ���ڽ��ϴ�.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink | default "tea" | quote }}
  food: {{ .Values.favorite.food | upper | quote }}
  {{if eq .Values.favorite.drink "coffee"}}
  mug: true
  {{end}}
```

���� YAML ������ ��ȿ������, ������ ���ø��� �������Ǹ� ���� ����� ����� ��µ˴ϴ�.

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: telling-chimp-configmap
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "PIZZA"

  mug: true

```

���� YAML ���Ͽ����� �� ������ �Բ� ��µǾ����ϴ�. ���ϱ��? ���ø� ������ ����� ��, ������ `{{` �� `}}` �׸��� _�����մϴ�_. �׷����� ���� ������ ��Ȯ�� �״�� ���ܵα� ��������.

��������, Helm ���ø����� �̸� �ذ��� �� �ִ� �� ���� ����� �����մϴ�. 

ù ��°��, �ձ׷��� ���� �극�̽� (curly brace) ������ ����ϸ� ���ø� ������ ������ �����ϵ��� ������ �� �ֽ��ϴ�. `{{- ` ( {{- �� ������ ��ĭ �߰��� �� ) �� ������ ������ ������� �Ѵٴ� ���� �ǹ��ϸ�,  ` -}}`�� �������� ������ ������� �Ѵٴ� ���� �ǹ��մϴ�. _���ο� ������ �������� ��޵ȴٴ� �Ϳ� �����մϴ�._

> `-`�� ������ ���� ���̿� ���� �ϳ��� �ݵ�� �����ؾ� �մϴ�. ���� ��� `{{- 3 }}`�� ������ ������ �����ϰ�, 3�� ����ض� ��� ���� �ǹ��մϴ�. �׷����� `{{-3}}` �� �ܼ��� -3�� ����ض� ��� ���� �ǹ��մϴ�.

�̷��� ������ ����ϸ� ���ο� ������ �����ϱ� ���ؼ� ���ø��� �Ʒ�ó�� ������ �� �ֽ��ϴ�.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink | default "tea" | quote }}
  food: {{ .Values.favorite.food | upper | quote }}
  {{- if eq .Values.favorite.drink "coffee"}}
  mug: true
  {{- end}}
```

���ݸ� �� ����ϰ� ����غ��� ���ؼ�, ���� ������ ���ݸ� ������ ���ڽ��ϴ�. �� ��Ģ�� ���� ������ ������ `*` �� ��ü�� ���ڽ��ϴ�. ������ �������� ���̴� `*` �� ������ ���ο� ������ �ǹ��ϴ� ���ڿ� (\n) �� ���մϴ�. 

> ���� : �� �׸��� ���� �ǹ̴� �����ϴ�. ��� �κ��� ������ �����Ǵ����� *�� ���ؼ� ��Ÿ���� ���Դϴ�. �׳� ���� �ϸ� �˴ϴ�.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink | default "tea" | quote }}
  food: {{ .Values.favorite.food | upper | quote }}*
**{{- if eq .Values.favorite.drink "coffee"}}
  mug: true*
**{{- end}}

```

�� ���� �����ؼ�, Helm ���ø��� �ٽ� ������ ������ �����ϸ� �Ʒ��� ���� ����� ���� �� ���� ���Դϴ�.

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: clunky-cat-configmap
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "PIZZA"
  mug: true
```

��, �̷��� ���ο� ������ ������ ���� ������ ���� �ϳ� �ֽ��ϴ�. �Ʒ��� ���� �Ǽ��� ����� ���� �ֱ� �����Դϴ�.

```yaml
  food: {{ .Values.favorite.food | upper | quote }}
  {{- if eq .Values.favorite.drink "coffee" -}}
  mug: true
  {{- end -}}

```

���� YAML ���ø��� `food: "PIZZA"mug:true` �� ����մϴ�. �극�̽� ���ʿ��� ���� ���ο� ������ �����ع����� �����Դϴ�.

> ���ø����� ������ �ٷ�� �ڼ��� ����� ���ؼ���, [Go ���� ������ �����ϱ� �ٶ��ϴ�.](https://godoc.org/text/template)

����������, ���δ� �̷��� ���ø� ���� ������ ����ϴ� �� ��ſ� ��������� �ε�Ʈ�� �󸶳� ����� �������� �����ϴ� ���� �� ���� ���� �ֽ��ϴ�. ����, `indent` �Լ� ((`{{indent 2 "mug:true"}}`))�� ����ϴ� ���� �� ������� �ֽ��ϴ�. 

## `with`�� �̿��� ������ �����ϱ�

�������� �ٷﺼ ���� ������ `with` �׼��Դϴ�. �̴� ������ ���� (scope) �� ������ �� ���Դϴ�. ������ Values�� ������� �� `.` �� _���� ���� (scope)_ �� ��Ÿ���� ���̾����� �ٽ� ����� ���ڽ��ϴ�. ���� `.Values` �� ���� �������� `Values` ������Ʈ�� ã�Ƽ� ����϶�� ���� �ǹ��߾����ϴ�.

`with` ������ `if` ������ ����մϴ�.

```yaml
{{ with PIPELINE }}
  # restricted scope
{{ end }}
```

scope�� ���� �� �ֽ��ϴ�. `with` �� ���� scope (`.`) �� Ư�� ������Ʈ�� ������ �� �ֵ��� �����մϴ�. ���� ���, �츮�� ���ݱ��� `.Values.favorites` ���� �۾��� �Ծ�����, �̹����� `.` scope��  `.Values.favorites` �� ����Ű���� ���Ǳ׸��� ��¦ ������ ���ڽ��ϴ�.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  {{- with .Values.favorite }}
  drink: {{ .drink | default "tea" | quote }}
  food: {{ .food | upper | quote }}
  {{- end }}
```

> ������ ����ߴ� �������� `if` ������ �����ߴٴ� ��ǿ� �����մϴ�.

`with` ���� �ȿ��� `.drink` �� `.food` ó�� ����ϰ� �ִٴ� ��ǿ� �ָ��մϴ�. �̴� `with` ������ ���ο����� `.` �� ����ϸ�`.Values.favorite` �� ����Ű���� �����Ǳ� �����Դϴ�. `.`�� `with` ������ `{{` �� `}}` �� ����� ������� ���ư��ϴ�.

�׷����� ������ ���� �� ���� �ֽ��ϴ�. ���ѵ� scope ���ο����� ���� scope�� �ٸ� ������Ʈ�� �������� ���մϴ�. ���� ���, �Ʒ��� ���� ����� ������ ���Դϴ�.

```yaml
  {{- with .Values.favorite }}
  drink: {{ .drink | default "tea" | quote }}
  food: {{ .food | upper | quote }}
  release: {{ .Release.Name }}
  {{- end }}
```

`Release.Name` �� `.` scope���� ����� �� ���� ������ ������ ����մϴ�. �׷��� �������� �� ������ ������ �ٲ۴ٸ� �ǵ��Ѵ�� �� ������ ���Դϴ�. �ֳ��ϸ� `{{end}}` �ڷδ� �ٽ� scope�� ������� ���ư��� �����Դϴ�.

```yaml
  {{- with .Values.favorite }}
  drink: {{ .drink | default "tea" | quote }}
  food: {{ .food | upper | quote }}
  {{- end }}
  release: {{ .Release.Name }}
```

`range` ������ ���캻 ��, ���� �̽��� �ذ��� �� �ִ� �ٸ� �ذ� ����� ���ø� ������ ���ؼ� �˾ƺ��ڽ��ϴ�.

## `range` �� ���� ����

���� ���α׷��� ���� `for` ������ ���� �ݺ����� �����մϴ�. Helm ���ø� ������ `range` ���۷����ͷ� �÷����� ���� ���� �ݺ����� ����� �� �ֽ��ϴ�.

�����ϱ� �ռ�, `values.yaml` ���Ͽ� �Ʒ��� ���� ���� ���� (����..) �� ����� �߰��� ���ڽ��ϴ�.

```yaml
favorite:
  drink: coffee
  food: pizza
pizzaToppings:
  - mushrooms
  - cheese
  - peppers
  - onions
```

���ø����� `slice` ��� �Ҹ��� `pizzaToppings` ����Ʈ�� ���Ӱ� �����߽��ϴ�. ���� ���Ǳ׸ʿ��� �� ����Ʈ�� ����ϵ��� ���ø��� ���� ������ ���ڽ��ϴ�.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  {{- with .Values.favorite }}
  drink: {{ .drink | default "tea" | quote }}
  food: {{ .food | upper | quote }}
  {{- end }}
  toppings: |-
    {{- range .Values.pizzaToppings }}
    - {{ . | title | quote }}
    {{- end }}

```

`toppings` ����Ʈ�� ������ ���� �ڼ��� ���캸�ڽ��ϴ�. `range` �Լ��� `pizzaToppings` ����Ʈ�� ��ȸ�� ���Դϴ�. �׷��� ���� ��մ� ����� �� �� �ִµ�, ��� ����غ��Ҵ� `with` �� `.`�� scope�μ� ���Ӱ� �����ߴ� ��ó��, `range` ���۷����� ���� `.`�� ���Ӱ� �����Ѵٴ� ���Դϴ�. ������ ��ȸ�� ������, `.`�� ���� ���� ������ ������ �����˴ϴ�. ��, ù ��° ��ȸ���� `.`�� `mushrooms` �� �����˴ϴ�. �� ��° ��ȸ������ `cheese` �� �����˴ϴ�,

`.` �� �������������� ���� ������ ����� ���� �ֽ��ϴ�. `{{ . | title | quote }}` ó�� ����ϸ� `.` �� `title` �� ������ (title case��� �Լ��Դϴ�), �׸��� �ٽ� `quota` �Լ��� �����ϴ�. �� ���ø��� �������ϸ� �Ʒ��� ���� ����� �� �� ���� ���Դϴ�.

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: edgy-dragonfly-configmap
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "PIZZA"
  toppings: |-
    - "Mushrooms"
    - "Cheese"
    - "Peppers"
    - "Onions"
```

��, ���� �� �������� �ణ tricky�� ����� ����߽��ϴ�. `toppings: |-` ������ ���� ���� �����ϴ� ���ڿ��� �ǹ��մϴ�. ���� ���� ����Ʈ�� ���� ����� ���� YAML ����Ʈ�� �ƴմϴ�. �̴� ���� ū �Ϸ��� ���ڿ��� ���Դϴ�. �� �̷��� ����������? ���Ǳ׸��� `data`�� Ű-���� ���� �̷���� �ִµ�, �� Ű�� ���� ���ڿ� ���� ������ �����Դϴ�. �̰��� ������ �ǹ��ϴ��� �ñ��ϴٸ� [�����Ƽ���� ���Ǳ׸� ���� ����](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)�� �����ϱ� �ٶ��ϴ�. ���� ���� �츮���� �־�� �̷��� ������ ���� ũ�� �߿������� �ʽ��ϴ�.

> `|-` �� YAML ���Ͽ��� ���� ���� ���ڿ��� ������ �� ����մϴ�. �̴� ���� ���ÿ��� �����ߴ� ��ó�� ���� ���ڿ��� �̷���� ū ���� YAML ���Ͽ��� ������ �� �ſ� �����մϴ�.

���δ� ���ø� �ȿ��� ���� ���ڿ��� ����� �̸� ��ȸ�ϴ� ���� �� ���Ҽ��� �ֽ��ϴ�. Helm ���ø��� `list` ��� �Ҹ��� �Լ��� �����մϴ�.

```yaml
  sizes: |-
    {{- range list "small" "medium" "large" }}
    - {{ . }}
    {{- end }}
```

���� ���ø��� �Ʒ��� ���� �������� ���Դϴ�.

```yaml
  sizes: |-
    - small
    - medium
    - large
```

����Ʈ�Ӹ� �ƴ϶�, `range` �� ���̳� ��ųʸ�ó�� Ű�� ���� ������ �÷����� ��ȸ�� �� �ſ� �����ϰ� ���� �� �ֽ��ϴ�. ���� ���̵� é�Ϳ����� �̸� ���ø� ������ ������ �� ��� ����� �� �ִ����� ���ؼ� ���캸�ڽ��ϴ�.



�� ������ ���� �������͸� Ŀ�� : [b2eed76](https://github.com/helm/helm/commit/b2eed7644ae5bab07925fd1bcf8e42132e769901) (on 5 Jun 2019)
