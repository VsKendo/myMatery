# 这是什么

这是我的hexo博客工作目录。我的hexo采用了matery主题，但是未来可能会对主题进行一定的编辑（比如Matery主题升级工作），所以开一个仓库。

至于https://github.com/VsKendo/vskendo.github.io ，是这个工作目录自动推上去的，并不需要人工维护。

# 如何使用

1. 克隆仓库`git clone`
2. `npm install`
3. 进行一些修改
4. `hexo d`

# 修改了什么

- 按照官方文档，添加了分类 categories 页、标签 tags 页、关于我 about 页、留言板 contact 页（目前被禁用）、友情链接 friends 页（可选的）、404 页。
- 根据官方文档，添加了以下插件（强烈建议安装，特别是代码高亮）：
  -  RSS 订阅支持
  - 文章字数统计
  - 中文链接转拼音
  - 代码高亮

- 修改了配置文件中的部分配置（如subtitle中的标语、删除了菜单中的留言板）

以及增加了一些文章和友链中的链接。

（以上修改都基于官方文档）

- 修复了一个bug，该bug导致在文章中，'{' 和 '}' 会被转义的问题（即使在代码块中也会）。修复方法是找到 `.\node_modules\hexo-prism-plugin\src\index.js`，然后增加2行，变为：

  ```javascript
  const map = {
    '&#39;': '\'',
    '&amp;': '&',
    '&gt;': '>',
    '&lt;': '<',
    '&quot;': '"',
    '&#123;': '{',
    '&#125;': '}'
  };
  ```

  `  '&#123;': '{'`和 `  '&#125;': '}'`是新增的，记得要在上一行加上逗号。

- 修改了 `bg-cover-content.ejs`，其硬编码了 `https://cdn.jsdelivr.net/npm/typed.js@2.0.11`，但是jsdelivr有可能出问题（如 `指向“https://cdn.jsdelivr.net/npm/typed.js@2.0.11”的 <script> 加载失败。` ），于是将原 script 改为

  ```javascript
  <script 
  	src="https://cdnjs.cloudflare.com/ajax/libs/typed.js/2.0.11/typed.min.js">
  </script>
  ```
  
  

# References

官方文档：https://github.com/blinkfox/hexo-theme-matery/blob/develop/README_CN.md

Hexo 主题 Matery 配置 ：https://www.cnblogs.com/mfrank/p/12830097.html

hexo上传博客代码时花括号被转义了：https://blog.csdn.net/weixin_47617631/article/details/121425697

jsdelivr.net挂了，前端如何动态的切换cdn呢？：https://segmentfault.com/q/1010000041149228/a-1020000041150172