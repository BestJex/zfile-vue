<template>
    <el-row :gutter="20">
        <el-col :span="8" :offset="8">
            <el-card class="box-card" align-center shadow="always">
                <el-form ref="form" :rules="rules" :model="form"
                         label-width="auto"
                         :status-icon="true"
                         v-loading="loading"
                         element-loading-text="保存并初始化中.">
                    <el-form-item label="站点名称" prop="siteName">
                        <el-input v-model="form.siteName"/>
                    </el-form-item>

                    <el-form-item label="管理员账号" prop="username">
                        <el-input v-model.trim="form.username"/>
                    </el-form-item>

                    <el-form-item label="管理员密码" prop="password">
                        <el-input v-model.trim="form.password"/>
                    </el-form-item>

                    <el-form-item label="站点地址/域名" prop="domain">
                        <el-input v-model.trim="form.domain"/>
                    </el-form-item>

                    <el-form-item>
                        <el-button type="primary" @click="submitForm('form')">确认</el-button>
                    </el-form-item>
                </el-form>
            </el-card>
        </el-col>
    </el-row>
</template>

<script>
    import qs from 'qs';

    export default {
        name: "Install",
        data() {
            return {
                active: 1,
                form: {
                    siteName: '',
                    username: '',
                    password: '',
                    domain: window.location.protocol + "//" + window.location.host
                },
                loading: false,
                storageStrategyForm: [],
                supportStrategy: [],
                rules: {
                    siteName: [
                        {required: true, message: '请输入站点名称', trigger: 'change'},
                    ],
                    username: [
                        {required: true, message: '请输入管理员账号', trigger: 'change'},
                    ],
                    password: [
                        {required: true, message: '请输入管理员密码', trigger: 'change'},
                    ],
                    domain: [
                        {required: true, type: 'url', message: '请输入正确的域名, 需以 http:// 或 https:// 开头', trigger: 'change'},
                    ]
                }
            };
        },
        methods: {
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        this.loading = true;
                        let that = this;
                        this.$http.post('install', qs.stringify(this.form)).then((response) => {
                            this.loading = false;
                            let data =  response.data;
                            this.$message({
                                message: "初始化成功",
                                type: data.code === 0 ? 'success' : 'error',
                                duration: 1500,
                                onClose() {
                                    that.$router.push('/main')
                                }
                            });
                        })
                    } else {
                        this.loading = false;
                        return false;
                    }
                });
            }
        },
        created() {
            // this.$http.get('is-installed').then((response) => {
            //     let data =  response.data;
            //     if (data.code !== 0) {
            //         this.$router.push('/main');
            //     }
            // })
        }
    }
</script>

<style scoped>
    .box-card {
        padding-top: 30px;
        padding-right: 30px;
        margin: 25vh auto;
        height: 40vh;
        overflow-y: auto;
    }

    .el-select {
        width: 100%;
    }


    .zfile-word-aux {
        margin-left: 20px;
        color: #aaaaaa;
    }
</style>