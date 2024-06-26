## **<u>CFGC</u>**  构建自己本地的 PYPI

**快速构建项目的自定义python运行环境**

------

```
当我们进行某些任务时，他们可能有多种类似的脚本，输出类似的内容，比如一些pipeline。
这时候任务脚本间可能存在大量的重复函数/变量，而这些函数并非pypi提供的，可能是我们自己定义的
如果对这些内容在各个脚本间重复定义则太不灵活，而CFGC(config-center)库则是为此而生！
```

如：

- 测评一系列类似的方法 (测评、数据加载函数很大概率是一种)
- 多流程分支分析 (数据读取、某些变量如输出路径)
- 日常使用 (如获取时间间隔的API、机器学习常用的数据拆分策略)
- ...更多可能

------

## Install

```python
pip install config-center
```

## Usage

```python
# region |- Import -|
import cfgc
import datetime
import os
# endregion

# list all envs
cfgc.envs.view()

# region |- Create an Env -|
env = cfgc.center.Environment()
env.define(
    "cfgc-base",  # env name: test
    {"date": datetime.datetime.now().strftime('%Y-%m-%d')},  # env domain(data): test
    os.path.join(cfgc.center.PATH.center)  # env engine(site-package): cfgc self
    )
env.save(safe=True)
# Input: yes
# endregion

del env

# region |- Reload the Env -|
from pprint import pprint
load_env = cfgc.center.Environment(load="cfgc-base")     # env name: test
pprint(dir(load_env.engine))
# endregion

# region |- Export the Env -|
load_env.export("../export")
del load_env
# reload
load_env = cfgc.center.Environment()
load_env.load_path("../export")     # env name: export
pprint(dir(load_env.engine.center))
print(load_env.domain)
# endregion
```



------

**CFGC实现非常简单，仅仅是使用了部分路径定义、模块加载的知识即可完成**

**而它可以保证让你在本地构建一些简单的python包，而无需上传pypi下载，来达到随心import**

- 完全不要再为相对路径、绝对路径的导入发愁
- 数据目录 与 函数目录分离, 无论项目的路径怎么变化, CFGC都可以定位到你本地的函数库
- 简单的客制化

------

## 欢迎使用体验CFGC！