<template>
    <div id="app">
        <el-steps :active="active" align-center>
            <el-step title="事项"></el-step>
            <el-step title="申办资格审核"></el-step>
            <el-step title="业务咨询"></el-step>
            <el-step title="业务办理"></el-step>
        </el-steps>
        <el-form ref="form" :model="form" label-width="80px">

            <!--1-->
            <div class="info" v-if="active==1">
                <el-form-item>
                    <el-card class="box-card" shadow="hover">
                    <div slot="header" class="clearfix">
                    <span id="spanName">事项名称</span>
                    </div>
                    <el-autocomplete
                    popper-class="form-autocomplete"
                    v-model="form.name"
                    :fetch-suggestions="queryFormNameSearch"
                    placeholder="请输入事项名称"
                    @select="handleSelect"
                    >   
                    <i
                    class="el-icon-edit el-input__icon"
                    slot="suffix"
                    @click="handleIconClick"></i>
                    <template slot-scope="formNames">
                    <div class="name">{{ formNames.item.value }}</div>
                    </template>
                    </el-autocomplete>
                    </el-card>
                </el-form-item>

                <el-form-item>
                    <el-card class="box-card" shadow="hover">
                    <div slot="header" class="clearfix">
                    <span id="spanName">事项代码</span>
                    </div>
                    <el-autocomplete
                    popper-class="form-autocomplete"
                    v-model="form.code"
                    :fetch-suggestions="queryFormCodeSearch"
                    placeholder="请输入事项代码"
                    @select="handleSelect"
                    >
                    <i
                    class="el-icon-edit el-input__icon"
                    slot="suffix"
                    @click="handleIconClick"></i>
                    <template slot-scope="formCodes">
                    <div class="name">{{ formCodes.item.value }}</div>
                    </template>
                    </el-autocomplete>
                    </el-card>
                </el-form-item>

                <el-form-item>
                    <el-card class="box-card" shadow="hover">
                    <div slot="header" class="clearfix">
                    <span id="spanName">事项内容(待遇标准)</span>
                    </div>
                    <el-input type="textarea" v-model="form.content"></el-input>
                    </el-card>
                </el-form-item>
                <div><br />
                    <h4 style="font-size: 30px; font-weight: bold">政策依据(文件名、文号)</h4>
                </div>
                <el-form-item>
                    <el-card class="box-card">
                    <div slot="header" class="clearfix">
                    <span id="spanName">文件名</span>
                    </div>
                    <el-autocomplete
                    popper-class="form-autocomplete"
                    v-model="form.fileName"
                    :fetch-suggestions="queryFormFileNameSearch"
                    placeholder="请输入文件名"
                    @select="handleSelect"
                    >
                    <i
                    class="el-icon-edit el-input__icon"
                    slot="suffix"
                    @click="handleIconClick"></i>
                    <template slot-scope="formFileNames">
                    <div class="name">{{ formFileNames.item.value }}</div>
                    </template>
                    </el-autocomplete>
                    </el-card>
                </el-form-item>

                <el-form-item>
                    <el-card class="box-card" shadow="hover">
                    <div slot="header" class="clearfix">
                    <span id="spanName">文号</span>
                    </div>
                    <el-autocomplete
                    popper-class="form-autocomplete"
                    v-model="form.number"
                    :fetch-suggestions="queryNumberSearch"
                    placeholder="请输入文号"
                    @select="handleSelect"
                    >
                    <i
                    class="el-icon-edit el-input__icon"
                    slot="suffix"
                    @click="handleIconClick"></i>
                    <template slot-scope="numbers">
                    <div class="name">{{ numbers.item.value }}</div>
                    </template>
                    </el-autocomplete>
                    </el-card>
                </el-form-item>
            </div>
    
            <!--2-->
            <div class="info" v-if="active==2">
    
                <el-form-item>
                    <el-card class="box-card" shadow="hover">
                    <div slot="header" class="clearfix">
                    <span id="spanName">申办所需资格条件</span>
                    </div>
                    <div v-for="(conList,index) in form.condition">
                        <el-input type="text" v-model="conList.con">
                            <el-button slot="append" icon="el-icon-delete" @click="delcon(index)"></el-button>
                        </el-input>
                    </div>                    
                    <el-button type="primary" @click="addcon">添加条件</el-button>
                    </el-card>
                </el-form-item>

                <el-form-item>
                    <el-card class="box-card" shadow="hover">
                    <div slot="header" class="clearfix">
                    <span id="spanName">申办材料</span>
                    </div>
                    <div v-for="(paperList,index) in form.paper">
                        <el-input type="text" v-model="paperList.pap">
                            <el-button slot="append" icon="el-icon-delete" @click="delpap(index)"></el-button>
                        </el-input><br/>
                    </div>
                    <el-button type="primary" @click="addpap">添加材料</el-button>
                    </el-card>
                </el-form-item>

                <el-form-item>
                    <el-card class="box-card" shadow="hover">
                    <div slot="header" class="clearfix">
                    <span id="spanName">审核时限</span>
                    </div>
                    <el-select v-model="form.date" placeholder="请选择">
                        <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value" >
                        </el-option>
                    </el-select>
                    </el-card>
                </el-form-item>
            </div>
    
            <!--3-->
            <div class="info" v-if="active==3">
                
                <el-form-item>
                    <el-card class="box-card" shadow="hover">
                    <div slot="header" class="clearfix">
                    <span id="spanName">网络咨询平台(点击上传图片)</span>
                    </div>
                    <el-upload class="avatar-uploader" action="https://jsonplaceholder.typicode.com/posts/" :show-file-list="false" :on-success="handleAvatarSuccess" :on-change="onchange" :before-upload="beforeAvatarUpload"> <img v-if="imageUrl" :src="imageUrl" class="avatar"> <i v-else class="el-icon-plus avatar-uploader-icon"></i> </el-upload>
                    </el-card>
                </el-form-item>

                <el-form-item>
                    <el-card class="box-card" shadow="hover">
                    <div slot="header" class="clearfix">
                    <span id="spanName">咨询电话</span>
                    </div>
                    <div v-for="(list,index) in form.phoneNumber">
                        <el-input v-model="list.a">
                            <el-button slot="append" icon="el-icon-delete" @click="del(index)"></el-button>
                        </el-input> <br/>
                    </div>
                    <el-button type="primary" @click="add">添加电话</el-button>
                    </el-card>
                </el-form-item>
            </div>
    
    
            <!--4-->
            <div class="info" v-if="active==4">
    
                <el-form-item>
                    <el-card class="box-card" shadow="hover">
                    <div slot="header" class="clearfix">
                    <span id="spanName">业务办理二维码(点击上传图片)</span>
                    </div>
                    <el-upload class="avatar-uploader" action="https://jsonplaceholder.typicode.com/posts/" :show-file-list="false" :on-success="handleAvatarSuccess2" :on-change="onchange2" :before-upload="beforeAvatarUpload"> <img v-if="imageUrl2" :src="imageUrl2" class="avatar"> <i v-else class="el-icon-plus avatar-uploader-icon"></i> </el-upload>
                    </el-card>
                </el-form-item>

                <el-form-item>
                    <el-card class="box-card" shadow="hover">
                    <div slot="header" class="clearfix">
                    <span id="spanName">办事大厅地址</span>
                    </div>
                    <div v-for="(List,index) in form.address">
                        <el-input type="text" v-model="List.b">
                            <el-button slot="append" icon="el-icon-delete" @click="del2(index)"></el-button>
                        </el-input><br/>
                    </div>
                    <el-button type="primary" @click="add2">添加地址</el-button>
                    </el-card>
                </el-form-item>
                <div><br/>
                    <el-button type="primary" @click="onSubmit1">保存提交</el-button>
                </div>
            </div>
    
    
    
            <el-button style="margin-top: 12px;" @click="pre" v-if="active>1">返回上一步</el-button>
            <el-button style="margin-top: 12px;" @click="next" v-if="active<4">保存并继续设置</el-button>
        </el-form>
    </div>
