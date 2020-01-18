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
