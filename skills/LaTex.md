# LaTex

## 论文引用

### 定义文献列表

```latex
\begin{thebibliography}{99}
\bibitem{cite1}Fernandez-Beltran R, Latorre-Carmona P, Pla F. Single-frame super-resolution in remote sensing: A practical overview[J]. International journal of remote sensing, 2017, 38(1): 314-354.
\bibitem{cite2}Irmak H, Akar G B, Yuksel S E. A map-based approach for hyperspectral imagery super-resolution[J]. IEEE Transactions on Image Processing, 2018, 27(6): 2942-2951.
\bibitem{cite3}Zhang Y, Duijster A, Scheunders P. A Bayesian restoration approach for hyperspectral images[J]. IEEE transactions on geoscience and remote sensing, 2012, 50(9): 3453-3462.
\end{thebibliography}
```

### 引用

1. 单引用

    ```latex
    high-resolution images \cite{cite1}.
    % 结果：high-resolution images [1].
    ```
    
2. 多引用

    ```latex
    high-resolution images \cite{cite1, cite2}.
    % 结果：high-resolution images [1, 2].
    ```

3. 连续引用使用破折号

    ```latex
    % 导入包
    \usepackage[numbers,sort&compress]{natbib}
    % 使用
    high-resolution images \cite{cite1, cite2, cite3}.
    % 结果：high-resolution images [1-3].
    ```

4. 标号显示在右上角

    ```latex
    % 在文档开始前加上下面的语句命令
    \newcommand{\upcite}[1]{\textsuperscript{\textsuperscript{\cite{#1}}}}
    % 使用
    high-resolution images \upcite{cite1, cite2, cite3}
    % 结果
    ```

## 安装

参考：[LaTex 论文排版(1): Win10 下 LaTex所需软件安装 (Tex live 2018 + Tex studio)_TechArtisan6的博客-CSDN博客_latex 安装](https://blog.csdn.net/zaishuiyifangxym/article/details/88170827)

## 公式

参考：[LaTeX 公式篇 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/110756681)



## 表格

参考：[LaTeX排版札记：part 5—表格和列表生成 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/34271941)

