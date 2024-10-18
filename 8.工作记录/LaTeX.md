# LaTeX文件扩展格式
![[Pasted image 20241012165939.png]]

## 编译测试
最后介绍一个实用的工具宏包 syntonly。加载这个宏包后，在导言区使用 \syntaxonly 命 令，可令 LATEX 编译后不生成 DVI 或者 PDF 文档，只排查错误，编译速度会快不少
```LaTeX
\usepackage{syntonly}
\syntaxonly
```
如果想生成文档，则用 % 注释掉 \syntaxonly 命令即可。

本表格也再一次说明，使用 ==xelatex== 命令是我们最推荐的方式。
![[Pasted image 20241014142831.png]]



linux安装

``` shell
sudo mkdir /mnt/texlive

sudo mount /mnt/d/texlive2024.iso /mnt/texlive

sudo /mnt/texlive/install-tl     输入【I】
```

修改 `~/.bashrc` 文件，在文件末尾添加
```bash
# Add TeX Live to the PATH, MANPATH, INFOPATH

export PATH=/usr/local/texlive/2024/bin/x86_64-linux:$PATH

export MANPATH=/usr/local/texlive/2024/texmf-dist/doc/man:$MANPATH

export INFOPATH=/usr/local/texlive/2024/texmf-dist/doc/info:$INFOPATH
```
保存退出，重启 bash 或者执行命令 `source ~/.bashrc` 来重载 bash 配置，然后执行

```bash
tex -v
```
或者
``` shell
which latex
```
如果正常输出了版本信息，则说明安装成功。


默认配置
``` shell
 set directories:
   TEXDIR (the main TeX directory):/usr/local/texlive/2024
   
   TEXMFLOCAL (directory for site-wide local files):/usr/local/texlive/texmf-local
   
   TEXMFSYSVAR (directory for variable and automatically generated data):/usr/local/texlive/2024/texmf-var
   
   TEXMFSYSCONFIG (directory for local config):/usr/local/texlive/2024/texmf-config
   
   TEXMFVAR (personal directory for variable and automatically generated data):~/.texlive2024/texmf-var
   
   TEXMFCONFIG (personal directory for local config):~/.texlive2024/texmf-config
   
   TEXMFHOME (directory for user-specific files):~/texmf
```


更新字体缓存
```shell
fc-cache -fv
```