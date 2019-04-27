# Install For Mac OSX
```
pip install tensorflow
```

# Install TensorBoard
```
pip install tensorboard
```
tensorboard --logdir logs

# View Pip List
```
pip list // pip freeze
```

tensorboard==1.12.2
tensorflow==0.5.0


给指定用户对应目录赋权限
sudo chown -R username /usr/local/

sudo chown -R username /Users/xxxx/

sudo chown -R username /Library/Python

创建一个新的虚拟环境，方法是选择 Python 解释器并创建一个 ./venv 目录来存放它：
virtualenv --system-site-packages -p python3 ./venv

使用特定于 shell 的命令激活该虚拟环境：
 # sh, bash, ksh, or zsh
source ~/./venv/bin/activate

当 virtualenv 处于有效状态时，shell 提示符带有 (venv) 前缀。
在不影响主机系统设置的情况下，在虚拟环境中安装软件包。首先升级 pip：
pip install --upgrade pip
pip list

之后要退出 virtualenv，请使用以下命令：
deactivate

安装 TensorFlow pip 软件包
pip install --upgrade tensorflow

验证安装效果：
python -c "import tensorflow as tf; tf.enable_eager_execution(); print(tf.reduce_sum(tf.random_normal([1000, 1000])))"


