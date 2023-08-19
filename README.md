# 使用West管理自定义板型

在你的项目的`west.yml`里，按照[West文档](https://docs.zephyrproject.org/latest/develop/west/manifest.html)等描述，增加remotes源和projects库。

注意：
1. boards目录的结构需要是固定的，即`boards/<arch>/<board_name>`。
2. child_image和configuration目录只在NCS SDK及其附带的Zephyr环境生效，普通的Zephyr环境只有boards可以识别，MCUboot相关配置仍然需要手动配置。

例如：
```diff
diff --git a/west.yml b/west.yml
index 6b80bb0df..4608ee114 100644
--- a/west.yml
+++ b/west.yml
@@ -37,6 +37,8 @@ manifest:
       url-base: https://github.com/ant-nrfconnect
     - name: babblesim
       url-base: https://github.com/BabbleSim
+    - name: ihidchaos
+      url-base: https://github.com/ihidchaos

   # If not otherwise specified, the projects below should be obtained
   # from the ncs remote.
@@ -237,6 +239,11 @@ manifest:
       revision: 908ffde6298a937c6549dbfa843a62caab26bfc5
       import:
         path-prefix: tools
+    - name: gyc-boards
+      repo-path: gyc-boards
+      revision: main
+      path: gyc-boards
+      remote: ihidchaos


   # West-related configuration for the nrf repository.
```

然后在拉取仓库以后执行`west update`即可。

NCS会自动识别到你的版型、MCUboot配置、静态分区等，不需要在CMakeLists.txt中编写任何代码。