<template>
  <el-dialog :append-to-body="true" :close-on-click-modal="false" :before-close="cancel" :visible.sync="dialog" :title="isAdd ? '新增' : '编辑'" width="500px" >
    <el-form ref="form" :model="form" :rules="rules" size="small" label-width="80px">
      <el-form-item label="用户昵称">
        <el-input v-model="form.nickname" style="width: 370px;" :disabled="true"  />
      </el-form-item>
      <el-form-item label="用户等级">
        <el-select placeholder="请选择" v-model="form.levelName" @change="getUserLevelId($event)">
          <el-option
            v-for="item in nameList"
            :key="item.id"
            :label="item.name"
            :value="item.id">
          </el-option>
        </el-select>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button type="text" @click="cancel">取消</el-button>
      <el-button :loading="loading" type="primary" @click="doSubmit">确认</el-button>
    </div>
  </el-dialog>
</template>

<script>
import { add, edit, userLevel, submitUserLevel } from '@/api/yxUser'
export default {
  props: {
    isAdd: {
      type: Boolean,
      required: true
    }
  },
  data() {
    return {
      loading: false, 
      dialog: false,
      form: {
        nickname:"",
        name:'',
        uid:'',
        account: '',
        pwd: '',
        realName:'',
        birthday: '',
        cardId: '',
        mark: '',
        partnerId: '',
        groupId: '',
        nickname: '',
        avatar: '',
        phone: '',
        addTime: '',
        addIp: '',
        lastTime: '',
        lastIp: '',
        nowMoney: '',
        brokeragePrice: '',
        integral: '',
        signNum: '',
        status:'',
        level: '',
        spreadUid: '',
        spreadTime: '',
        userType: '',
        isPromoter:'',
        payCount: '',
        spreadCount: '',
        cleanTime: '',
        addres: '',
        adminid: '',
        loginType: '',
        levelName:'',
        mylevelId:''
      },
      nameList: [],
      value: '',
      rules: {
      }
    }
  },
  created(){
    this.getUserLevel();
    console.log('aaaaaaaaaaa')
    console.log(this.form)
  },
  methods: {
    //取消
    cancel() {
      this.resetForm()
    },
    //提交
    doSubmit(){
      console.log('guanbb')
      this.loading = true;
      submitUserLevel(this.form).then(res => {
        console.log(res)
        this.dialog = false;
        this.loading = false;
        this.$parent.init();
      })
    },
    //取消
    resetForm() {
      this.dialog = false
      this.$refs['form'].resetFields()
      this.form = {
        uid: '',
        account: '',
        pwd: '',
        realName: '',
        birthday: '',
        cardId: '',
        mark: '',
        partnerId: '',
        groupId: '',
        nickname: '',
        avatar: '',
        phone: '',
        addTime: '',
        addIp: '',
        lastTime: '',
        lastIp: '',
        nowMoney: '',
        brokeragePrice: '',
        integral: '',
        signNum: '',
        status: '',
        level: '',
        spreadUid: '',
        spreadTime: '',
        userType: '',
        isPromoter: '',
        payCount: '',
        spreadCount: '',
        cleanTime: '',
        addres: '',
        adminid: '',
        loginType: ''
      }
    },
    //等级列表
    getUserLevel(){
      userLevel({page:1,size:100}).then(res => {
        console.log(res)
        this.nameList = res.content;
      });
    },
    //获取等级id
    getUserLevelId(event){
      console.log(event)
      this.form.mylevelId = event;
      let obj = {};
      obj = this.nameList.find((item)=>{
          return item.id === event;
      });
      this.form.levelName = obj.name;
      console.log(obj.name);

    }
  }
}
</script>

<style scoped>

</style>
