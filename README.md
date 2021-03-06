# taro-iconfont-cli
在Taro框架中使用iconfont图标，不依赖字体，支持多色彩。

# 支持平台

* React-Native
* 微信小程序
* 支付宝小程序
* 百度小程序
* 头条小程序
* QQ小程序
* H5

# 特性
1、一键生成标准组件，多端支持
<br>
2、使用方便，import即可
<br>
3、支持多色彩
<br>
4、支持自定义颜色
<br>
5、支持es6和typescript两种模式

# Step 1
安装插件
```bash
# Yarn
yarn add taro-iconfont-cli --dev

# Npm
npm install taro-iconfont-cli --save-dev
```


# Step 2
生成配置文件
```bash
npx iconfont-init
```
此时项目根目录会生成一个`iconfont.json`的文件，内容如下：
```json
{
  "symbol_url": "请参考README.md，复制 http://iconfont.cn 官网提供的JS链接",
  "save_dir": "./iconfont",
  "use_typescript": false,
  "platforms": "*",
  "use_rpx": true,
  "trim_icon_prefix": "icon",
  "default_icon_size": 18,
  "componnent_name": "IconFont"
}
```
### 配置参数说明：
### symbol_url
请直接复制[iconfont](http://iconfont.cn)官网提供的项目链接。请务必看清是`.js`后缀而不是.css后缀。如果你现在还没有创建iconfont的仓库，那么可以填入这个链接去测试：`http://at.alicdn.com/t/font_1373348_kk9y3jk2omq.js`

<br />

![](https://github.com/fwh1990/mini-program-iconfont-cli/blob/master/images/symbol-url.png?raw=true)

### save_dir
根据iconfont图标生成的组件存放的位置。每次生成组件之前，该文件夹都会被清空。

### use_typescript
如果您的项目使用Typescript编写，请设置为true。这个选项将决定生成的图标组件是`.tsx`还是`.js`后缀。

当该值为false时，我们会为您的图标生成`.js`和`.d.ts`两个文件，以便您能享受到最好的开发体验。

### platforms
选择需要支持的平台，默认是`*`，意味着所有平台都需要支持（如果有）。如果你只想支持部分平台，也可以设置成数组：
```json5
{
  // 选择你需要的平台
  // 说明 =>  weapp: 微信  |  swan: 百度  |  alipay: 支付宝  |  tt: 字节跳动
  "platforms": ["weapp","swan" ,"alipay", "rn", "h5", "tt", "qq"]
}
```

### use_rpx
是否使用[尺寸单位rpx](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxss.html#%E5%B0%BA%E5%AF%B8%E5%8D%95%E4%BD%8D)还是普通的像素单位`px`。默认值为true，与Taro保持一致的缩放。您也可以设置为false，强制使用`px`

### trim_icon_prefix
如果你的图标有通用的前缀，而你在使用的时候又不想重复去写，那么可以通过这种配置这个选项把前缀统一去掉。

### default_icon_size
我们将为每个生成的图标组件加入默认的字体大小，当然，你也可以通过传入props的方式改变这个size值。

### component_name
就是组件的名称，默认名称为`IconFont`，您也可以改成`Icons`或者您喜欢的名字。记住，它是一个变量名，您必须遵守Javascript中关于变量的语法规则。以及作为组件名，请尽量以大写字母开头。


# Step 3
开始生成Taro标准组件
```bash
npx iconfont-taro
```
生成后查看您设置的保存目录中是否含有所有的图标

-------

在生成代码之前，你可以顺便参考[snapshots目录](https://github.com/fwh1990/taro-iconfont-cli/tree/master/snapshots)自动生成的快照文件。

# Step 4
如果您需要将Taro编译成`React-Native`的项目，那么请看完这一步，否则您可以直接忽略这一步

----------

在React-Native中，图标需要依赖`react-native-svg`，所以您需要在`taro`项目中安装这个插件
```bash
# Npm
npm install react-native-svg

# Yarn
yarn add react-native-svg
```

接着，您需要在壳子中也安装这个插件，并做静态链接。

官方推荐的壳子是：[NervJS/taro-native-shell](https://github.com/NervJS/taro-native-shell)，您可以查看仓库的README.md或者查看[官方文档](https://taro-docs.jd.com/taro/docs/react-native.html)了解更多壳子信息
```bash
# 在壳子`taro-native-shell`中或是你自定义的壳子

# Npm or Yarn
yarn add react-native-svg

# Link android
react-native link react-native-svg

# Link ios
cd ios && pod install && cd -

```

处理完后您就可以顺利地启动项目了。。。可能也不会太顺利，因为壳子`taro-native-shell`的版本比RN官方低很多，所以启动过程中碰到的问题也很多。

# 使用
在Page中使用图标
```jsx harmony
import Taro, { Component } from '@tarojs/taro';
import IconFont from '../iconfont';

class App extends Component {
  render() {
    return <IconFont name="alipay" />;
  }
}

export default App;
```
更多用法：
```jsx harmony
// 原色彩
<IconFont name="alipay" />

// 单色：红色
<IconFont name="alipay" color="red" />

// 多色：红色+橘色
<IconFont name="alipay" color={['red', 'orange']} size={300} />

// 不同格式的颜色写法
<IconFont name="alipay" color={['#333', 'rgb(50, 124, 39)']} />


// 与文字对齐
<View style={{ display: 'flex', alignItems: 'center' }}>
  <Text>Hello</text>
  <IconFont name="alipay" />
</View>
```

# 更新图标
当您在iconfont.cn中的图标有变更时，只需更改配置`symbol_url`，然后再次执行`Step 3`即可生成最新的图标组件
```bash
# 修改 symbol_url 配置后执行：
npx iconfont-taro
```

# 扩展
|平台|库|
|----|---|
|小程序|[mini-program-iconfont-cli](https://github.com/fwh1990/mini-program-iconfont-cli)|
|React Native|[react-native-iconfont-cli](https://github.com/fwh1990/react-native-iconfont-cli)|
|React H5|[react-iconfont-cli](https://github.com/fwh1990/react-iconfont-cli)|
|Flutter|[flutter-iconfont-cli](https://github.com/fwh1990/flutter-iconfont-cli)|


--------

欢迎使用，并给我一些反馈和建议，让这个库做的更好
