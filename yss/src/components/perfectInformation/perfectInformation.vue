<template>
  <div class="perfectInformation-wrapper">
    <div class="resetPwd-content">
      <div class="process-wrapper">
        <div class="process-1 active">
          1、基本信息
          <div class="triangle">
            <span></span>
          </div>
        </div>
        <div class="process-2 active">
          2、手机验证
          <div class="triangle">
            <span></span>
          </div>
        </div>
        <div class="process-3 active">
          3、完善资料
        </div>
      </div>
      
      <div class="tips">您通过注册及验证手机号，已经获得20元积分的奖励；完善以下资料，您将额外获得10元积分奖励，并且只有通过医师认证后， 您才能参加医学调查和兑换礼品。</div>     

      <div class="thanks"><i>//</i>请填写与您工作有关的信息，以获得更加适合您的调查<i>//</i></div>
      
      <el-form ref="form" :model="form" :rules="rules" label-width="150px" :inline-message='true' status-icon> 
        <el-form-item label="您的性别" required prop="sex">
          <el-radio-group v-model="form.sex">
            <el-radio label="0">男</el-radio>
            <el-radio label="1">女</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="出生年份" required prop="birth_year">
          <el-date-picker v-model="form.birth_year" type="year" placeholder="请选择出生年份" class="yearSelect" value-format="yyyy" ></el-date-picker>
        </el-form-item>
        <el-form-item label="加入的学会" prop="meeting">
          <el-select v-model="form.meeting" multiple placeholder="请填写您参加过的学会" class="meetingInput">
            <el-option v-for="item in meeting_options" :label="item.societyname" :key="item.id" :value="item.id"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="最高学历" required prop="level">
          <el-select v-model="form.level" placeholder="请选择最高学历" class="levelSelect">
            <el-option v-for="item in level_options" :label="item.sysname" :key="item.id" :value="item.id"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="毕业时间" required prop="graduation_time">
          <el-date-picker v-model="form.graduation_time" type="year" placeholder="选择日期" class="graduationTimeSelect" value-format="yyyy"></el-date-picker>
        </el-form-item> 
        <el-form-item label="毕业院校" required prop="school">
          <el-select placeholder="请选择省份" class="provinceSelect" v-model="form.province" @change="getGraduateList(form.province)">
            <el-option v-for="item in province_options" :label="item.provinceName" :key="item.provinceId" :value="item.provinceId"></el-option>
          </el-select>
          <el-select placeholder="请选择学校" class="schoolSelect" v-model="form.school">
            <el-option v-for="item in school_options" :label="item.graduatename" :key="item.graduateid" :value="item.graduateid"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="证书认证方式" required>
          <!--<el-radio-group v-model="form.authentication_type">-->
          <el-checkbox-group v-model="form.authentication_type">
            <el-checkbox label="1">上传执业证书</el-checkbox>
            <el-checkbox label="2">科室电话验证</el-checkbox>
            <!--<el-radio :label="1">上传执业证书</el-radio>-->
            <!--<el-radio :label="2">科室电话验证</el-radio>-->
          </el-checkbox-group>          
        </el-form-item>
        <div v-if="form.authentication_type.indexOf('1') != -1">
          <el-form-item label="执业证类型" prop="membercertificatetype">
            <el-select class="professionalSelect" placeholder="请选择执业证类型" v-model="form.membercertificatetype">
              <el-option label="医师资格证" value="1"></el-option>
              <el-option label="医师执业证" value="2"></el-option>
              <el-option label="护士资格证" value="3"></el-option>
              <el-option label="药师资格证" value="4"></el-option>
              <el-option label="药师执业证" value="5"></el-option>
              <el-option label="护士执业证" value="8"></el-option>
            </el-select>
          </el-form-item> 
          <el-form-item label="执业证书编号" prop="certificate_num">
            <el-input v-model="form.certificate_num" placeholder="请填写执业证书编号" class="certificateInput"></el-input>
          </el-form-item>
          <el-form-item label="执业证书第一页" required>
            <el-upload
              class="upload-demo"
              ref="upload1"
              action="123"
              :auto-upload=false
              :before-upload="beforeUpload1"
              :on-change="newhandleChange1"
              :on-success="newhandlesuccess1"
              :on-exceed="handleExceed1"
              :limit="1"
              accept="image/jpeg,image/png">
              <el-button size="small" type="primary" class="btn-upload">点击上传</el-button>
              <a href="" class="btn-example" target="_blank">查看示例图</a>
              <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过5MB</div>
            </el-upload>            
          </el-form-item>
          <el-form-item label="执业证书第二页" required>
            <el-upload
              class="upload-demo"
              ref="upload2"
              action="123"
              :auto-upload=false
              :before-upload="beforeUpload2"
              :on-change="newhandleChange2"
              :on-success="newhandlesuccess2"
              :on-exceed="handleExceed2"
              :limit="1"
              accept="image/jpeg,image/png">
              <el-button size="small" type="primary" class="btn-upload">点击上传</el-button>
              <a href="" class="btn-example" target="_blank">查看示例图</a>
              <div slot="tip" class="el-upload__tip">只能上传jpg/png文件，且不超过5MB</div>
            </el-upload>            
          </el-form-item>
        </div>
        <div v-if="form.authentication_type.indexOf('2') != -1">
          <el-form-item label="科室电话" required>
            <el-form-item prop="region_number">
              <el-input placeholder="区号" type="number" v-model="form.region_number" class="regionnumInput"></el-input>              
            </el-form-item>
            <el-form-item prop="phone_number">
              <el-input placeholder="电话号码" v-model="form.phone_number" class="phonenumInput"></el-input>
            </el-form-item>
            <el-form-item prop="room_number">
              <el-input placeholder="分机号" v-model="form.room_number" class="roomnumInput"></el-input>
            </el-form-item>              
            
            <div class="phone-tips">如：010-55555555转606，请详细填写所在医院的固定电话区号-电话及分机号，请确定能联系到您。我们将拨打此电话，核实是否是本人注册。</div>
            <div class="phone-warning">注：此电话不会被公开</div>
          </el-form-item>
          <el-form-item label="接听电话时间" prop="end_time" required>
            <el-time-select
              placeholder="请选择起始时间"
              class="startTime"
              v-model="form.start_time"
              :picker-options="{
                start: '06:00',
                step: '01:00',
                end: '18:00',
              }">
            </el-time-select>
            <span style="margin: 0 12px;">到</span>
            <el-time-select
              placeholder="请选择结束时间"
              class="endTime"
              v-model="form.end_time"
              :picker-options="{
                start: '06:00',
                step: '01:00',
                end: '18:00',
                minTime: form.start_time
              }">
            </el-time-select>
          </el-form-item>
        </div>
        <el-form-item>
          <el-button class="btn-complete" @click="onSubmit('form')">完成完善资料</el-button>
        </el-form-item> 
      </el-form>
    </div>    
  </div>
