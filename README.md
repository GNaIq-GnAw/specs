## 编码规范

为保证团队编码风格统一，建议遵循vue官方推荐的风格指南，同时借助第三方工具（eiditorconfig eslint等）约束编码风格。

### template(html) 

* 必须有`<div>`根节点包裹。如果确实需要移除根节点，可使用[`vue-fragment`](https://www.npmjs.com/package/vue-fragment)。
* 尽量不要写行内样式，因为会导致样式混乱，统一写到`<style>`标签中。
* 定义组件或页面时，尽量保证根节点的`class`和页面的`name`一致。

    ```vue
    <template>
        <div class="ElementName"></div>
    </template>
    
    <script>
        export default {
            name: "ElementName",
        };
    </script>
    
    <style lang="scss" scoped>
        .ElementName {
            
        }
    </style>
    ```

*注*：vue3中已经去掉了根节点限制

### css

* 如无特殊要求，统一使用[`sass`](https://www.sass.hk/)作为预处理，并且使用`scss`模式。
* 命名格式遵循[`BEM`](https://en.bem.info/)规范。

 > block-name__element-name--modifier-name，也就是模块名 + 元素名 + 修饰器名。

    ```vue
    <template>
        <div class="page-name page-name--error">
            <div class="page-name__header page-name__header--warning">
                <!-- class命名不受层级影响 -->
                <div class="page-name__text"></div>
            </div>
            <div class="page-name__body">
                <div class="page-name__item page-name__item--info"></div>
            </div>
            <div class="page-name__footer"></div>
        </div>
    </template>
    
    <style lang="scss" scoped>
        .page-name {
            &--error {
                color: red;
            }
            &__header {
                &--warning {
                    color: #d27070;
                }
            }
            &__text {
        
            }
            &__body {
        
            }
            &__item {
                &--info {
                    color: #999999;
                }
            }
            &__footer {
        
            }
        }
    </style>
    ```

* 组件样式全部写入vue组件文件，目前框架中很多框架及的样式都是在独立的scss文件中，需要逐步转移至各组件中。对于全局使用的文件，可以保留在style目录中，或直接引入第三方库。比如：`Normalize.css`可以直接npm安装。

* 对于scss的全局变量，可以考虑使用[`sass-resources-loader`](https://www.npmjs.com/package/sass-resources-loader)插件，方便组件内使用scss变量。

### javascript

* 变量声明可以用`const`的不要用`let`。
* 变量命名要有意义，别瞎起，不要让ide报拼写错误。
* 变量名称全部驼峰命名。
* 私有变量、私有属性统一`_`前缀。
* 导入外部模块时，本模块下级使用`./`引用，非本模块下级使用`@/`引用。
* 严格遵守`ESLint`语法校验。
* 写判断时尽量使用`===`替代`==`。
* 每行代码结束加`;`。
* 代码该缩进缩进，不要写成一坨，按照4个空格缩进。
* 运算符两侧加空格。

    ```js
    // bad
    const x=y+5;
    
    // good
    const x = y + 5;
    
    // bad
    const isRight = result === 0? false: true;
    
    // good
    const isRight = result === 0 ? false : true;
    
    // bad - 一元运算符与操作对象间不应有空格
    const x = ! y;
    
    // good
    const x = !y;
    ```

### 注释 

* 基于`koroFileHeader`自动生成文件头部信息，必须填写`Description`一项。

  ```js
  /*
   * @Author: 作者
   * @Date: 2021-06-28 11:10:30
   * @LastEditTime: 2021-06-28 14:54:43
   * @LastEditors: 作者
   * @Description: 本地环境的配置文件
   * @FilePath: \xxxxxx\xxxxx\xxxxx.js
   * @CopyRight: xxxxxxx
   */
  ```

* 函数逻辑需用伪代码写清楚。

* 函数的传入参数及返回需写清楚。

* 各枚举类型的含义需写清楚。

* 网上搜到的解决方案请注释好来源地址。

* 对于一些存在歧义但必须按照某种方案去做的地方，记录好时间，需求方，及相关原因。

* 对于待解决问题，配合`TODO Highlight`插件，学会使用`TODO:` `FIXME:` `BUG:` 关键字。

  ```js
  //TODO: 需要做错误处理。
  //FIXME: 这里缺少空判断。
  //BUG: 请求的参数不对，稍后处理。
  ```


### 官方风格指南

vue官方的编码建议，分为ABCD四个等级，尽可能的按照此规范编码。

> 地址：https://cn.vuejs.org/v2/style-guide/

#### 组件命名及文件命名

**组件名应该始终是多个单词的。如：`export default {name: "todoItem", ...}` `{name: "news-detail"}`。**

当碰到单例组件名时，可前缀`the`。如：`the-list`。 或者统一约定组件前缀。比如`elementUI`统一前缀`el-`。

**组件文件名始终以大写字母开头。如：`TodoItem.vue` `NewsDetail.vue`**

**注意**：官方建议组件文件命名以大写字母开头，如：`TheDetail.vue`。目前我们的项目并没有遵循这一规则，需逐渐向规范靠拢：新创建组件，统一大写开头。鉴于windows和linux对大小写命名文件的处理不同，已存在的组件文件，名称不做调整。

#### 组件文件的存放规则

**只要有能够拼接文件的构建系统，就把每个组件单独分成文件。**

尽可能的把各模块的组件打散，每个小组件为一个vue文件，放在该模块目录下的`components`目录下。

```bash
components/
|- TodoList.vue
|- TodoItem.vue
```

若其它模块用到了此组件，可直接引用，可把此组件提升至全局组件的存放路径，也可在该模块下的`components`目录下创建同名vue文件继承一下目标组件。

### editorconfig

>  官方网站：https://editorconfig.org/

定义和维护一致的编码风格。

#### 使用方法

1. 安装editorconfig插件
   如果是vscode，则需要安装；如果是webStorm则可以忽略。

2. 在项目根目录创建`.editorconfig`文件并配置。

<details>
    <summary>具体配置可参考官方网站或以下配置</summary>

```editorconfig
#/.editorconfig
root=true

[*]
#字符集
charset=utf-8
#换行格式
end_of_line=lf
#缩进
indent_style=space
indent_size=4
#新行结束
insert_final_newline=true
#移除行尾多余空格
trim_trailing_whitespace=true

[*.md]
insert_final_newline=false
trim_trailing_whitespace=false
```

</details>

### eslint

> 官方网站：https://eslint.org/
>
> 中文指南：https://eslint.bootcss.com/

检查Javascript编程的语法错误。

vue-cli创建项目会询问是否开启及开启的模式。选定即可。

### 样式穿透 - 深度作用选择器

如果要改变组件内样式，需要用到样式穿透。

标准操作符为`>>>`，但部分预处理器无法正常解析，所以大多数都使用`>>>`的别名：`/deep/` 或者 `::v-deep`。

建议统一在父组件使用`::v-deep`模式。

## 常用库

### day.js

Day.js 是一个轻量的处理时间和日期的 JavaScript 库，和 Moment.js 的 API 设计保持完全一样.

> https://www.npmjs.com/package/dayjs
>
> https://github.com/iamkun/dayjs/blob/HEAD/docs/zh-cn/README.zh-CN.md

### lodash

Lodash 是一个一致性、模块化、高性能的 JavaScript 实用工具库。

> https://www.npmjs.com/package/lodash
>
> https://www.lodashjs.com/

### normalize.css

Normalize.css 是一个可以定制的CSS文件，它让不同的浏览器在渲染网页元素的时候形式更统一。

> https://www.npmjs.com/package/normalize.css
>
> http://necolas.github.io/normalize.css/

### remixicon

Remix Icon 是一套面向设计师和开发者的开源图标库。

> https://www.npmjs.com/package/remixicon
>
> https://github.com/Remix-Design/remixicon/blob/HEAD/README_CN.md

### elementUI

Element，一套为开发者、设计师和产品经理准备的基于 Vue 2.0 的桌面端组件库

> https://element.eleme.cn/#/zh-CN

### vant

轻量、可靠的移动端 Vue 组件库

> https://vant-contrib.gitee.io/vant/#/zh-CN/

