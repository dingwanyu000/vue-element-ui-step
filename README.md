# extensionStep
a extension step of element-ui

![image](https://github.com/dingwanyu000/extensionStep/blob/master/demo.png)

import ExtensionStep from './ExtensionStep' // 文件引入

components: {ExtensionStep}, 

<extension-step :data="data" :size="size" v-on:child-say="listenToChild"></extension-step>

props：
size：3 // 每行显示个数
data:[{ id: 1, // buttonId
        state: 'success', //buttonState
        name: '步骤1', // buttonName
        description: '我是描述文字',
        isReal: true // 是否为真实节点
      }]
      
listenToChild返回参数为data中的id

      
