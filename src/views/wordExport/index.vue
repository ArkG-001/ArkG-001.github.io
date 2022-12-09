<template>
  <div class="word-box" @click="exportWord">导出word</div>
</template>

<script>
import Docxtemplater from 'docxtemplater'
import JSZip from 'jszip'
import JSZipUtils from 'jszip-utils'
import { saveAs } from 'file-saver'

export default {
  name: 'Docx',
  props: {
    // 文件名
    fileName: {
      type: String,
      default: ''
    },
    // word模板名称
    fileTemplete: {
      type: String,
      default: ''
    },
    // word数据
    wordData: {
      type: Object,
      default: () => {}
    }
  },
  methods: {
    // 点击导出word
    exportWordDocx() {
      // 读取并获得模板文件的二进制内容
      JSZipUtils.getBinaryContent(
        './static/file/' + this.fileTemplete + '.docx',
        (error, content) => {
          // 抛出异常
          if (error) {
            throw error
          }
          // 创建一个JSZip实例，内容为模板的内容
          const zip = new JSZip(content)
          // 创建并加载docxtemplater实例对象
          const doc = new Docxtemplater()
          doc.loadZip(zip)
          // 设置模板变量的值
          doc.setData({
            ...this.wordData
          })
          try {
            // 用模板变量的值替换所有模板变量
            doc.render()
          } catch (error) {
            // 抛出异常
            const e = {
              message: error.message,
              name: error.name,
              stack: error.stack,
              properties: error.properties
            }
            console.log(JSON.stringify({ error: e }))
            throw error
          }
          // 生成一个代表docxtemplater对象的zip文件（不是一个真实的文件，而是在内存中的表示）
          const out = doc.getZip().generate({
            type: 'blob',
            mimeType:
              'application/vnd.openxmlformats-officedocument.wordprocessingml.document'
          })
          // 将目标文件对象保存为目标类型的文件，并命名
          saveAs(out, this.fileName + '.docx')
        }
      )
    },
    exportWord: function() {
      const that = this
      // 读取并获得模板文件的二进制内容
      JSZipUtils.getBinaryContent('../../cardreResume.docx', function(error, content) {
        // model.docx是模板。我们在导出的时候，会根据此模板来导出对应的数据
        // 抛出异常
        if (error) {
          throw error
        }

        // 创建一个PizZip实例，内容为模板的内容
        const zip = new JSZip(content)
        // let zip = new PizZip(content);
        // 创建并加载docxtemplater实例对象
        const doc = new Docxtemplater().loadZip(zip)
        // 设置模板变量的值
        doc.setData({
          ...that.tableData
        })

        try {
          // 用模板变量的值替换所有模板变量
          doc.render()
        } catch (error) {
          // 抛出异常
          const e = {
            message: error.message,
            name: error.name,
            stack: error.stack,
            properties: error.properties
          }
          console.log(JSON.stringify({ error: e }))
          throw error
        }

        // 生成一个代表docxtemplater对象的zip文件（不是一个真实的文件，而是在内存中的表示）
        const out = doc.getZip().generate({
          type: 'blob',
          mimeType: 'application/vnd.openxmlformats-officedocument.wordprocessingml.document'
        })
        // 将目标文件对象保存为目标类型的文件，并命名
        saveAs(out, '干部任免审批表-' + that.tableData.name + '.docx')
      })
    }
  }
}
</script>
