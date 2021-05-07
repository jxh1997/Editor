# miniprogram-richtext

#### 博客地址
[微信小程序之实现封装一个富文本编辑器 Editor 的完整流程](https://blog.csdn.net/XH_jing/article/details/115509316)

#### 开始之前
>使用此组件需要依赖小程序基础库 2.7.0 以上版本，低版本需做[兼容处理](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html)。
>更多组件问题，请阅读[官方文档。](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/)

#### 安装教程

1.  下载源码

```
git clone https://github.com/jxh1997/Editor.git
```


#### 使用说明

1.  在下载源码中找到组件目录：components/richText，将 richText 整个文件夹复制到你的项目中。
2.  在page.json中引入组件

```
"usingComponents": {
    "richText":"../../components/richText/richText"
},
```
3.  在page.wxml中使用组件

```
<richText 
  id='richText' 
  readOnly='{{readOnly}}'
  placeholder='{{placeholder}}' 
  formatDate='YY/MM/DD'
  buttonTxt='保存'
  bind:clearBeforeEvent='clearBeforeEvent'
  bind:clearSuccess='clearSuccess'
  bind:undo='undo'
  bind:restore='restore'
  bind:onEditorReady='onEditorReady' 
  bind:bindfocus='bindfocus' 
  bind:bindblur='bindblur' 
  bind:bindinput='bindinput' 
  bind:insertImageEvent='insertImageEvent' 
  bind:getEditorContent='getEditorContent'></richText>
```
4.   在page.js中，接受富文本编辑器相关事件。这里以插入图片为例。

```
const app = getApp();
let richText = null;  //富文本编辑器实例
Page({

  /**
   * 页面的初始数据
   */
  data: {
    
  },

  //编辑器初始化完成时触发，可以获取组件实例
  onEditorReady() {
    console.log('[onEditorReady callback]')
    richText = this.selectComponent('#richText'); //获取组件实例
  },

  //插入图片
  insertImageEvent() {
    wx.chooseImage({
      count: 1,
      success: res => {
        let path = res.tempFilePaths[0];
        //调用子组件（富文本组件）方法，图片应先上传再插入，不然预览时无法查看图片。
        richText.insertImageMethod(path).then(res => {
          console.log('[insert image success callback]=>', res)
        }).catch(res => {
          console.log('[insert image fail callback]=>', res)
        });
      }
    })
  },
})
```

### 组件属性
| 属性名         | 类型      | 默认值      | 说明       |
|-------------|---------|----------|----------|
| readOnly    | Boolean | false         | 编辑器是否只读  |
| placeholder | String  | 开始编辑吧...  | 编辑器默认提示语 |
| formatDate  | String  | YY/MM/DD      | 插入的日期格式，支持格式有：'YY-MM', 'YY.MM.DD', 'YY-MM-DD', 'YY/MM/DD', 'YY.MM.DD HH:MM', 'YY/MM/DD HH:MM', 'YY-MM-DD HH:MM'  |
| buttonTxt   | String  | 保存       | 按钮文字     |


### 组件方法
| 函数               | 类型       | 默认值 | 说明                   |
|------------------|----------|-----|----------------------|
| clearBeforeEvent | function | 空   | 清空编辑器内容之前的回调       |
| clearSuccess     | function | 空   | 清空编辑器内容成功时回调       |
| undo             | function | 空   | 撤销内容成功时回调           |
| restore          | function | 空   | 恢复内容成功时回调             |
| onEditorReady    | function | 空   | 编辑器初始化完成时回调，可以获取组件实例 |
| bindfocus        | function | 空   | 编辑器聚焦时触发             |
| bindblur         | function | 空   | 编辑器失去焦点时触发           |
| bindinput        | function | 空   | 编辑器输入中时触发，实时返回富文本内容            |
| insertImageEvent | function | 空   | 插入图片按钮点击时回调           |
| getEditorContent | function | 空   | 保存按钮点击时回调，返回富文本内容           |

### 演示
1.  你可以用 [微信web开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html) 打开项目 miniprogram-richtext 查看完整演示。

2.  效果截图  
![效果截图](https://images.gitee.com/uploads/images/2020/0515/155231_09025a5b_1486634.png)

### 源码借鉴
https://github.com/vobear/miniprogram-richtext.git
感谢原作者，如有侵权，请联系删除。
