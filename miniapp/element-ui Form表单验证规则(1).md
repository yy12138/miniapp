
```
telphone: [
    { required: true, message: '请输入手机号', trigger: 'blur' },
    { pattern: /^1[34578]\d{9}$/, message: '目前只支持中国大陆的手机号码' },
    { validator: validatePass2, trigger: 'blur' }
]
```
pattern匹配正则
required表示必须输入
validator表示自定义规则，通过回调函数表达错误信息

```
var validatePass = (rule, value, callback) => {
    if (value === '') {
      callback(new Error('请输入密码'));
    } else {
      if (this.ruleForm2.checkPass !== '') {
        this.$refs.ruleForm2.validateField('checkPass');
      }
      callback();
    }
    };
    var validatePass2 = (rule, value, callback) => {
    if (value === '') {
      callback(new Error('请再次输入密码'));
    } else if (value !== this.ruleForm2.pass) {
      callback(new Error('两次输入密码不一致!'));
    } else {
      callback();
    }
    };
     var checkPhone = (rule, value, callback) => {
        if (!value) {
            return callback(new Error('手机号不能为空'));
        } else {
            const reg = /^1[3|4|5|7|8][0-9]\d{8}$/
            // console.log(reg.test(value));
            if (reg.test(value)) {
                callback();
            } else {
                return callback(new Error('请输入正确的手机号'));
            }
        }
    };
```
单个标签验证以及倒计时 vue + element

```
this.$refs['registerData'].validateField('email', (err) => {
    if (!err) {
        this.getNumberBol = false;
        this.timer = setInterval(() => {
            if (this.countDown > 0 && this.countDown <= 60) {
                this.countDown--;
            } else {
                this.getNumberBol = true;
                clearInterval(this.timer);
                this.timer = null;
            }
        },1000);
    } else {
        this.$refs['registerEmail'].focus();
    }
})
```



