<template>
    <section>
        <div v-for="(item,rowIndex) in newData" :key="rowIndex">
            <el-row>
                <el-col v-for="(subItem,index) in item" :key="index" :span="5" style="padding:0;height:70px;margin:20">
                    <div v-if="subItem.isReal">
                        <div v-if="subItem.state == 'success'">
                            <el-button :id="subItem.id" icon="el-icon-check" @click="click" size="medium" type="primary" plain circle></el-button>
                            <div v-bind:style="successStyle">{{subItem.name}}</div>
                            <div v-bind:style="successStyle">{{subItem.description}}</div>
                        </div>
                        <div v-else-if="subItem.state == 'info'">
                            <el-button :id="subItem.id" icon="el-icon-edit-outline" @click="click" size="medium" type="info" plain circle></el-button>
                            <div v-bind:style="infoStyle">{{subItem.name}}</div>
                            <div v-bind:style="infoStyle">{{subItem.description}}</div>
                        </div>
                        <div v-else>
                            <el-button :id="subItem.id" icon="el-icon-close" @click="click" size="medium" type="danger" plain circle></el-button>
                            <div v-bind:style="errorStyle">{{subItem.name}}</div>
                            <div v-bind:style="errorStyle">{{subItem.description}}</div>
                        </div>
                        <div v-if="(index+1)%size!=0&&(index!=(size-1)&&item[index+1].isReal)">
                            <div v-if="rowIndex%2==1">
                                <div v-if="index!=(size-1)&&item[index+1].state == 'success'">
                                    <div v-bind:style="successBorderBottomStyle"></div>
                                </div>
                                <div v-else-if="index!=(size-1)&&item[index+1].state == 'info'">
                                    <div v-bind:style="infoBorderBottomStyle"></div>
                                </div>
                                <div v-else>
                                    <div v-bind:style="errorBorderBottomStyle"></div>
                                </div>
                            </div>
                            <div v-else>
                                <div v-if="subItem.state == 'success'">
                                    <div v-bind:style="successBorderBottomStyle"></div>
                                </div>
                                <div v-else-if="subItem.state == 'info'">
                                    <div v-bind:style="infoBorderBottomStyle"></div>
                                </div>
                                <div v-else>
                                    <div v-bind:style="errorBorderBottomStyle"></div>
                                </div>
                            </div>
                        </div>
                        <div v-if="rowIndex%2==1">
                            <div v-if="((rowIndex%2==1&&index==0)||(rowIndex%2==0&&index==(size-1)))&&newData[rowIndex+1]!=undefined">
                                <div v-if="subItem.state == 'success'">
                                    <div style="border-left:3px solid #409EFF;height:28px;margin:19px 0px 0px 15px"></div>
                                </div>
                                <div v-else-if="subItem.state == 'info'">
                                    <div style="border-left:3px solid #909399;height:28px;margin:19px 0px 0px 15px"></div>
                                </div>
                                <div v-else>
                                    <div style="border-left:3px solid #F56C6C;height:28px;marg==in:19px 0px 0px 15px"></div>
                                </div>
                            </div>
                        </div>
                        <div v-else>
                            <div v-if="((rowIndex%2==1&&index==0)||(rowIndex%2==0&&index==(size-1)))&&newData[rowIndex+1]!=undefined">
                                <div v-if="subItem.state == 'success'">
                                    <div style="border-left:3px solid #409EFF;height:26px;margin:4px 0px 0px 15px"></div>
                                </div>
                                <div v-else-if="subItem.state == 'info'">
                                    <div style="border-left:3px solid #909399;height:26px;margin:4px 0px 0px 15px"></div>
                                </div>
                                <div v-else>
                                    <div style="border-left:3px solid #F56C6C;height:26px;margin:4px 0px 0px 15px"></div>
                                </div>
                            </div>
                        </div>
                    </div>
                </el-col>
            </el-row>
        </div>
    </section>
</template>
<script>
export default {
  data() {
    return {
      newData: [],
      successStyle: {
        'text-align': 'center',
        float: 'left',
        'margin-left': '-6em',
        'margin-right': '12.5em',
        width: '180px',
        color: '#409EFF'
      },
      infoStyle: {
        'text-align': 'center',
        float: 'left',
        'margin-left': '-6em',
        'margin-right': '12.5em',
        width: '180px',
        color: '#909399'
      },
      errorStyle: {
        'text-align': 'center',
        float: 'left',
        'margin-left': '-6em',
        'margin-right': '12.5em',
        width: '180px',
        color: '#F56C6C'
      },
      successBorderBottomStyle: {
        'border-bottom': '3px solid #409EFF',
        margin: '-19px 7px 0px 41px'
      },
      infoBorderBottomStyle: {
        'border-bottom': '3px solid #909399',
        margin: '-19px 7px 0px 41px'
      },
      errorBorderBottomStyle: {
        'border-bottom': '3px solid #F56C6C',
        margin: '-19px 7px 0px 41px'
      }
    }
  },
  props: {
    size: {
      type: Number,
      default: false
    },
    data: {
      type: Array,
      default: false
    }
  },
  mounted() {
    this.newData = this.chunk(this.$props.data, this.$props.size)
  },
  methods: {
    click(e) {
      if (e.srcElement.tagName == 'I') {
        this.$emit('child-say', e.srcElement.parentElement.id)
      } else {
        this.$emit('child-say', e.srcElement.id)
      }
    },
    chunk(arr, size) {
      var newArr = []
      for (var i = 0; i < arr.length; i = i + size) {
        newArr.push(arr.slice(i, i + size))
      }
      var obj = {
        state: 'error',
        name: '',
        isReal: false
      }
      if (newArr[newArr.length - 1].length != this.size) {
        let arraySize = newArr[newArr.length - 1].length
        for (let index = 0; index < this.size - arraySize; index++) {
          if (newArr.length % 2 == 1) {
            newArr[newArr.length - 1].push(obj)
          } else {
            newArr[newArr.length - 1].unshift(obj)
          }
        }
      }
      return newArr
    }
  }
}
</script>

