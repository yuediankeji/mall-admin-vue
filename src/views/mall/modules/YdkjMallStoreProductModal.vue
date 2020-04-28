<template>
  <a-drawer
    :title="title"
    :width="width"
    placement="right"
    :closable="false"
    @close="close"
    :visible="visible">
  
    <a-spin :spinning="confirmLoading">
      <a-form :form="form">

        <a-form-item label="商户Id(0为总后台管理员创建,不为0的时候是商户后台创建)" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'merId', validatorRules.merId]" placeholder="请输入商户Id(0为总后台管理员创建,不为0的时候是商户后台创建)" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="商品图片" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'image', validatorRules.image]" placeholder="请输入商品图片"></a-input>
        </a-form-item>
        <a-form-item label="轮播图" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'sliderImage', validatorRules.sliderImage]" placeholder="请输入轮播图"></a-input>
        </a-form-item>
        <a-form-item label="商品名称" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'storeName', validatorRules.storeName]" placeholder="请输入商品名称"></a-input>
        </a-form-item>
        <a-form-item label="商品简介" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'storeInfo', validatorRules.storeInfo]" placeholder="请输入商品简介"></a-input>
        </a-form-item>
        <a-form-item label="关键字" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'keyword', validatorRules.keyword]" placeholder="请输入关键字"></a-input>
        </a-form-item>
        <a-form-item label="产品条码（一维码）" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'barCode', validatorRules.barCode]" placeholder="请输入产品条码（一维码）"></a-input>
        </a-form-item>
        <a-form-item label="分类id" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'cateId', validatorRules.cateId]" placeholder="请输入分类id"></a-input>
        </a-form-item>
        <a-form-item label="商品价格" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'price', validatorRules.price]" placeholder="请输入商品价格" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="会员价格" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'vipPrice', validatorRules.vipPrice]" placeholder="请输入会员价格" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="市场价" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'otPrice', validatorRules.otPrice]" placeholder="请输入市场价" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="邮费" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'postage', validatorRules.postage]" placeholder="请输入邮费" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="单位名" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'unitName', validatorRules.unitName]" placeholder="请输入单位名"></a-input>
        </a-form-item>
        <a-form-item label="排序" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'sort', validatorRules.sort]" placeholder="请输入排序" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="销量" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'sales', validatorRules.sales]" placeholder="请输入销量" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="库存" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'stock', validatorRules.stock]" placeholder="请输入库存" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="状态（0：未上架，1：上架）" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'isShow', validatorRules.isShow]" placeholder="请输入状态（0：未上架，1：上架）" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="是否热卖" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'isHot', validatorRules.isHot]" placeholder="请输入是否热卖" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="是否优惠" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'isBenefit', validatorRules.isBenefit]" placeholder="请输入是否优惠" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="是否精品" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'isBest', validatorRules.isBest]" placeholder="请输入是否精品" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="是否新品" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'isNew', validatorRules.isNew]" placeholder="请输入是否新品" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="产品描述" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-textarea v-decorator="['description', validatorRules.description]" rows="4" placeholder="请输入产品描述"/>
        </a-form-item>
        <a-form-item label="添加时间" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'addTime', validatorRules.addTime]" placeholder="请输入添加时间" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="是否包邮" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'isPostage', validatorRules.isPostage]" placeholder="请输入是否包邮" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="是否删除" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'isDel', validatorRules.isDel]" placeholder="请输入是否删除" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="商户是否代理 0不可代理1可代理" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'merUse', validatorRules.merUse]" placeholder="请输入商户是否代理 0不可代理1可代理" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="获得积分" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'giveIntegral', validatorRules.giveIntegral]" placeholder="请输入获得积分" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="成本价" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'cost', validatorRules.cost]" placeholder="请输入成本价" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="秒杀状态 0 未开启 1已开启" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'isSeckill', validatorRules.isSeckill]" placeholder="请输入秒杀状态 0 未开启 1已开启" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="砍价状态 0未开启 1开启" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'isBargain', validatorRules.isBargain]" placeholder="请输入砍价状态 0未开启 1开启" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="是否优品推荐" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'isGood', validatorRules.isGood]" placeholder="请输入是否优品推荐" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="虚拟销量" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'ficti', validatorRules.ficti]" placeholder="请输入虚拟销量" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="浏览量" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input-number v-decorator="[ 'browse', validatorRules.browse]" placeholder="请输入浏览量" style="width: 100%"/>
        </a-form-item>
        <a-form-item label="产品二维码地址(用户小程序海报)" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'codePath', validatorRules.codePath]" placeholder="请输入产品二维码地址(用户小程序海报)"></a-input>
        </a-form-item>
        <a-form-item label="淘宝京东1688类型" :labelCol="labelCol" :wrapperCol="wrapperCol">
          <a-input v-decorator="[ 'soureLink', validatorRules.soureLink]" placeholder="请输入淘宝京东1688类型"></a-input>
        </a-form-item>
        
      </a-form>
    </a-spin>
    <a-button type="primary" @click="handleOk">确定</a-button>
    <a-button type="primary" @click="handleCancel">取消</a-button>
  </a-drawer>
