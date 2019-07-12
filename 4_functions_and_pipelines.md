# ���ø� �Լ��� ����������

���ݱ��� ���ø��� ��� �����͸� �Է��ϴ����� ���ؼ� �˾ƺ��ҽ��ϴ�. �׷��� �����ʹ� ���ø� ���ο� �������� ���� ä�� ���޵���. �Ƹ� �������� �����͸� Ư�� ������� ������ �� �� ����ϱ� ������ ����� ���� ���Դϴ�.

���ø� �ۼ��� ���� ���� (Best Practice) �ϳ��� ���÷μ� ������ ������ ���ڽ��ϴ�. `.Values` ������Ʈ���� ���ø����� ���� ������ ��, ���ڿ��� ����ǥ ó�� �ϴ� ���� �����ϴ�. ����ǥ ó���� ���ø� ������ �� `quota` ��� �Լ��� ���ؼ� ������ ó���� �� �ֽ��ϴ�.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ quote .Values.favorite.drink }}
  food: {{ quote .Values.favorite.food }}
```

���ø� �Լ��� `�Լ��̸� ����1 ����2...` �� ���� ������ ������ �����ϴ�. ���� ���ÿ����� `quote .Values.favorite.drink` �� ���� ������� ����ߴµ�, �̴� `quote` �Լ��� �Ķ���� �ϳ��� ������ ���Դϴ�.

Helm�� �뷫 60�� �̻��� �Լ��� �����մϴ�. �׷��� �Լ��� �� �Ϻδ� [Go template language](https://godoc.org/text/template) ��ü�� ���� ���ǵǴ� �͵� ����. ��� �Լ��� [Sprig template library](https://godoc.org/github.com/Masterminds/sprig) �� �Ϻ��� �͵� �ֽ��ϴ�. �̷��� �͵鿡 ���ؼ��� ������ ��� �ٷﺸ�� ��������� �ϰڽ��ϴ�.

> ������ "Helm ���ø� ���" �� Helm������ ����ϴ� ���� Ư���� ��ó�� ������ ���� ���� ���Դϴ�. �׷����� ��� Helm ���ø� ���� 1. Go ���ø� ���, 2. ���� �Լ���, 3. �׸��� ������Ʈ ������ ���ø��� �����ϱ� ���� �پ��� Wrapper��� �����Ǿ� �ֽ��ϴ�. GO ���ø����� ��� ������ �پ��� ���ҽ����� �˾Ƶθ� Helm ���ø� �� ���� ���� ������ ������ �𸨴ϴ�.

## ����������

���ø� ����� ���� ������ ��� �� �ϳ��� _pipelines_ �� �̿��� ���������� �Դϴ�. ������������ ���� ���� ���ø� ��ɾ �Ϸ��� ������ ���·� �ܼ��ϰ� ǥ���ϱ� ���� ���� ������� �����ϸ� �˴ϴ�. ���� �������ڸ�, ������������ ���� ���� ������ ������� ǥ���� �� �ִ� ȿ������ ����̶�� �����ϸ� �˴ϴ�. ������ �ٷ�� ���ø� ������������ ���� �� �� �����ϰ� �ٽ� �ۼ��غ��ڽ��ϴ�.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink | quote }}
  food: {{ .Values.favorite.food | quote }}
```

�� ���ÿ����� `quote ����` ó�� ����ϴ� ���, {{ }} ������ ������ ���� �ٲ���ϴ�. �� ���ÿ����� �Լ��� ���ڸ� ���������� (`|`) �� ���� �����߽��ϴ�. ������������ ����ϸ� ���� ���� �Լ��� ���޾� ����� ���� �ֽ��ϴ�.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink | quote }}
  food: {{ .Values.favorite.food | upper | quote }}
```

> {{ }} ������ ������ ������ ���� ���ø������� ���� ���Դϴ�. ������ �������� `quote .val` ���ٴ� `.val | quote` �� �� �� ���� ���� �� ���Դϴ�. �׷����� ���� ������ ������ �ִ� ���� �ƴϸ�, ��� ���� ����ϴ��� ����� �����ϴ�.

���� ���ø� ���� ������ ��ȯ�Ǹ� �Ʒ��� ���� ���ø��� �������˴ϴ�.

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: trendsetting-p-configmap
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "PIZZA"
```

`pizza` ��� ���� `"PIZZA"` �� �����Ǿ����ϴ�!

���� ���� ���ڵ��� ���������ο� ���� ��, ù ��° ���� ó���� ��� (`.Values.favorite.drink`) ���� _�Լ��� ������ ����_�μ� ���޵˴ϴ�. �̸� �� �� ���������� �����ϱ� ����, ���� ���ÿ��� {{ }} ������ ���� �� ���� ���ڸ� ������ �� �ֵ��� ���� �ٲ㺸�ڽ��ϴ�. �Ʒ�ó�� `repeat COUNT STRING` �� ����� ���ڽ��ϴ�.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-configmap
data:
  myvalue: "Hello World"
  drink: {{ .Values.favorite.drink | repeat 5 | quote }}
  food: {{ .Values.favorite.food | upper | quote }}
