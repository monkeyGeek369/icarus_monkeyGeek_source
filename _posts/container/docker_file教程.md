---
title: dockerFile教程
category: container
date: 2024-10-05 15:30:06
updated: 2024-10-05 15:30:06
enname: docker_file
categories: container
tags: docker
keywords:
	- docker
	- 容器
permalink:
thumbnail:
---



docker file基础指令介绍

</br>

# 基础指令

```python
# 使用easyrpa基础镜像
FROM easyrpa:0.0.1

# 设置工作目录，类似与cd命令
WORKDIR /app

# 复制项目文件到容器中
# /EasyRPA此路径是在docker build -f /path/to/Dockerfile -t myimage:latest .中的上下文路径上的
COPY /EasyRPA /app/EasyRPA
COPY /EasyRPA-Common /app/EasyRPA-Common

# 为了在后续的RUN指令中使用Conda环境，我们需要初始化Conda
RUN conda init bash

# 安装项目依赖
WORKDIR /app/EasyRPA-Common
RUN python setup.py bdist_wheel

# 激活Conda环境
WORKDIR /app/EasyRPA-Common/dist
# 使用SHELL指令将shell命令的解释器从默认的/bin/sh更改为指定的命令
SHELL ["conda", "run", "-n", "easyrpa", "/bin/bash", "-c"]
RUN pip install easyrpa-0.1.1-py3-none-any.whl

# 暴露容器的 5003 端口
EXPOSE 5003

# 运行 Flask 应用
WORKDIR /app/EasyRPA
CMD ["/bin/bash", "-c", "source activate easyrpa && python app.py"]
#CMD ["/bin/bash"]
```





</br>

</br>

</br>

<script>
var _hmt = _hmt || [];
(function() {
  var hm = document.createElement("script");
  hm.src = "https://hm.baidu.com/hm.js?2f798e6b269c8a40f12bef25d7f1876d";
  var s = document.getElementsByTagName("script")[0]; 
  s.parentNode.insertBefore(hm, s);
})();
</script>

<div style="height:260px; background-color:rgb(238,240,244); padding:10px;border-radius:10px;">
    <p style="color:#f36c21;font:bold 16px/20px 'kaiTi';">
      版权声明：本文为博主原创文章，欢迎转载，转载请注明作者、原文超链接，感谢各位看官!!!
    </p>
    <p>
      <span style="font:bold 16px/20px 'kaiTi';">本文出自：</span><a href="https://monkeyGeek369.github.io">monkeyGeek</a> 
    </p>
    <p>
      <span style="font:bold 16px/20px 'kaiTi';">座右铭：</span><span>生于忧患，死于安乐</span> 
    </p>
    <p>
      <span style="font:16px/20px 'kaiTi';">欢迎志同道合的朋友一起交流、探讨！</span> 
    </p>
    <img style="height:auto; width:auto;flot:left;" src="../../../../image/monkey64.png" /><span style="font:16px/20px 'kaiTi';flot:left;">   monkeyGeek</span>