</template>

<script>

  import { httpAction } from '@/api/manage'
  import pick from 'lodash.pick'
  import { validateDuplicateValue } from '@/utils/util'
  
  export default {
    name: "YdkjMallStoreProductModal",
    components: { 
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
          merId: {rules: [
          ]},
          image: {rules: [
            {required: true, message: '请输入商品图片!'},
          ]},
          sliderImage: {rules: [
            {required: true, message: '请输入轮播图!'},
          ]},
          storeName: {rules: [
            {required: true, message: '请输入商品名称!'},
          ]},
          storeInfo: {rules: [
            {required: true, message: '请输入商品简介!'},
          ]},
          keyword: {rules: [
            {required: true, message: '请输入关键字!'},
          ]},
          barCode: {rules: [
          ]},
          cateId: {rules: [
            {required: true, message: '请输入分类id!'},
          ]},
          price: {rules: [
            {required: true, message: '请输入商品价格!'},
          ]},
          vipPrice: {rules: [
          ]},
          otPrice: {rules: [
          ]},
          postage: {rules: [
          ]},
          unitName: {rules: [
          ]},
          sort: {rules: [
          ]},
          sales: {rules: [
          ]},
          stock: {rules: [
          ]},
          isShow: {rules: [
          ]},
          isHot: {rules: [
          ]},
          isBenefit: {rules: [
          ]},
          isBest: {rules: [
          ]},
          isNew: {rules: [
          ]},
          description: {rules: [
          ]},
          addTime: {rules: [
          ]},
          isPostage: {rules: [
          ]},
          isDel: {rules: [
          ]},
          merUse: {rules: [
          ]},
          giveIntegral: {rules: [
          ]},
          cost: {rules: [
          ]},
          isSeckill: {rules: [
          ]},
          isBargain: {rules: [
          ]},
          isGood: {rules: [
          ]},
          ficti: {rules: [
          ]},
          browse: {rules: [
          ]},
          codePath: {rules: [
            {required: true, message: '请输入产品二维码地址(用户小程序海报)!'},
          ]},
          soureLink: {rules: [
          ]},
        },
        url: {
          add: "/org.jeecg.modules/ydkjMallStoreProduct/add",
          edit: "/org.jeecg.modules/ydkjMallStoreProduct/edit",
        }
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
          this.form.setFieldsValue(pick(this.model,'merId','image','sliderImage','storeName','storeInfo','keyword','barCode','cateId','price','vipPrice','otPrice','postage','unitName','sort','sales','stock','isShow','isHot','isBenefit','isBest','isNew','description','addTime','isPostage','isDel','merUse','giveIntegral','cost','isSeckill','isBargain','isGood','ficti','browse','codePath','soureLink'))
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
        this.form.setFieldsValue(pick(row,'merId','image','sliderImage','storeName','storeInfo','keyword','barCode','cateId','price','vipPrice','otPrice','postage','unitName','sort','sales','stock','isShow','isHot','isBenefit','isBest','isNew','description','addTime','isPostage','isDel','merUse','giveIntegral','cost','isSeckill','isBargain','isGood','ficti','browse','codePath','soureLink'))
      }
      
    }
  }
</script>

<style lang="less" scoped>
/** Button按钮间距 */
  .ant-btn {
    margin-left: 30px;
    margin-bottom: 30px;
    float: right;
  }
</style>