```

`repeat` �Լ��� �־��� ���� ���� N�� ��ŭ ��� (echo) �մϴ�. ��, ���������δ� �Ʒ��� ���� �������˴ϴ�. 

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: melting-porcup-configmap
data:
  myvalue: "Hello World"
  drink: "coffeecoffeecoffeecoffeecoffee"
  food: "PIZZA"
```

## `default` �Լ� ����ϱ�

���ø����� �� ���� ���Ǵ� �Լ��� `default` �Դϴ�. �̴� `default [�⺻ ��] [�־��� ��]` ���·� ���˴ϴ�. �� �Լ��� ���ø� ���ο��� ���� �����Ǿ��� �� �⺻ ���� �����ϴ� ����� �����մϴ�. `default` �Լ��� �̿��� ������ ����� ���ø� �� �� �ٲ㺸���� ����.

```yaml
drink: {{ .Values.favorite.drink | default "tea" | quote }}
```

����ó�� �׳� ���ø��� �������� ����, �Ʒ��� ���� ���ø��� ��µ˴ϴ�.

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: virtuous-mink-configmap
data:
  myvalue: "Hello World"
  drink: "coffee"
  food: "PIZZA"
```

��, �׷��ٸ� `values.yaml` ���Ͽ��� drint ��� ���� ������ ���ڽ��ϴ�.

```yaml
favorite:
  #drink: coffee
  food: pizza
```

�ٽ� `helm install --dry-run --debug ./mychart` ��ɾ ������ ���� �Ʒ��� ���� �������˴ϴ�.

```yaml
# Source: mychart/templates/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: fair-worm-configmap
data:
  myvalue: "Hello World"
  drink: "tea"
  food: "PIZZA"
```

���� ��Ʈ�� ����� ���� ��� ���� ������ values.yaml ���� ���ο� ���ǵǾ� �־�� �մϴ�. �׷���, `default` ��ɾ�� values.yaml ���� ���ο��� ����� �� ����, ���� ��� ���� ���μ��̵� ���鿡 ���ؼ��� ����ϱ⿡ ��������. �Ʒ��� ����ó�� ���Դϴ�.

```yaml
drink: {{ .Values.favorite.drink | default (printf "%s-tea" (include "fullname" .)) }}
```

��� ��쿡�� `if` ���ǹ��� `default` ���� �� ȿ������ ���� �ֽ��ϴ�. �̿� ���ؼ��� ���� �忡�� �˾ƺ����� ����.

## ���۷����� (Operator) �� �Լ�

���۷����ʹ� �Ҹ� ���� ��ȯ�ϴ� �Լ��μ� �����Ǿ� �ֽ��ϴ�. `eq`, `ne`, `lt`, `gt`, `and`, `or`, `not` �� ���� �����ڸ� ����ϱ� ���ؼ��� �Լ��� ����ϴ� �Ͱ� ���������� ���� ���� �Ķ���� �տ� ���۷����� �����ڸ� ����ϸ� �˴ϴ�. ���� ���� ���۷����͸� ���޾Ƽ� ����Ϸ��� ��ȣ�� ����� ��������� �� ���۷����͸� �����ؾ� �մϴ�.

```yaml
{{/* .Values.fooString�� ���� �����ϰ�, "foo" ��� ������ �����Ǿ��� �� ������ ������ ���Խ�ŵ�ϴ�. */}}
{{ /* ���� : and �Լ��� ���ڰ� .Values.fooString�� (eq .Values.fooString "foo") ��� �����ϸ� �˴ϴ�. */}}
{{ /* �׸��� �� ������� if�� ���޵˴ϴ�. */}}
{{ if and .Values.fooString (eq .Values.fooString "foo") }}
    {{ ... }}
{{ end }}
```

```
{{/* �������� ���� ���� false�� ��ȯ�Ǹ�, .Values.setVariable�� not �Լ��� ���� �����Ǿ����Ƿ� (true -> false) ������ ������ ���Ե��� �ʽ��ϴ�. */}}
{{ if or .Values.anUnsetVariable (not .Values.aSetVariable) }}
   {{ ... }}
{{ end }}
```



�Լ��� ������������ ����� ��������, ���� �忡���� ���ǹ��� �ݺ���, �׸��� ���� ������ (scope modifiers) �� ���� ����� ���ڽ��ϴ�.

�� ������ ���� �������͸� Ŀ�� : [63c970c](https://github.com/helm/helm/commit/63c970c5ce29b0971cdc6409d9c0e156321ea32a) (on 28 Feb 2019)


