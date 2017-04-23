##### 文件规范：
1. 文件命名全小写
2. 多个单词之间使用中横线连接
3. 文件编码统一使用UTF8
4. 文件头必须要有注释,标明文件用途、作者、版本等信息


##### css命名规范：
1. 类名全部为小写
2. 类名多个单词之间使用中横线连接


##### js语法规范:
1. 变量名/函数名统一使用小驼峰命名,可以为拼音或英文但不可混合使用,拼音应用全拼;
2. 构造函数或类的声明应使用大驼峰命名;
3. 请尽量语义化变量名/属性名/自定义标签名;
4. 变量或函数应有其适当注释, 拒绝无注释代码, 并提高函数重用率;
4. 无特别情况每行代码应尽量保持在100字符以内, 代码应有必要的缩进;
6. 花括号和关键词应位于一行,不应另起一行,如：
```js
   function user() {   //这里和function关键词位于一行
      ...
   };

   if(flag) {    //这里和if关键词同行
      ...
   };
```
7. 请使用var 来声明变量,且在连续声明变量时注意加上空格, 如: var user, name; //在user, 和 name 之间保持一个空格;
8. 私有变量以下划线打头如:var _classVarName;
9. 常量应全为大写;
10. 规范定义json对象,  key 和 value 都加上双引号, 如: {“name”:”jackson”};
11. 方法与方法之间应有一个空行;
12. 提交代码前应先拉取代码,另外代码提交应写好1-50字的描述;
13. 请注意语句结束或文件开头必须加上”;”号;
14. 函数应该是统一的返回类型, 即:一个函数中不能有多个不同的数据类型返回;
15. 函数命名应为动词+名词形式,部分常用命名参考常用固定命名;
16. 字符串统一使用’单引号’, 且不要在非赋值的地方出现中文;
17. 原则上可采用任意开发工具但不可如下使用:
> 使用IDE的视图模式”画”代码;
> 使用IDE的部分功能生成相关功能代码;


##### 参考文档
1. [Web Styleguide](https://github.com/gionkunz/chartist-js/blob/develop/CODINGSTYLE.md)
2. [Web前端开发规范文档](http://blog.csdn.net/gebitan505/article/details/39498115)
3. [前端开发规范文档](http://blog.csdn.net/gaofei880219/article/details/46924293)


##### 附录1 - 常用固定命名(XXXX可替换为名词)
> initXXXX	初始化
> queryXXXX 	查询
> getXXXX 	获取
> updateXXXX 	更新
> deleteXXXX 	删除
> saveXXXX 	保存
> importXXXX 	引入
> exportXXXX 	导出
> refreshXXXX	刷新
> startXXXX 	开始
> encodeXXXX	编码
> decodeXXXX	解码
> restoreXXXX	恢复
> resetXXXX	重置	resetForm/重置表单
> sendXXXX	发送
> calcXXXX

##### 附录2 - 常用固定动词
> query 查询/update 更新/delete 删除
> get 获取/set 设置/add 增加/remove 删除/create 创建/destory 移除
> start 启动/stop 停止/open 打开/close 关闭/read 读取/write 写入
> load 载入/save 保存/create 创建/destroy 销毁
> begin 开始/end 结束/backup 备份/restore 恢复/detach 脱离
> import 导入/export 导出/split 分割/merge 合并/inject 注入/extract 提取
> attach 附着/bind 绑定/separate 分离/ view 查看/browse 浏览
> edit 编辑/modify 修改/select 选取/mark 标记/copy 复制/paste 粘贴/undo 撤销
> redo 重做/insert 插入/delete 移除/add 加入/append 添加
> clean 清理/clear 清除/index 索引/sort 排序/find 查找/search 搜索/
> increase 增加/decrease 减少/play 播放/pause 暂停/
> launch 启动/run 运行/pack 打包/unpack 解包/parse 解析/emit 生成
> compile 编译/execute 执行/debug 调试/trace 跟踪
> observe 观察/listen 监听/build 构建/publish 发布/push 推/pull 拉
> input 输入/output 输出/encode 编码/decode 解码
> expand 展开/collapse 折叠/encrypt 加密/decrypt 解密
> compress 压缩/decompress 解压缩
> connect 连接/disconnect 断开/send 发送/receive 接收
> download 下载/upload 上传/refresh 刷新/synchronize 同步
> update 更新/revert 复原/lock 锁定/unlock 解锁
> check out 签出/check in 签入/submit 提交/commit 交付
> begin 起始/end 结束/start 开始/finish 完成/enter 进入/exit 退出
> abort 放弃/quit 离开/obsolete 废弃/depreciate 废旧
> collect 收集/aggregate

[欢迎访问我的guthub首页,点击访问](https://github.com/MisterChangRay)
