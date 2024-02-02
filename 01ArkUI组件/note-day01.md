`harmonyOs 4.0 + Next星河版 开发学习文档   . LiuJinTao` test



@[toc]

2024年2月1日

## 一、网络请求权限配置



为了方便模拟器对网络请求的权限，所以我们事先在module.json5 文件中进行配置好网络秦秋权限的配置 如下：

```ts
"module": {
        "requestPermissions": [
      {
        "name": "ohos.permission.INTERNET"
      }
    ],
}
```

添加完毕后，我们模拟器就能正常的请求我们网络上面的资源了。



## 二、Image组件





![](D:\桌面\桌面文件夹\文件夹\Typora\harmonyOs 4.0 + Next星河版\01ArkUI组件\image\01 image组件.png)

#### 2.1 第一种：使用网络超链接实现图片渲染

+ 最开始我们需要进行配置网络请求权限(最开始上面已经配置好了)

代码示例：

```ts
   
        // 使用超链接网络加载的图片渲染
Image('https://res2.vmallres.com/pimages/uomcdn/CN/pms/202401/gbom/6942103109591/428_428_650AA2D1F4312445D02527C6CC0FD74Fmp.png')
          .width(250)
```



效果展示：


![](D:\桌面\桌面文件夹\文件夹\Typora\harmonyOs 4.0 + Next星河版\01ArkUI组件\image\02 第一种图片渲染效果展示.png)

#### 2.2  第二种：使用本地资源进行渲染（1）

+ 代码示例：

```ts
        // 使用本地资源图片渲染方式（resources目录下media文件中）
        Image($r('app.media.icon'))
          .width(250)
```

+ 效果展示：

![](D:\桌面\桌面文件夹\文件夹\Typora\harmonyOs 4.0 + Next星河版\01ArkUI组件\image\03 第二种图片渲染效果展示.png)

当然，我们上面说过，除了组件的通用属性外，还有一些组件独有的属性，如上图，图片看起来有点锯齿，此时就得使用Image组件上的插值属性了

+ 代码如下：

```ts

          .interpolation(ImageInterpolation.High)	// update
```

+ 效果展示：

当然这里使用的是预览模式，效果看起来不是很明显，大家使用模拟器或者真机效果会更加。

![](D:\桌面\桌面文件夹\文件夹\Typora\harmonyOs 4.0 + Next星河版\01ArkUI组件\image\04 第二种使用了插值属性后的效果图.png)

#### 2.3 第三种：使用本地资源进行渲染（2）

+ 代码示例：

```ts
        // 使用本地资源图片渲染方式（resources目录下rawfile文件中） 这种只需要写图片名字即可
        Image($rawfile("mate60.png"))
          .width(250)
          .interpolation(ImageInterpolation.High)
      }
```



+ 效果展示：

![](D:\桌面\桌面文件夹\文件夹\Typora\harmonyOs 4.0 + Next星河版\01ArkUI组件\image\05 第三种图片渲染的方式 rawfile目录下.png)



## 三、Text组件

![](D:\桌面\桌面文件夹\文件夹\Typora\harmonyOs 4.0 + Next星河版\01ArkUI组件\image\06 Text组件.png)

+ 代码示例：

第一步：配置字符串变量

1、`在en_US目录中的string.json文件中配置如下代码`

```ts
{
    "String" : [
        {
      	 "name": "width_label",
     	 "value": "Image Width:"
    	}
    ]
}
```



2、`在zh_CN目录中的string.json文件中配置如下代码`

```ts
{
    "String" : [
        {
      	 "name": "width_label",
     	 "value": "图片宽度: "
    	}
    ]
}
```

3、` 在element目录中的string文件配置如下代码：`

```ts
{
    "String" : [
        {
      	 "name": "width_label",
     	 "value": "图片宽度: "    // 这里中英文都可以，因为会自动根据我们的语言进行自动匹配上面我们配置的（但是name必须一样）
    	}
    ]
}
```

4、 `最后在我们页面中 ImagePage.ets 文件中使用`

```ts
@Entry
@Component
struct ImagePage { // 这里我的文件名叫做 IamgePage
  @State message: string = 'Hello World'
  build() {
    Row() {
      Column() {
        Image($rawfile("mate60.png"))
          .width(250)
          .interpolation(ImageInterpolation.High)
        Text($r('app.string.width_label'))
          .fontSize(30) // 设置字体大小
          .fontWeight(FontWeight.Bold)  // 设置字体加粗
      }
      .width('100%')
    }
    .height('100%')
  }
}
```



+ 效果展示：

![](D:\桌面\桌面文件夹\文件夹\Typora\harmonyOs 4.0 + Next星河版\01ArkUI组件\image\07 Text 组件的基本使用以及效果图.png)



## 四、 TextInput组件

![](D:\桌面\桌面文件夹\文件夹\Typora\harmonyOs 4.0 + Next星河版\01ArkUI组件\image\08 TextInput文本框组件.png)



+ 代码示例：

```ts
@Entry
@Component
struct ImagePage {
  @State message: string = 'Hello World'
  @State imageWidth: number = 100
  build() {
    Row() {
      Column() {
        // 设置图片
        Image($rawfile("mate60.png"))
          .width(this.imageWidth)
          .interpolation(ImageInterpolation.High)
        // 设置文本
        Text($r('app.string.width_label'))
          .fontSize(18) // 设置字体大小
          .fontWeight(FontWeight.Bold)  // 设置字体加粗
        // * 设置输入框
        TextInput({text: this.imageWidth.toFixed(0)}) // 值为图片宽度，然后类型转换
          .width(100)
          .backgroundColor('#FFCCAA') // 输入框背景色
          .type(InputType.Number)     // 输入框类型
          .onChange(value => {  // 更新事件触发的箭头函数
            this.imageWidth = parseInt(value) // 将输入的字符串转为数字赋值给变量
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}
```

+ 效果展示：

![](D:\桌面\桌面文件夹\文件夹\Typora\harmonyOs 4.0 + Next星河版\01ArkUI组件\image\09 TextInput效果图.png)

+ 这里我们再输入框中修改数据，都能实现图片宽度的自动变化



