<script setup>
import { ref } from 'vue'
const userPwdInfo = ref({
    old_pwd: '',
    new_pwd: '',
    re_pwd: ''
})
const checkPwd = () => {

}
const rules = {
    old_pwd: [
        { required: true, message: '請輸入舊密碼', trigger: 'blur' },
    ],
    new_pwd: [
        { required: true, message: '請輸入新密碼', trigger: 'blur' },
    ],
    re_pwd: [
        { required: true, message: '請確認新密碼', trigger: 'blur' },
    ],
}

//修改個人信息
import { userPwdUpdateService } from '@/api/user.js'
import { ElMessage } from 'element-plus'
const userPwdUpdate = async () => {
    if (userPwdInfo.value.new_pwd !== userPwdInfo.value.re_pwd) {
        ElMessage.error('兩次填寫的新密碼不一樣')
        return
    }
    //調用接口
    let result = await userPwdUpdateService(userPwdInfo.value);
    ElMessage.success(result.msg ? result.msg : '修改成功');
}
</script>
<template>
    <el-card class="page-container">
        <template #header>
            <div class="header">
                <span>修改密碼</span>
            </div>
        </template>
        <el-row>
            <el-col :span="12">
                <el-form :model="userPwdInfo" :rules="rules" label-width="100px" size="large">
                    <el-form-item label="原密碼" prop="old_pwd">
                        <el-input type="password" show-password v-model="userPwdInfo.old_pwd"></el-input>
                    </el-form-item>
                    <el-form-item label="新密碼" prop="new_pwd">
                        <el-input type="password" show-password v-model="userPwdInfo.new_pwd"></el-input>
                    </el-form-item>
                    <el-form-item label="確認新密碼" prop="re_pwd">
                        <el-input type="password" show-password v-model="userPwdInfo.re_pwd"></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="userPwdUpdate">提交修改</el-button>
                    </el-form-item>
                </el-form>
            </el-col>
        </el-row>
    </el-card>
</template>