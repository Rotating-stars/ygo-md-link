---
title: "工具"
order: 1
---

<script setup>
import { ref } from 'vue'
import { ElMessage } from 'element-plus'
// 表单ref对象
const formRef = ref(null)
// 表单数据
const formDate = ref({
  mdPath: '',
  id: '',
  mdcn: '',
})
// 校验规则
const rules = {
  mdPath: [
    { required: true, message: '请输入md文件路径', trigger: 'blur' }
  ],
  id: [
    { required: true, message: '请输入登录账号编码', trigger: 'blur' }
  ]
  // mdcn 有默认值且非必填，所以不需要校验规则
}
// 重置方法
const resetForm = () => {
  formDate.value = {
    mdPath: '',
    id: '',
    mdcn: '',
  };
  // 重置表单校验状态
  formRef.value?.clearValidate();
  ElMessage.primary('已重置！');
}
// 脚本字符串
const createBatString = () => {
  const pathID = formDate.value.mdPath + "\\LocalData\\" + formDate.value.id + "\\0000"
  const pathMDCN = formDate.value.mdPath + "\\LocalData\\" + (formDate.value.mdcn||"MDCN") + "\\0000"
  const batString = `mklink /d "${pathID}" "${pathMDCN}"`
  return batString
}
// 下载方法
const download = (batString) => {
  // 创建 Blob 对象
  const blob = new Blob([batString], {type: 'application/bat'});
  // 创建下载链接
  const url = URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.href = url;
  link.download = 'YGO-MD-Link.bat';
  // 下载开始提示
  ElMessage.success('开始下载！');
  // 触发点击事件
  document.body.appendChild(link);
  link.click();
  // 清理
  document.body.removeChild(link);
  URL.revokeObjectURL(url);
}
// 点击下载方法
const handleDownload = async () => {
  // 校验表单
  if (!formRef.value) return
  try {
    await formRef.value.validate();
    // 校验通过后执行下载
    download(createBatString());
  } catch (error) {
    ElMessage.warning('请填写完整信息后再下载！');
  }
}
// 复制方法
const copyBatString = async () => {
  // 复制前校验
  if (!formRef.value) return
  try {
    await formRef.value.validate()
    // 调用浏览器复制API
    await navigator.clipboard.writeText(createBatString());
    ElMessage.success('复制成功！');
  } catch (err) {
    ElMessage.warning('请填写完整信息后再复制！');
  }
}

</script>

# 链接工具

::: warning 在使用本项目前

你需要知道操作区标签的**具体含义**，详细说明请查阅[文档](文档.html)

:::

## 操作区
<el-card shadow="hover">
  <el-form
    ref="formRef"
    :model="formDate"
    :rules="rules"
    label-position="right"
    label-width="130px">
    <el-form-item label="md文件路径：" prop="mdPath">
      <el-input v-model="formDate.mdPath" placeholder="示例：D:\Steam\steamapps\common"/>
    </el-form-item>
    <el-form-item label="登录账号编码：" prop="id">
      <el-input v-model="formDate.id" placeholder="示例：3e5b93f3"/>
    </el-form-item>
    <el-form-item label="原始文件编码：">
      <el-input v-model="formDate.mdcn" placeholder="默认MDCN"/>
    </el-form-item>
  </el-form>
  <template #footer>
    <el-button type="primary" @click="handleDownload()">
      下载脚本文件
    </el-button>
    <el-button @click="resetForm()">重置</el-button>
  </template>
</el-card>

## 脚本预览

<el-card shadow="hover">
  {{createBatString()}}
  <template #footer>
    <el-button type="primary" @click="copyBatString()">点击复制</el-button>
  </template>
</el-card>
