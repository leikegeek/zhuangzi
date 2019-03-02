## 环境搭建

###  构建虚拟运行环境（Windows系统）

- 第一步，下载 [Miniconda](https://conda.io/en/master/miniconda.html)  注意选择python版本和运行的计算机版本，注意在安装的过程中勾选`Add Anaconda to the system PATH environment variable` 选项

- 第二步，下载 本课程的样例代码 [D2L](https://zh.d2l.ai/d2l-zh-1.0.zip)

- 第三步，使用conda创建虚拟环境，这里使用国内的镜像站，使用如下命令。`注意讲上一步的代码解压到一个文件夹下，在此D2L目录下打开CMD执行一下命令

  ```shell
  # 配置清华conda镜像
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  # 配置清华PyPI镜像（如无法运行，将pip版本升级到>=10.0.0）
  pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
  # 接下来使用conda创建虚拟环境并安装本书需要的软件。这里environment.yml是放置在代码压缩包中的文件。使用文本编辑器打开该文件，即可查看运行压缩包中本书的代码所依赖的软件（如MXNet和d2lzh包）及版本号。
  # 这个命令需要执行很长时间中途会自动下载很多依赖包和插件
  conda env create -f environment.yml
  # 如果环境需要升级使用一下命令升级环境
  conda env update -f environment.yml
  ```

​     若使用国内镜像后出现安装错误，首先取消conda镜像配置，即复制已执行的conda镜像配置命令，将其中的`--add`改   为`--remove`再分别执行。然后取消PyPI镜像配置，即执行命令`pip config unset global.index-url`。最后重试命令`conda env create -fenvironment.yml`

- 第四步是激活之前创建的环境。激活该环境是能够运行本书的代码的前提。如需退出虚拟环境，可使用命令`conda deactivate`（若conda版本低于4.4，使用命令`deactivate`）。

  ```shell
  conda activate gluon  # 若conda版本低于4.4，使用命令activate gluon
  # 上面执行完成后可以看到命令行前已经带有(gluon)了
  ```

- 第五步是打开Jupyter 在构建虚拟环境的时候此软件已经自动安装，直接执行一下命令即可

  ```shell
  jupyter notebook
  # 上面执行完成后，这时在浏览器打开 http://localhost:8888 （通常会自动打开）就可以查看和运行本书中每一节的代码了
  # 另外 本书中若干章节的代码会自动下载数据集和预训练模型，并默认使用美国站点下载。我们可以在运行Jupyter前指定MXNet使用国内站点下载书中的数据和模型。
  set MXNET_GLUON_REPO=https://apache-mxnet.s3.cn-north-1.amazonaws.com.cn/ jupyter notebook
  ```

- 第六步，使用GPU版本的MXNet

  通过前面介绍的方式安装的MXNet只支持CPU计算。本书中部分章节需要或推荐使用GPU来运行。如果你的计算机上有NVIDIA显卡并安装了CUDA，建议使用GPU版的MXNet。

  第一步是卸载CPU版本MXNet。如果没有安装虚拟环境，可以跳过此步。如果已安装虚拟环境，需要先激活该环境，再卸载CPU版本的MXNet：

  ```shell
  pip uninstall mxnet
  ```

  然后退出虚拟环境。

  第二步是更新依赖为GPU版本的MXNet。使用文本编辑器打开本书的代码所在根目录下的文件`environment.yml`，将里面的字符串“mxnet”替换成对应的GPU版本。例如，如果计算机上装的是8.0版本的CUDA，将该文件中的字符串“mxnet”改为“mxnet-cu80”。如果计算机上安装了其他版本的CUDA（如7.5、9.0、9.2等），对该文件中的字符串“mxnet”做类似修改（如改为“mxnet-cu75”“mxnet-cu90”“mxnet-cu92”等）。保存文件后退出。

  第三步是更新虚拟环境，执行命令

  ```shell
  conda env update -f environment.yml
  ```

  之后，我们只需要再激活安装环境就可以使用GPU版的MXNet运行本书中的代码了。需要提醒的是，如果之后下载了新代码，那么还需要重复这3步操作以使用GPU版的MXNet。