</template>

<script>
  export default {
    data() {
      var checkTime = (rule, value, callback) => {
        console.log(value);
        console.log(this.form.start_time);
        if(this.form.start_time == null){
          callback(new Error('请选择接听电话开始时间'));
        }else{
          callback();
        }
      };
      return {
        form: {
          sex: null,//性别
          birth_year:null,//出生年份
          meeting:'',//加入的学会
          level:null,//最高学历
          graduation_time:null,//毕业时间
          authentication_type:['1','2'],//证书认证方式
          certificate_num:null,//执业证书编号
          school:null,
          province:null,
          region_number:null,
          phone_number:null,
          room_number:null,
          start_time: null,
          end_time: null,
          fileParam_1:null,
          fileParam_2:null,
          membercertificatetype:null,
        },
        uploadForm: new FormData(),
        fileList:[],
        meeting_options:[],
        level_options:[],
        province_options:[],
        school_options:[],
        rules: {
          sex: [
            { required: true, message: '请选择性别', trigger: 'blur' },
          ],
          birth_year: [
            { required: true, message: '请选择出生年份', trigger: 'change' }
          ],
          meeting:[
            { required: true, message: '请选择加入的学会', trigger: 'blur' }
          ],
          level:[
            { required: true, message: '请选择最高学历', trigger: 'change' },
          ],
          graduation_time:[
            { required: true, message: '请选择毕业时间', trigger: 'change' },
          ],
          school:[
            { required: true, message: '请选择毕业院校', trigger: 'change' },
          ],
          region_number:[
            { required: true, message: '请输入区号', trigger: 'blur' },
            { min: 3, message: '请输入正确的区号', trigger: 'blur' }
          ],
          phone_number:[
            { required: true, message: '请输入科室电话', trigger: 'blur' },
            { min: 7, message: '请输入正确的科室电话', trigger: 'blur' }
          ],
          room_number:[
            { required: true, message: '请输入分机号', trigger: 'blur' },
            { min: 3, message: '请输入正确的分机号', trigger: 'blur' }
          ],
          end_time:[
            { required: true, message: '请选择接听电话时间', trigger: 'change' },
            { validator: checkTime, trigger: 'change' }
          ],
          certificate_num:[
            { required: true, message: '请输入执业证书编号', trigger: 'blur'},
            { min: 12, message: '请输入正确的执业证书编号', trigger: 'blur' }
          ],
          membercertificatetype:[
            { required: true, message: '请选择执业证类型', trigger: 'change'},
          ]
        }
      }
    },
    mounted(){
      this.getSociety();
      this.getEducational();
      this.getProvince();
    },
    methods: {
      onSubmit(formName) {
        console.log(this.form.authentication_type);
        this.$refs[formName].validate((valid) => {
          if (valid) {
            var checkmethod = null;
            var sex = null;
            if(this.form.authentication_type.length == 2){
              checkmethod = 0
            }else if(this.form.authentication_type.length == 1 && this.form.authentication_type[0] == '1'){
              checkmethod = 2
            }else if(this.form.authentication_type.length == 1 && this.form.authentication_type[0] == '2') {
              checkmethod = 1
            }
            
//          if(this.form.authentication_type == '1'){
//            checkmethod = 2
//          }else if(this.form.authentication_type == '2') {
//            checkmethod = 1
//          }            
            
            if(this.form.sex == '0'){
              sex = '男';
            }else{
              sex = '女';
            }
            
            if(checkmethod == 2){
              this.uploadForm.append('certificateOne', this.form.fileParam_1[0].raw);
              this.uploadForm.append('certificateTwo', this.form.fileParam_2[0].raw);
              this.uploadForm.append('memberidcard', this.form.certificate_num);                
              this.uploadForm.append('membercertificatetype',Number(this.form.membercertificatetype));
            }else if(checkmethod == 1){
              this.uploadForm.append('departmentstle', this.form.region_number+'-'+this.form.phone_number+'-'+this.form.room_number);
              this.uploadForm.append('beginTime', Number(this.form.start_time.slice(0,2)));
              this.uploadForm.append('endTime', Number(this.form.end_time.slice(0,2)));
            }else if(checkmethod == 0){
              this.uploadForm.append('certificateOne', this.form.fileParam_1[0].raw);
              this.uploadForm.append('certificateTwo', this.form.fileParam_2[0].raw);
              this.uploadForm.append('memberidcard', this.form.certificate_num);                
              this.uploadForm.append('membercertificatetype',Number(this.form.membercertificatetype));              
              this.uploadForm.append('departmentstle', this.form.region_number+'-'+this.form.phone_number+'-'+this.form.room_number);
              this.uploadForm.append('beginTime', Number(this.form.start_time.slice(0,2)));
              this.uploadForm.append('endTime', Number(this.form.end_time.slice(0,2)));              
            }
            this.uploadForm.append('memberSex', sex);
            this.uploadForm.append('memberBirYear', this.form.birth_year);
            this.uploadForm.append('societyid', this.form.meeting.join(','));
            this.uploadForm.append('memberEducation', Number(this.form.level));
            this.uploadForm.append('graduationTime', this.form.graduation_time);
            this.uploadForm.append('graduationInstitutions', this.form.school);
            this.uploadForm.append('checkmethod', Number(checkmethod));

            this.axios.post(this.common.getApi() + '/web/api/register/perfectInfo',this.uploadForm,{
              headers: {
                'Content-Type': 'multipart/form-data'
              }
            }).then((res) => {
              console.log(res)
              if(res.data.success){
                this.$message({
                  message: '提交成功',
                  type: 'success'
                });
                this.uploadForm = new FormData()
                this.$router.push('/index');
              }else{
                this.$message.error(res.data.msg);
                this.fileList = [];
//              this.form.fileParam_1 = '';
//              this.form.fileParam_2 = '';
                this.uploadForm = new FormData()
              }
            })            
          } else {
            console.log(this.form.sex)
            return false;
          }
        });
      },
      handleExceed1(files, fileList) {
        this.$message.warning(`最多只能选择1个文件`);
      },
      beforeUpload1(file){
        
      },
      newhandlesuccess1(file){
        console.log(file);
      },
      newhandleChange1(response,file,filelist){
        this.form.fileParam_1 = file;
        console.log(this.form.fileParam_1);
      },
      handleExceed2(files, fileList) {
        this.$message.warning(`最多只能选择1个文件`);
      },
      beforeUpload2(file){
        
      },
      newhandlesuccess2(file){
        console.log(file);
      },
      newhandleChange2(response,file,filelist){
        this.form.fileParam_2 = file;
        console.log(this.form.fileParam_2);
      },
      getSociety(){
        this.axios.get(this.common.getApi() + '/web/api/society/getSociety','',{
          headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
          }
        }).then((res) => {
          console.log(res);
          if(res.data.code == '200'){
            this.meeting_options = res.data.obj
          }
        })             
      },
      getEducational(){
        this.axios.get(this.common.getApi() + '/web/api/systemmaster/getEducational','',{
          headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
          }
        }).then((res) => {
          console.log(res);
          if(res.data.code == '200'){
            this.level_options = res.data.obj
          }
        })         
      },
      // 获取省份
      getProvince(){
        this.axios.get(this.common.getApi() + '/web/api/area/getProvince','',{
          headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
          }
        }).then((res) => {
          if(res.data.code == '200'){
            this.province_options = res.data.obj;
          }
        })        
      },
      getGraduateList(provinceId){
        this.school_options = null;
        this.form.school = null;
        this.axios.get(this.common.getApi() + '/web/api/graduate/getGraduateList',{
          params:{
            params:{
              provinceId: provinceId
            }
          }          
        },{
          headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
          }
        }).then((res) => {
          if(res.data.code == '200'){
            this.school_options = res.data.obj;
          }
        })        
      }
    }
  }  
