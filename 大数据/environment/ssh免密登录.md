**为什么要配置免密登录**

**可以在配置完第一台虚拟机后，直接将文件传输给其余两台虚拟机，提高开发效率**

**三步配置**

1、配置主机名

**执行：vim** **/etc/hosts**

编辑主机名

注意：主机名里不能有下滑线，或者特殊字符 #$，不然会找不到主机导致无法启动

这种方式更改主机名需要重启才能永久生效，因为主机名属于内核参数。

```text
192.168.11.131 hadoop01
192.168.11.132 hadoop02
192.168.11.133 hadoop03
```

![img](https://pic3.zhimg.com/80/v2-60f571565e0cc46862fd2ff018d8f996_720w.jpg)



2、在主机节点（hadoop01）执行

```text
ssh-keygen
```

然后三次回车

![img](https://pic3.zhimg.com/80/v2-b7827681856784ea4de9a867e8789c8e_720w.jpg)

生成节点的公钥和私钥，生成的文件会自动放在/root/.ssh目录下

![img](https://pic2.zhimg.com/80/v2-6dc52c6af424feb60b024d574d4d270d_720w.jpg)

3、然后把公钥发往3台远程机器1 2 3

```bash
ssh-copy-id root@hadoop01
ssh-copy-id root@hadoop02
ssh-copy-id root@hadoop03
```