</template>

<script>
import imageConversion from 'image-conversion'
export default {

    data() {
        return {
            dialogImageUrl: '',
            imageUrl: '',
            imageUrl2: '',
            dialogVisible: false,
            visible: false,
            active: 1,
            options: [{
              label:'法定办结时限：60个工作日',
              value:'60'
            },
            {
              label:'承诺办结时限：40个工作日',
              value:'40'
            }
            ],

            form: {
                name: '',
                phoneNumber: [],
                address: [],
                code: '',
                content: '',
                fileName: '',
                number: '',
                condition: [],
                time: '',
                paper: [],
                date:'',
            },

            formNames: [],  //记录cookie中的事项名称
            formCodes: [],  //记录cookie中的事项代码
            formFileNames: [],  //记录cookie中的文件名
            numbers: [],    //记录cookie中的文号
        }
    },
    methods: {
        onSubmit1: function() {
            console.log("点击成功")
        },
        add: function() {
            let cope = {
                a: "",
            }
            this.form.phoneNumber.push(cope);
            console.log(this.form.phoneNumber)
        },
        del: function(index) {
            this.form.phoneNumber.splice(index, 1);
            console.log(this.form.phoneNumber)
        },

        add2: function() {
            let cope = {
                b: "",
            }
            this.form.address.push(cope);
            console.log(this.form.address)
        },
        del2: function(index) {
            this.form.address.splice(index, 1);
            console.log(this.form.address)
        },

        addcon: function() {
            let cope = {
                con: "",
            }
            this.form.condition.push(cope);
            console.log(this.form.condition)
        },
        delcon: function(index) {
            this.form.condition.splice(index, 1);
            console.log(this.form.condition)
        },

        addpap: function() {
            let cope = {
                pap: "",
            }
            this.form.paper.push(cope);
            console.log(this.form.paper)
        },
        delpap: function(index) {
            this.form.paper.splice(index, 1);
            console.log(this.form.paper)
        },



        handleRemove(file, fileList) {
            console.log(file, fileList);
        },




        handleAvatarSuccess(res, file) {

            this.imageUrl = URL.createObjectURL(file.raw);
        },


        beforeAvatarUpload(file) {
            const isJPG = file.type === 'image/jpeg';
            const isLt5M = file.size / 1024 / 1024 < 5;
            const is512K = file.size / 1024 < 520;
            if (!isJPG) {
                this.$message.error('上传图片只能是 JPG 格式!');
            }
            if (!isLt5M) {
                this.$message.error('上传图片大小不能超过 5MB!');
            } else return new Promise((resolve, reject) => {
                if (is512K) {
                    resolve(file)
                }
                console.log(file)
                imageConversion.compressAccurately(file, 500).then(res => { // console.log(res)
                    resolve(res)
                })
            })
        },

        onchange(file, fileList) {
            var _this = this;
            var event = event || window.event;
            var file = event.target.files[0];
            var reader = new FileReader();
            //转base64
            reader.onload = function(e) {
                _this.imageUrl = e.target.result //将图片路径赋值给src
            }
            reader.readAsDataURL(file);
        },

        onchange2(file, fileList) {
            var _this = this;
            var event = event || window.event;
            var file = event.target.files[0];
            var reader = new FileReader();
            //转base64
            reader.onload = function(e) {
                _this.imageUrl2 = e.target.result //将图片路径赋值给src
            }
            reader.readAsDataURL(file);
        },


        handleAvatarSuccess2(res, file) {
            this.imageUrl2 = URL.createObjectURL(file.raw);
        },



        next() {
            if (this.active > 4) this.active = 1;
            if(this.active == 1){
                this.$options.methods.setCookie("formNames", this.form.name, 1, this.formNames);
                this.$options.methods.setCookie("formCodes", this.form.code, 1, this.formCodes);
                this.$options.methods.setCookie("formFileNames", this.form.fileName, 1, this.formFileNames);
                this.$options.methods.setCookie("numbers", this.form.number, 1, this.numbers);
            }
            this.active ++;
        },
        pre() {
            if (this.active-- < 1) this.active = 1;
        },

        //判断项目名称输入框的输入内容筛选列表数据
        queryFormNameSearch(queryString, cb) {
            var formNames = this.formNames;
            var results = queryString ? formNames.filter(this.createFilter(queryString)) : formNames;
            // 调用 callback 返回建议列表的数据
            cb(results);
        },

        queryFormCodeSearch(queryString, cb) {
            var formNames = this.formCodes;
            var results = queryString ? formNames.filter(this.createFilter(queryString)) : formNames;
            // 调用 callback 返回建议列表的数据
            cb(results);
        },

        queryFormFileNameSearch(queryString, cb) {
            var formNames = this.formFileNames;
            var results = queryString ? formNames.filter(this.createFilter(queryString)) : formNames;
            // 调用 callback 返回建议列表的数据
            cb(results);
        },

        queryNumberSearch(queryString, cb) {
            var formNames = this.numbers;
            var results = queryString ? formNames.filter(this.createFilter(queryString)) : formNames;
            // 调用 callback 返回建议列表的数据
            cb(results);
        },

        //筛选输入数据的列表
        createFilter(queryString) {
            return (formName) => {
                return (formName.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0);
            };
        },

        //设置浏览器保存输入的近期记录
        setCookie(cname, cvalue, exdays, formNames){
            if(cvalue == null || cvalue == "" || cvalue == undefined)return;
            console.log(cvalue);
            cvalue = "--" + cvalue;
            if(formNames.length == 10){
                let sign = Math.floor(Math.random()*10);
                for(var i=0; i<10; i++){
                    if(i != sign){ cvalue = cvalue + "--" + formNames[i].value; }
                }
            }else{
                for(var i=0; i<formNames.length; i++){
                    cvalue = cvalue + "--" + formNames[i].value;
                }
            }

            var d = new Date();
            d.setTime(d.getTime() + (exdays*24*60*60*1000));
            var expires = "expires="+d.toGMTString();
            document.cookie = cname + "=" + cvalue + ";" + expires + ";path=/";
        },

        //找到对应cookie值
        getCookie(cname){
            var cookie = document.cookie.split("; ");
            var name = cname + "=";
            for(var i=0; i<cookie.length; i++){
                if(cookie[i].indexOf(name) == 0){ cookie = cookie[i]; break;}
            }
            if(i == cookie.length)
                return [];
            cookie = cookie.split("--");
            var list = [];
            for(var i=1; i<cookie.length; i++){
                list.push({"value": cookie[i]});
            }
            return list;
        },


        handleSelect(item) {
            console.log(item);
        },


        handleIconClick(ev) {
            console.log(ev);
        },
    },
        
    mounted() {
        this.formNames = this.getCookie("formNames");
        this.formCodes = this.getCookie("formCodes");
        this.formFileNames = this.getCookie("formFileNames");
        this.numbers = this.getCookie("numbers");
        // console.log(this.formNames);
    }

}
</script>

<style>
#app{
    width: 90%;
    margin: 0px auto;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;

}

#spanName{
    font-size: 25px;
    font-weight: bold;
    color: #409EFF;
}

.el-select{
    width: 100%
}

.el-steps.el-steps--horizontal{
    margin-top: 30px;
    margin-bottom: 30px;
}

.el-step__line{
    height: 200px;
}

.el-step__icon.is-text{
    width: 44px;
    height: 44px;
}

.el-form-item__content{
    margin-right: 80px;
}

.el-autocomplete{
    width: 100%;
}


.el-card{
    border: 2px solid #409EFF;
}

.avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
}

.avatar-uploader .el-upload:hover {
    border-color: #409EFF;
}

.avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
}

.avatar {
    width: 178px;
    height: 178px;
    display: block;
}

.my-autocomplete {
  li {
    line-height: normal;
    padding: 7px;

    .name {
      text-overflow: ellipsis;
      overflow: hidden;
    }
    .addr {
      font-size: 12px;
      color: #b4b4b4;
    }

    .highlighted .addr {
      color: #ddd;
    }
  }
}
</style>
