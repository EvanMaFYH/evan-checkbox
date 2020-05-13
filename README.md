# evan-checkbox
uniapp复选框组件，支持双向数据绑定，支持自定义icon   
测试过微信小程序、app（包括nvue模式）、h5

### 1.基本用法

```
<template>
    <evan-checkbox v-model="checked">复选框</evan-checkbox>
    <evan-checkbox-group v-model="color">
        <evan-checkbox ﻿v-for="item in colorList" :key="item.value" :label="item.value">﻿{{item.label}}</evan-checkbox>
    </evan-checkbox-group>
</template>
<script>
export default{
    data(){
        return{
            checked:true,
            color: null,
            colorList: [{
                    label: '红色',
                    value: 'red'
                },
                {
                    label: '绿色',
                    value: 'green'
                },
                {
                    label: '蓝色',
                    value: 'blue'
                },
                {
                    label: '粉色',
                    value: 'pink'
                },
                {
                    label: '黑色',
                    value: 'black'
                }
            ]
        }
    }
}
</script>
```

### 2.更多用法
参考[github demo](https://github.com/EvanMaFYH/evan-checkbox)中的用法

### 3.注意点

#### 1.nvue模式下checkbox的文本需要包裹在text组件中
```
<evan-checkbox v-model="checked">复选框</evan-checkbox>
<evan-checkbox v-model="checked"><text class="your-class">复选框</text></evan-checkbox>
```
#### 2.通过slot方式自定义图标时两种状态如果是某个图标的显示与隐藏，在外层套一个view，不然在h5下有问题
```
<evan-checkbox v-model="checked">
    完全自定义图标
    <template slot="icon">
        <image v-if="checked" style="width: 20px;height: 20px;display: block;" src="/static/logo.png"></image>
        <image v-else style="width: 30px;height: 30px;display: block;" src="/static/logo.png"></image>
    </template>
</evan-checkbox>

<evan-checkbox v-model="checked">
    完全自定义图标
    <template slot="icon">
        <view>
            <uni-icons v-if="checked" type="checkmarkempty" size="30" color="#108ee9"></uni-icons>
        </view>
    </template>
</evan-checkbox>
```

#### 3.demo项目需要安装依赖，记得先npm install，因为加入了与EvanForm组件配合使用的例子

#### 4.EvanCheckboxPopup中使用到了解构插槽，目前uniapp似乎在这一块问题还比较多，如果要在同一个页面使用多个EvanCheckboxPopup组件时记得要import多个，不然目前slot中的内容的样式除了第一个都会被最后一个影响，预计官方在下个版本修复，参考[这里](https://github.com/dcloudio/uni-app/issues/1662)
```
<evan-checkbox-popup></evan-checkbox-popup>
<evan-checkbox-popup2></evan-checkbox-popup2>
<evan-checkbox-popup3></evan-checkbox-popup3>

import EvanCheckboxPopup from '@/components/evan-checkbox/evan-checkbox-popup.vue'
import EvanCheckboxPopup2 from '@/components/evan-checkbox/evan-checkbox-popup.vue'
import EvanCheckboxPopup3 from '@/components/evan-checkbox/evan-checkbox-popup.vue'
export default{
    components:{
        EvanCheckboxGroup,
        EvanCheckboxPopup2,
        EvanCheckboxPopup3
    }
}
```

#### 5.如果层级出现问题去调一下uniPopup组件的层级试试（uniPopup组件推荐将popup写在其他元素后面，但是目前是在组件中的，因此如果嵌套比较深可能无法弹出）

### evan-checkbox props
| 参数           | 说明            | 类型    | 可选值     | 默认值  |    
| :-------------------- | :------------------------------ | :---------- | :-------- | :--- |  
| v-model | 是否选中 | boolean | - | false |
| shape | 在非自定icon模式下图标的形状 | string | round/square | round |
| label | 选中状态的值，只在group模式下有效 | string｜number | - | - |
| disabled | 是否禁用 | boolean | - | false |
| primary-color | 主题色 | string | - | #108ee9 |
| icon | 自定义图标，使用uni-icons的图标 | string | - | - |
| icon-size | 图标的大小 | number | - | 20 |
| title-style | 复选框文本样式 | object | - | - |
| prevent-click | 阻止内部的点击事件，只在自定义样式由外部触发toggle事件时设置为true | boolean | - | false |

### evan-checkbox events
| name | 说明 | 回调参数 |
| :--- | :---------------- | ------------------|
| change | 选中状态发生变更 | 更新后的选中状态 |

### evan-checkbox methods
| 方法名   | 说明       | 参数     |   
| :--------------- | :------------------------------------ | :-------|
| toggle | 切换复选框的选中状态，一般配合prevent-click属性使用来自定义样式 | - |

### evan-checkbox slot
| name | 说明 |
| :--- | :---------------- |
| icon | evan-checkbox自定义icon，如果是用的图片等显示不居中，将图片的display设为block |

### evan-checkbox-group props   
| 参数           | 说明            | 类型    | 可选值     | 默认值  |    
| :------------- | :------------------------------ | :------ | :----- | :--- |  
| v-model | 选中项的label | array | - | - |
| disabled | 是否禁用 | boolean | - | false |
| max | 最多选中的个数 | number | - | - |

### evan-checkbox-group events
| name | 说明 | 回调参数 |
| :--- | :---------------- | ------------------|
| change | 选中状态发生变更 | 更新后选中值的label组 |

### evan-checkbox-group methods
| 方法名   | 说明       | 参数     |   
| :--------------- | :------------------------------------ | :-------|
| selectAll | 全选，注意主动调用该方法不会触发change事件 | - |
| selectReverse | 反选，注意主动调用该方法不会触发change事件 | - |
| clearAll | 清空，注意主动调用该方法不会触发change事件 | - |

### evan-checkbox-popup props   
| 参数           | 说明            | 类型    | 可选值     | 默认值  |    
| :------------- | :------------------------------ | :------ | :----- | :--- |  
| v-model | 选中项的label | array | - | - |
| options | 选项数组 | array | - | - |
| optionLabel | 选项数组中显示内容对应键名 | string | - | label |
| optionValue | 选项数组中value内容对应键名 | string | - | value |
| primary-color | 主题色 | string | - | #108ee9 |
| cancelText | 取消按钮的文字 | string | - | 取消 |
| confirmText | 确定按钮的文字 | string | - | 确定 |
| title | 标题的文字 | string | - | 请选择 |
| max | 最多选中的个数 | number | - | - |
| labelSeparator | 传递给父组件的当前选中值以什么符号分隔（目前uniapp只支持解构插槽） | string | - | , |
| maskClick | 点击遮罩层是否关闭 | boolean | - | true |

### evan-checkbox-popup slot
| name | 说明 |
| :--- | :---------------- |
| - | 触发选项弹窗的元素 |
| trigger | 触发选项弹窗的元素，会返回给父组件当前选中项的显示值 |

### evan-checkbox-popup events
| name | 说明 | 回调参数 |
| :--- | :---------------- | ------------------|
| confirm | 点击确定的回调 | 点击确定时选中值的value数组 |
| cancel | 取消关闭弹窗时的回调（包括点击遮罩层关闭）| 关闭时选中值的value数组 |

### evan-checkbox-popup methods
| 方法名   | 说明       | 参数     |   
| :--------------- | :------------------------------------ | :-------|
| selectAll | 全选，注意主动调用该方法不会触发change事件 | - |
| selectReverse | 反选，注意主动调用该方法不会触发change事件 | - |
| clearAll | 清空，注意主动调用该方法不会触发change事件 | - |
