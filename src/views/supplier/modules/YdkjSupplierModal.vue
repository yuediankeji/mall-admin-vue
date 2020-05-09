<template>
  <a-modal
    :title="title"
    :width="width"
    :visible="visible"
    :confirmLoading="confirmLoading"
    @ok="handleOk"
    @cancel="handleCancel"
    cancelText="关闭">
    <a-spin :spinning="confirmLoading">
      <a-form :form="form">

        <a-form-item label="供应商" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'supplier', validatorRules.supplier]" placeholder="请输入供应商"></a-input>
        </a-form-item>
        <a-form-item label="公司名称" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'company', validatorRules.company]" placeholder="请输入公司名称"></a-input>
        </a-form-item>
        <a-form-item label="公司联系方式" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'companyLink', validatorRules.companyLink]" placeholder="请输入公司联系方式"></a-input>
        </a-form-item>
        <a-form-item label="公司地址" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'companyAddess', validatorRules.companyAddess]" placeholder="请输入公司地址"></a-input>
        </a-form-item>
        <a-form-item label="选择用户" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-select v-decorator="['realName', validatorRules.realName]" show-search placeholder="选择用户" :default-active-first-option="false" :show-arrow="false" :filter-option="false" :not-found-content="null" @search="handleSearch"
            @change="handleChange">
            <a-select-option v-for="d in userList" :key="d.id">
              {{ d.realname }}
            </a-select-option>
          </a-select>
        </a-form-item>

      </a-form>
    </a-spin>
  </a-modal>
</template>

<script>

  import { httpAction } from '@/api/manage'
  import pick from 'lodash.pick'
  import querystring from 'querystring';
  import { validateDuplicateValue } from '@/utils/util'
  import JDate from '@/components/jeecg/JDate'  
  import {
    appUserList
  } from '@/api/api'

let timeout;
let currentValue;
  function fetch(value, callback) {
    if (timeout) {
      clearTimeout(timeout);
      timeout = null;
    }
    currentValue = value;
    function fake() {
      appUserList({
        keyword: value,
        pageNo: 1,
        pageSize: 30
      }).then((res) => {
        if (res.success) {
          var data = res.result.records;
          callback(data)
        }
      })
    }
    timeout = setTimeout(fake, 300);
  }
  export default {
    name: "YdkjSupplierModal",
    components: { 
      JDate,
    },
    data () {
      return {
        form: this.$form.createForm(this),
        title:"操作",
        width:800,
        visible: false,
        model: {},
        labelCol: {
          xs: { span: 24 },
          sm: { span: 5 },
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 },
        },
        confirmLoading: false,
        validatorRules: {
          supplier: {
            rules: [{
              required: true,
              message: '供应商不能为空',
              trigger: 'blur'
            }]
          },
          company: {
            rules: [{
              required: true,
              message: '公司名不能为空',
              trigger: 'blur'
            }]
          },
          companyLink: {
            rules: [{
              required: true,
              message: '公司联系方式不能为空',
              trigger: 'blur'
            }]
          },
          companyAddess: {
            rules: [{
              required: true,
              message: '公司地址不能为空',
              trigger: 'blur'
            }]
          },
          realName: {
            rules: [{
              required: true,
              message: '用户不能为空',
              trigger: 'blur'
            }]
          },
        },
        url: {
          add: "/supplier/ydkjSupplier/add",
          edit: "/supplier/ydkjSupplier/edit",
        },
        userValue: undefined,
        userList: []
      }
    },
    created () {
    },
    methods: {
      add () {
        this.edit({});
      },
      edit (record) {
        this.form.resetFields();
        this.model = Object.assign({}, record);
        this.visible = true;
        this.$nextTick(() => {
          this.form.setFieldsValue(pick(this.model,'supplier','company','companyLink','companyAddess','userId','userName','realName'))
        })
      },
      close () {
        this.$emit('close');
        this.visible = false;
      },
      handleOk () {
        const that = this;
        // 触发表单验证
        this.form.validateFields((err, values) => {
          if (!err) {
            that.confirmLoading = true;
            let httpurl = '';
            let method = '';
            if(!this.model.id){
              httpurl+=this.url.add;
              method = 'post';
            }else{
              httpurl+=this.url.edit;
               method = 'put';
            }
            let formData = Object.assign(this.model, values);
            console.log("表单提交数据",formData)
            httpAction(httpurl,formData,method).then((res)=>{
              if(res.success){
                that.$message.success(res.message);
                that.$emit('ok');
              }else{
                that.$message.warning(res.message);
              }
            }).finally(() => {
              that.confirmLoading = false;
              that.close();
            })
          }
         
        })
      },
      handleCancel () {
        this.close()
      },
      popupCallback(row){
        this.form.setFieldsValue(pick(row,'supplier','company','companyLink','companyAddess','userId','userName','realName'))
      },
      handleSearch(value) {
        fetch(value, data => (this.userList = data));
      },
      handleChange(value) {
        console.log(value);
        this.model.userId = value;
        fetch(value, data => (this.userList = data));
      },

      
    }
  }
</script>