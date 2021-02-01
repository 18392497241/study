####	必填项去掉*号

~~~vue
.el-form-item.is-required:not(.is-no-asterisk) .el-form-item__label-wrap>.el-form-item__label:before, .el-form-item.is-required:not(.is-no-asterisk)>.el-form-item__label:before {
            content: '';
        }
~~~

####	css样式去掉小眼睛样式

~~~vue
input[type="password"]::-ms-reveal{
            display:none;
        }
~~~

####	check登录  rules

body

~~~html
<el-form label-width="100px" :rules="rules" :model="userForm" ref="userForm" label-position="left" show-message>
                <el-form-item label="ユーザー" prop="username">
                    <el-input size="large" v-model="userForm.username" :rules="rules.username" placeholder="ユーザを入力してください" maxlength="10"></el-input>
                </el-form-item>
                <el-form-item label="パスワード" prop="password">
                    <el-input size="large" v-model="userForm.password" :rules="rules.password" placeholder="パスワードを入力してください" maxlength="8" show-password></el-input>
                </el-form-item>
                <el-form-item class="btn">
                    <el-button style="width: 100px" type="primary" @click="doLogin('userForm')">ログイン</el-button>
                    <el-button style="width: 100px" type="primary" @click="doClear">クリア</el-button>
                </el-form-item>
            </el-form>
~~~

script

~~~javascript
data() {
            return{
                userForm:{
                    username:'',
                    password:''
                },
                rules: {
                    username: [
                        { required: true,  message: 'ユーザーを入力してください。', trigger: 'blur' },
                        { max: 10, message: 'ユーザーの長さは10文字を超えてはいけません。', trigger: 'blur' }
                    ],
                    password: [
                        { required: true, message: 'パスワードを入力してください。', trigger: 'blur' },
                        { max: 8, message: 'パスワードの長さは8文字を超えてはいけません。', trigger: 'blur' }
                    ],
                }
            };
        },
~~~

method

~~~javas
doLogin(formName) {

                this.$refs[formName].validate((valid) => {
                    let data = {
                        'userId': this.userForm.username,
                        'password':this.userForm.password
                    }
                    if (valid) {
                        $.ajax({
                            type:"post",
                            url:"/attendance-record/login",
                            contentType: 'application/json',
                            data: JSON.stringify(data),
                            dataType:"JSON",
                            success:function(res){
                                if(res.code === 'login100001'){
                                    // tokenを設定
                                    localStorage.setItem('fxs_token', JSON.stringify(res.data.token))
                                    window.location.href='/attendance-record/pages/calendar/calendar.html'
                                }else{
                                    vm.clearMessage()
                                    vm.$message.error(res.msg)
                                }
                            }
                        });
                    }else{
                        this.errorEmpty();
                    }
                })
            },
            errorEmpty(){

                this.clearMessage()

                vm.$message.error({
                    showClose: true,
                    message: 'ログインユーザー情報を入力してください。',
                    type: 'error'
                });
            },
            doClear() {
                this.userForm.username = ''
                this.userForm.password = ''
            },
            clearMessage() {
                vm.$message.closeAll()
            }
        }
~~~

####	滚动条

ie隐藏、显示滚动条   外面隐藏、里面显示

~~~css
body{
     -ms-overflow-style:none;
}
#calendar{
      -ms-overflow-style:auto;
}
~~~

谷歌浏览器  隐藏

~~~css
 body::-webkit-scrollbar{ display: none;}
~~~

  火狐

~~~css
  scrollbar-width: none; 
~~~



​          