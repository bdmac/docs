Use `jobs.<job_id>.container` to create a container to run any steps in a job that don't already specify a container. スクリプトアクションとコンテナアクションの両方を使うステップがある場合、コンテナアクションは同じボリュームマウントを使用して、同じネットワーク上にある兄弟コンテナとして実行されます。

`container`を設定しない場合は、コンテナで実行されるよう設定されているアクションを参照しているステップを除くすべてのステップが、`runs-on`で指定したホストで直接実行されます。

### Example: Running a job within a container

```yaml{:copy}
name: CI
on:
  push:
    branches: [ main ]
jobs:
  container-test-job:
    runs-on: ubuntu-latest
    container:
      image: node:14.16
      env:
        NODE_ENV: development
      ports:
        - 80
      volumes:
        - my_docker_volume:/volume_mount
      options: --cpus 1
    steps:
      - name: Check for dockerenv file
        run: (ls /.dockerenv && echo Found dockerenv) || (echo No dockerenv)
```

コンテナイメージのみを指定する場合、`image`は省略できます。

```yaml
jobs:
  container-test-job:
    runs-on: ubuntu-latest
    container: node:14.16
```
