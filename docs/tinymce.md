# tinymce组件

目前直接使用[tinymce-vue](https://github.com/tinymce/tinymce-vue)，暂未二次封装。

## 1. 安装及引用

注：只安装tinymce-vue不可以，还需安装tinymce，否则会报错

```
npm install tinymce
npm install @tinymce/tinymce-vue

import tinymce from 'tinymce/tinymce'
import Editor from '@tinymce/tinymce-vue'
```
## 2. 初始化编辑器（记录出现的问题和解决方案）

- 按示例初始化发现编辑器不显示，报“`theme.js:1 Uncaught SyntaxError: Unexpected token <`”这个错。
 > 解决方案：手动引入tinymce主题，在init({})方法里加`theme: 'silver',`没用。
    ```
    import 'tinymce/themes/silver'
    ```

- 不报错了但是编辑器还是不显示，继续研究，发现还需要定义[皮肤](https://www.tiny.cloud/docs/advanced/usage-with-module-loaders/)，在init({})里加`skin: "oxide"`没用。
 > 解决方案：先在public目录下新建一个文件夹命名为tinymce，然后在node_modules里找到tinymce的skin包，复制到public/tinymce里，最后在初始化tinymce时的init里配置skin_url：

    ```
    <editor :init="{skin_url:'/tinymce/skins/ui/oxide'}"></editor>
    ```
  > *注：资源用绝对路径引入时，读取的是public文件夹里的资源。如果应用没有部署在域名的根部，那么需要为 URL 配置 publicPath前缀。*
  
    ```
    <editor :init="{skin_url:`${publicPath}tinymce/skins/ui/oxide`}"></editor>
    ...
    data () {
        return {
          publicPath: process.env.BASE_URL
        }
    }
    ```
    
## 3. 配置

- 一些常用的配置属性

    ```
      browser_spellcheck: true, // 拼写检查
      branding: false, // 去水印
      elementpath: false,  //禁用编辑器底部的状态栏
      statusbar: false, // 隐藏编辑器底部的状态栏
      paste_data_images: true, // 允许粘贴图像
      menubar: false, // 隐藏最上方menu
    ```

- plugins

    使用某个插件需要先引入这个插件，例：

    ```
    import 'tinymce/plugins/fullscreen'
    import 'tinymce/plugins/preview'
    
    plugins: 'fullscreen preview'
    ```
    
- toolbar

    可以使用“|”给工具栏分组，把某一类功能划分成一组，例：
    
    ```
    toolbar: 'bold italic underline | alignleft aligncenter alignright'
    ```

## 4. 定制

- 将语言改为中文

    步骤：
    
    1. 在官网下载语言包[https://www.tiny.cloud/get-tiny/language-packages/](https://www.tiny.cloud/get-tiny/language-packages/)
    
    2. 把下载的语言包放到之前新建的tinymce文件夹里
    
    3. 初始化时添加以下代码
    
    ```
          language_url: `/tinymce/langs/zh_CN.js`,
          language: 'zh_CN',
    ```

- 在tinymce5工具栏添加自定义功能按钮

    ```
    this.tinymceInit = {
    ...
    toolbar: 'imageUpload',
      setup: (editor) => {
        editor.ui.registry.addButton('imageUpload', {
          tooltip: '插入图片',
          icon: 'image',
          onAction: () => {
    
          }
        })
      }
    }
    ```