</script>

<style>
  .perfectInformation-wrapper{
    width: 100%;
    background: #F6F6F6;
    padding-top: 20px;
    padding-bottom: 40px;
  }
  
  .perfectInformation-wrapper .resetPwd-content{
    width: 1180px;
    min-height: 640px;
    background: #FFF;
    padding-top: 40px; 
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    margin: 0px auto;
    padding: 0 200px;
    padding-top: 40px;
  }
  
  .perfectInformation-wrapper .process-wrapper{
    width: 780px;
    height: 48px;
    border: 1px solid #E6E6E6;
    margin: 0 auto;
    display: flex;
    line-height: 48px
  }
  
  .perfectInformation-wrapper .process-1,
  .perfectInformation-wrapper .process-2,
  .perfectInformation-wrapper .process-3{
    flex: 1;
    text-align: center;
    font-size: 14px;
    color: #333333;
    position: relative;
  }
  
  .perfectInformation-wrapper .triangle {
    width: 0;
    height: 0;
    border-width: 25px 0 25px 24px;
    border-style: solid;
    border-color: transparent transparent transparent #E6E6E6;
    position: absolute;
    top: -1px;
    right: -25px;
  }
  
  .perfectInformation-wrapper .triangle span{
    display: block;
    width: 0;
    height: 0;
    border-width: 25px 0 25px 24px;
    border-style: solid;
    border-color: transparent transparent transparent #fff;
    position: absolute;
    top: -25px;
    left: -26px;
  }  
  
  .perfectInformation-wrapper .process-wrapper .process-1.active,
  .perfectInformation-wrapper .process-wrapper .process-2.active,
  .perfectInformation-wrapper .process-wrapper .process-3.active{
    background: #13B5BB;
    color: #FFFFFF;
  }
  
  .perfectInformation-wrapper .process-wrapper .process-1.active .triangle span,
  .perfectInformation-wrapper .process-wrapper .process-2.active .triangle span{
    border-color: transparent transparent transparent #13B5BB;
  }
  
  .perfectInformation-wrapper .tips{
    font-weight: bold;
    font-size: 14px;
    color: #0C0C0C;
    padding: 29px 0;
    border-bottom: 1px solid #E6E6E6;
  }  
  
  .perfectInformation-wrapper .process-1 {
    z-index: 100;
  }  
  
  .perfectInformation-wrapper .process-2 {
    z-index: 99;
  }  
  
  .perfectInformation-wrapper .el-form{
    margin-top: 40px;
    border-bottom: 1px solid #E6E6E6;
  }
  
  .perfectInformation-wrapper .mobileInput{
    width: 400px;
  }
  
  .perfectInformation-wrapper .btn-complete{
    width: 400px;
    background: #13B5BB;
    color: #FFF;
    border: none;
    margin-top: 30px;
    margin-bottom: 80px;
  }
  
  .perfectInformation-wrapper .thanks{
    width:560px;
    height:40px;
    background: #13B5BB;
    line-height: 40px;
    text-align: center;
    color: #FFF;
    margin-top: 40px;
  }
  
  .perfectInformation-wrapper .thanks i{
    font-style: italic;
    margin: 0 10px
  }
  
  .perfectInformation-wrapper .yearSelect,
  .perfectInformation-wrapper .levelSelect,
  .perfectInformation-wrapper .graduationTimeSelect{
    width: 400px;
  }
  
  .perfectInformation-wrapper .meetingInput{
    width: 400px;
  }
  
  .perfectInformation-wrapper .btn-addData{
    background: #13B5BB;
    color: #FFFFFF;
    border: none;
    width: 40px;
    height: 40px;
    text-align: center;
    padding: 0;
    margin-left: 15px;
  }
  
  .perfectInformation-wrapper .provinceSelect,
  .perfectInformation-wrapper .citySelect,
  .perfectInformation-wrapper .regionSelect{
    width: 120px;
    margin-left: 17px;
  }
  
  .perfectInformation-wrapper .provinceSelect{
    margin-left: 0;
  }
  
  .perfectInformation-wrapper .schoolSelect {
    width: 276px;
  }
  
  .perfectInformation-wrapper .professionalSelect{
    width: 400px;
  }
  
  .perfectInformation-wrapper .certificateInput{
    width: 400px;
  }
  
  .perfectInformation-wrapper .btn-upload{
    background: #13B5BB;
    border: none;
  }
  
  .perfectInformation-wrapper .regionnumInput{
    width: 80px;
  }
  
  .perfectInformation-wrapper .phonenumInput{
    width: 200px;
    margin-top: 10px;
    /*margin-left: 17px;*/
  }  
  
  .perfectInformation-wrapper .roomnumInput{
    width: 80px;
    margin-top: 10px;
    /*margin-left: 17px;*/
  }    
  
  .perfectInformation-wrapper .phone-tips{
    line-height: 20px;
    color: #666666;
    width: 400px;
    font-size: 12px;
    margin: 10px 0;
  }
  
  .perfectInformation-wrapper .phone-warning{
    color: #FF6161;
    font-size: 12px;
  }
  
  .perfectInformation-wrapper .startTime,
  .perfectInformation-wrapper .endTime{
    width: 180px;
  }
  
  .perfectInformation-wrapper .upload-demo{
    width: 400px;
  }
  
  .perfectInformation-wrapper .el-upload{
    width: 100%;
    text-align: left;
  }
  
  .perfectInformation-wrapper .btn-example{
    color: #13B5BB;
    float: right;
  }
  
  .perfectInformation-wrapper .el-radio__input.is-checked .el-radio__inner{
    border-color: #13B5BB;
    background: #13B5BB;
  }
  
  .perfectInformation-wrapper .el-radio__input.is-checked+.el-radio__label {
    color: #13B5BB;
  } 
  
  .perfectInformation-wrapper .el-input input{
    background: #F9F9F9;
  } 
  
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
  }
 
  input[type="number"] {
    -moz-appearance: textfield;
  }
</style>