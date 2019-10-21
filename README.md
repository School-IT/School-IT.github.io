# 汕头侨中School IT 文档中心  

## 本地启动  

1. 先下载项目到本地
    ``` bash
    git clone https://github.com/School-IT/School-IT.github.io.git

    cd School-IT.github.io/

    npm install

    npm run server
    ```
2. 进入项目根目录
    ``` bash
    cd School-IT.github.io/
    ```
3. 安装 npm 依赖（需要 `Nodejs` 环境）
    ``` bash
    npm install
4. 启动 Server
    ``` bash
    npm run server
    ```
5. Server启动成功后，访问网址 [http://localhost:5000](http://localhost:5000) 。

## 如何增加文档？  

1. 在 `docs` 文件夹内新建 `Markdown` 文档，图片放在 `images` 文件夹内；
2. 在 `docs/SUMMARY.md` 文件里新增导航；
3. 提交 `PR` ；

## 在线查看  
[https://school-it.github.io/](https://school-it.github.io/)