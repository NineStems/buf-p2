### Удаленная генерация c ветки
#### Команда
```shell
cd branch && buf generate && cd ..
```
#### buf.yaml
```yaml
version: v2
deps:
  - buf.build/grpc-ecosystem/grpc-gateway
  - buf.build/googleapis/googleapis
inputs:
  - git_repo: https://github.com/NineStems/buf-p1
    branch: master
    subdir: source/0_generate/proto # Директория в репозитории 
    #paths:
    #  - source/0_generate/proto # Эквивалентно записи выще 
    depth: 30
lint:
  use:
    - STANDARD
breaking:
  use:
    - FILE
```

#### buf.gen.yaml
```yaml
version: v2
clean: true
managed:
  enabled: true
  disable:
    - module: buf.build/googleapis/googleapis
  override:
    - file_option: go_package_prefix
      value: buf-p2/internal/controller/grpc/proto # Меняем prefix пакета на нужный нам
plugins:
  - local: protoc-gen-go
    out: internal/controller/grpc/proto 
    opt: paths=source_relative
  - local: protoc-gen-go-grpc
    out: internal/controller/grpc/proto
    opt: paths=source_relative
inputs:
  - git_repo: https://github.com/NineStems/buf-p1
    branch: master
    subdir: source/0_generate/proto # Директория в репозитории 
    #paths:
    #  - acme/manager/v1 # Конректные пути в указанной директории, можно использовать отдельно от subdir
    depth: 30
```
### Удаленная генерация на tag v1.0.0
```shell
cd tag1 && buf generate && cd ..
```

### Удаленная генерация на tag v1.0.1
```shell
cd tag2 && buf generate && cd ..
```

### Удаленная генерация на tag v1.0.0 через dep
```shell
cd directdep && buf generate && cd ..
```
Обеспечивается с помощью в `buf.yaml`
```yaml
deps:
  - git://github.com/NineStems/buf-p1?tag=v1.0.0#subdir=source/0_generate/proto
```