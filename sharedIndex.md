## Shared Index 测试步骤

### 前提条件：请提前关闭需要执行的IntelliJ IDEA进程

#### IDEA版本：2024.1.6

#### Shared-index-cli 版本：0.9.9

1. 下载 shared-index-cli 工具
   from [here](https://packages.jetbrains.team/maven/p/ij/intellij-shared-indexes/com/jetbrains/intellij/indexing/shared/ij-shared-indexes-tool-cli/?_gl=1*14ueamh*_gcl_aw*R0NMLjE3MjY2NDI5ODkuQ2owS0NRanc5S20zQmhEakFSSXNBR1ViNG56bk9lZ21idHlDblU5NXpUaUJsamhiMkR4emhhLWl2LVZ6WGZ3bnBhMWFHanVZaEtJSVdBY2FBa3pzRUFMd193Y0I.*_gcl_au*MzE5OTc4NjQ1LjE3MjIzNDgxNzc.*_ga*NDc3ODgxMjAzLjE2NDc0MTcyMjI.*_ga_9J976DJZ68*MTcyOTQwNDc1Ni45ODEuMS4xNzI5NDA0OTQwLjI3LjAuMA..).
2. 利用 cli 工具的 boost testing 对比使用共享前后的索引时间:

```shell
/Users/ethan/Downloads/ij-shared-indexes-tool-cli-0.9.9/bin/ij-shared-indexes-tool-cli boost --ij "/Users/ethan/JetBrainsTools/IntelliJ IDEA Ultimate.app" --project "/Users/ethan/IdeaProjects/spring-ms-demo"
```

3. 生成项目的共享索引命令行:

```shell
/Users/ethan/Downloads/ij-shared-indexes-tool-cli-0.9.9/bin/ij-shared-indexes-tool-cli indexes --ij "/Users/ethan/JetBrainsTools/IntelliJ IDEA Ultimate.app" --project "/Users/ethan/IdeaProjects/spring-ms-demo" --base-url "http://127.0.0.1:8080" --data-directory "/Users/ethan/Downloads/ij-shared-indexes-tool-cli-0.9.9/output"
```

4. 新建一个 `intellij.yaml` 配置文件放到项目的根目录下:

```yaml

sharedIndex:
  project:
    - url: http://127.0.0.1:8080/project/spring-ms-demo
  consents:
    - kind: project
      decision: allowed

```

5. 拷贝生成好的共享索引数据，至文件服务器（测试用的目录为cli工具目录下的output文件夹）

6. 用命令行启动本地测试用服务

```shell
/Users/ethan/Downloads/ij-shared-indexes-tool-cli-0.9.9/bin/ij-shared-indexes-tool-cli server --port 8080 --server-directory "/Users/ethan/Downloads/ij-shared-indexes-tool-cli-0.9.9/server"
```

7. 清除缓存和索引，重启IDE（File -> Invalidate Caches / Restart）