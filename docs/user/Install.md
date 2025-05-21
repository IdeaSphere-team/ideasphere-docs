# 安装IdeaSphere

## Ubuntu/Debian

### 安装 Python3.13 和 pip

执行以下命令，添加Python 3.13的PPA仓库并安装Python 3.13：
```bash
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.13
```
然后，安装pip：
```bash
sudo apt install python3-pip
```

### 安装 Nginx

执行以下命令，安装nginx：
```bash
sudo apt update
sudo apt install nginx
```

### 配置并重启 Nginx

以下是推荐的nginx配置内容，将以下内容保存到`/etc/nginx/sites-available/ideasphere`：
```nginx
server {
    listen 80;
    server_name your_server_ip;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
替换`your_server_ip`为你的服务器IP地址，然后运行以下命令，使配置生效：
```bash
sudo ln -s /etc/nginx/sites-available/ideasphere /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

### 创建并使用 Python 虚拟环境

执行以下命令，创建并激活Python虚拟环境：
```bash
python3.13 -m venv myenv
source myenv/bin/activate
```

### pip 安装依赖

执行以下命令，拉取IdeaSphere源代码并安装依赖：
```bash
git clone https://github.com/IdeaSphere-team/IdeaSphere.git
cd IdeaSphere
pip install -r requirements.txt
```

### 启动 IdeaSphere

执行以下命令，启动IdeaSphere：
```bash
python app.py
```
如果你想让IdeaSphere在后台运行，可以使用`nohup`命令：
```bash
nohup python app.py &
```

### 关闭 IdeaSphere

如果你是通过终端直接启动IdeaSphere（如使用`python app.py`命令），关闭程序的方法取决于你如何启动它。以下是一些常见的情况：

- **在前台运行**：如果IdeaSphere正在终端的前台运行，你可以通过按下`Ctrl + C`来中断并停止程序。

- **在后台运行（使用`&`）**：如果IdeaSphere是在后台启动的（例如使用`python app.py &`），你可以通过以下步骤关闭它：
  1. 查找IdeaSphere进程：
     ```bash
     ps aux | grep app.py
     ```
     这将显示与`app.py`相关的进程信息，包括进程ID（PID）。

  2. 杀死进程：
     使用查到的PID来杀死进程。例如，如果PID是1234，你可以运行：
     ```bash
     kill 1234
     ```

- **使用`nohup`或`screen`/`tmux`**：如果使用了`nohup`或在`screen`/`tmux`会话中运行IdeaSphere，你可以先找到对应的进程，然后用`kill`命令关闭它，或者如果使用`screen`/`tmux`，可以重新附加上会话，然后在会话内使用`Ctrl + C`来停止程序。

- **使用进程管理工具**：如果你使用了如`systemd`等进程管理工具来管理IdeaSphere服务，你可以使用相应的命令来停止服务，例如：
   ```bash
   sudo systemctl stop ideasphere
   ```
   （前提是已经为IdeaSphere创建了相应的`systemd`服务文件并启用了服务。）

## Windows

### 安装 Python 3.13

  1. 访问 Python 官方网站：<https://www.python.org/> ，在首页找到并点击 “Downloads” 选项，在下拉菜单中选择 “Python 3.13” 进行下载（选择与您系统架构匹配的版本，通常是 Windows 版本）。
  2. 下载完成后，运行安装程序。在安装过程中，建议勾选 “Add Python 3.13 to PATH” 选项，这样可以方便在命令行中直接使用 Python 命令 。然后一直点击 “Next”，按照默认设置完成安装。

### 获取 IdeaSphere

访问[IdeaSphere 仓库地址](https://github.com/IdeaSphere-team/IdeaSphere)，点击 “Code” 按钮，选择 “Download ZIP” 将代码下载到本地。下载完成后，解压该 ZIP 文件到您希望存放项目的工作目录。

#### （可选）创建并使用 Python 虚拟环境

  1. 打开命令提示符（Win + R，输入 “cmd”，回车）。
  2. 切换到 IdeaSphere 项目目录，例如项目解压在 D 盘的 IdeaSphere 文件夹，输入命令：

     * `D:` 回车切换到 D 盘
     * `cd IdeaSphere` 回车进入项目目录

  3. 创建虚拟环境，输入命令：`python -m venv myenv`（myenv 是虚拟环境名称，可根据个人喜好修改）。
  4. 激活虚拟环境，输入命令：`myenv\Scripts\activate`，此时命令提示符前会显示虚拟环境名称（myenv）。

### pip 安装依赖

  1. 在命令提示符中（如果激活了虚拟环境，确保在虚拟环境下），确保当前目录是 IdeaSphere 项目目录。
  2. 输入命令：`pip install -r requirements.txt`，该命令会根据 requirements.txt 文件中的内容，自动安装 IdeaSphere 所需的 Python 依赖库。等待安装完成，期间可能会显示一些安装进度信息。

### 启动和关闭 IdeaSphere

#### 启动

  1. 在命令提示符中（虚拟环境可激活也可不激活，根据您前面的操作情况），进入 IdeaSphere 项目目录。
  2. 输入命令：`python app.py`，程序开始运行，IdeaSphere 论坛将在本地启动，默认一般会运行在 <http://127.0.0.1:5000/> 地址，您可以在浏览器中访问该地址查看论坛界面。

#### 关闭

  1. 在命令提示符中，按 “Ctrl + C” 组合键，可以终止正在运行的 IdeaSphere 程序，从而关闭论坛服务。

### 生产环境配置：安装 Nginx 并配置

#### 安装 Nginx

  1. 访问 Nginx 官方网站：<https://nginx.org/en/download.html>，在 Windows 下载部分选择适合您系统的版本进行下载。
  2. 下载完成后，运行安装程序，按照安装向导提示完成安装，默认设置一般即可满足需求。

#### 配置 Nginx

  1. 找到 Nginx 的安装目录，通常默认安装在 C:\nginx 或其他自定义目录。
  2. 打开 conf 文件夹下的 nginx.conf 文件，使用文本编辑器（如记事本）进行编辑。
  3. 推荐的 Nginx 配置内容如下（以反代 IdeaSphere 运行在本地 5000 端口为例）：

```nginx
server {
    listen 80;
    server_name your_server_ip; # 可根据实际需求修改为您的域名或服务器 IP

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

  4. 保存修改后的 nginx.conf 文件。
  5. 打开命令提示符，进入 Nginx 安装目录下的 bin 文件夹，输入命令：`nginx -s reload` 重启 Nginx，使新的配置生效。现在，通过访问服务器的 IP 或域名（如果配置了域名解析），就可以由 Nginx 转发请求到本地运行的 IdeaSphere 论坛服务了。