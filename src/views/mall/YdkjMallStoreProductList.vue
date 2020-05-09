<template>
  <a-card :bordered="false">
    <!-- 查询区域 -->
    <div class="table-page-search-wrapper">
      <a-form layout="inline" @keyup.enter.native="searchQuery">
        <a-row :gutter="24">

        </a-row>
      </a-form>
    </div>
    <!-- 查询区域-END -->
    
    <!-- 操作按钮区域 -->
    <div class="table-operator">
      <a-button @click="handleAdd" type="primary" icon="plus">新增</a-button>
      <a-button type="primary" icon="download" @click="handleExportXls('ydkj_mall_store_product')">导出</a-button>
      <a-upload name="file" :showUploadList="false" :multiple="false" :headers="tokenHeader" :action="importExcelUrl" @change="handleImportExcel">
        <a-button type="primary" icon="import">导入</a-button>
      </a-upload>
      <a-dropdown v-if="selectedRowKeys.length > 0">
        <a-menu slot="overlay">
          <a-menu-item key="1" @click="batchDel"><a-icon type="delete"/>删除</a-menu-item>
        </a-menu>
        <a-button style="margin-left: 8px"> 批量操作 <a-icon type="down" /></a-button>
      </a-dropdown>
    </div>

    <!-- table区域-begin -->
    <div>
      <div class="ant-alert ant-alert-info" style="margin-bottom: 16px;">
        <i class="anticon anticon-info-circle ant-alert-icon"></i> 已选择 <a style="font-weight: 600">{{ selectedRowKeys.length }}</a>项
        <a style="margin-left: 24px" @click="onClearSelected">清空</a>
      </div>

      <a-table
        ref="table"
        size="middle"
        bordered
        rowKey="id"
        :columns="columns"
        :dataSource="dataSource"
        :pagination="ipagination"
        :loading="loading"
        :rowSelection="{fixed:true,selectedRowKeys: selectedRowKeys, onChange: onSelectChange}"
        
        @change="handleTableChange">

        <template slot="htmlSlot" slot-scope="text">
          <div v-html="text"></div>
        </template>
        <template slot="imgSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无此图片</span>
          <img v-else :src="getImgView(text)" height="25px" alt="图片不存在" style="max-width:80px;font-size: 12px;font-style: italic;"/>
        </template>
        <template slot="fileSlot" slot-scope="text">
          <span v-if="!text" style="font-size: 12px;font-style: italic;">无此文件</span>
          <a-button
            v-else
            :ghost="true"
            type="primary"
            icon="download"
            size="small"
            @click="uploadFile(text)">
            下载
          </a-button>
        </template>

        <span slot="action" slot-scope="text, record">
          <a @click="handleEdit(record)">编辑</a>

          <a-divider type="vertical" />
          <a-dropdown>
            <a class="ant-dropdown-link">更多 <a-icon type="down" /></a>
            <a-menu slot="overlay">
              <a-menu-item>
                <a-popconfirm title="确定删除吗?" @confirm="() => handleDelete(record.id)">
                  <a>删除</a>
                </a-popconfirm>
              </a-menu-item>
            </a-menu>
          </a-dropdown>
        </span>

      </a-table>
    </div>

    <ydkjMallStoreProduct-modal ref="modalForm" @ok="modalFormOk"></ydkjMallStoreProduct-modal>
  </a-card>
</template>

<script>

  import { JeecgListMixin } from '@/mixins/JeecgListMixin'
  import YdkjMallStoreProductModal from './modules/YdkjMallStoreProductModal'

  export default {
    name: "YdkjMallStoreProductList",
    mixins:[JeecgListMixin],
    components: {
      YdkjMallStoreProductModal
    },
    data () {
      return {
        description: 'ydkj_mall_store_product管理页面',
        // 表头
        columns: [
          {
            title: '#',
            dataIndex: '',
            key:'rowIndex',
            width:60,
            align:"center",
            customRender:function (t,r,index) {
              return parseInt(index)+1;
            }
          },
          {
            title:'商户Id(0为总后台管理员创建,不为0的时候是商户后台创建)',
            align:"center",
            dataIndex: 'merId'
          },
          {
            title:'商品图片',
            align:"center",
            dataIndex: 'image'
          },
          {
            title:'轮播图',
            align:"center",
            dataIndex: 'sliderImage'
          },
          {
            title:'商品名称',
            align:"center",
            dataIndex: 'storeName'
          },
          {
            title:'商品简介',
            align:"center",
            dataIndex: 'storeInfo'
          },
          {
            title:'关键字',
            align:"center",
            dataIndex: 'keyword'
          },
          {
            title:'产品条码（一维码）',
            align:"center",
            dataIndex: 'barCode'
          },
          {
            title:'分类id',
            align:"center",
            dataIndex: 'cateId'
          },
          {
            title:'商品价格',
            align:"center",
            dataIndex: 'price'
          },
          {
            title:'会员价格',
            align:"center",
            dataIndex: 'vipPrice'
          },
          {
            title:'市场价',
            align:"center",
            dataIndex: 'otPrice'
          },
          {
            title:'邮费',
            align:"center",
            dataIndex: 'postage'
          },
          {
            title:'单位名',
            align:"center",
            dataIndex: 'unitName'
          },
          {
            title:'排序',
            align:"center",
            dataIndex: 'sort'
          },
          {
            title:'销量',
            align:"center",
            dataIndex: 'sales'
          },
          {
            title:'库存',
            align:"center",
            dataIndex: 'stock'
          },
          {
            title:'状态（0：未上架，1：上架）',
            align:"center",
            dataIndex: 'isShow'
          },
          {
            title:'是否热卖',
            align:"center",
            dataIndex: 'isHot'
          },
          {
            title:'是否优惠',
            align:"center",
            dataIndex: 'isBenefit'
          },
          {
            title:'是否精品',
            align:"center",
            dataIndex: 'isBest'
          },
          {
            title:'是否新品',
            align:"center",
            dataIndex: 'isNew'
          },
          {
            title:'产品描述',
            align:"center",
            dataIndex: 'description'
          },
          {
            title:'添加时间',
            align:"center",
            dataIndex: 'addTime'
          },
          {
            title:'是否包邮',
            align:"center",
            dataIndex: 'isPostage'
          },
          {
            title:'是否删除',
            align:"center",
            dataIndex: 'isDel'
          },
          {
            title:'商户是否代理 0不可代理1可代理',
            align:"center",
            dataIndex: 'merUse'
          },
          {
            title:'获得积分',
            align:"center",
            dataIndex: 'giveIntegral'
          },
          {
            title:'成本价',
            align:"center",
            dataIndex: 'cost'
          },
          {
            title:'秒杀状态 0 未开启 1已开启',
            align:"center",
            dataIndex: 'isSeckill'
          },
          {
            title:'砍价状态 0未开启 1开启',
            align:"center",
            dataIndex: 'isBargain'
          },
          {
            title:'是否优品推荐',
            align:"center",
            dataIndex: 'isGood'
          },
          {
            title:'虚拟销量',
            align:"center",
            dataIndex: 'ficti'
          },
          {
            title:'浏览量',
            align:"center",
            dataIndex: 'browse'
          },
          {
            title:'产品二维码地址(用户小程序海报)',
            align:"center",
            dataIndex: 'codePath'
          },
          {
            title:'淘宝京东1688类型',
            align:"center",
            dataIndex: 'soureLink'
          },
          {
            title: '操作',
            dataIndex: 'action',
            align:"center",
            scopedSlots: { customRender: 'action' }
          }
        ],
        url: {
          list: "/mall/ydkjMallStoreProduct/list",
          delete: "/mall/ydkjMallStoreProduct/delete",
          deleteBatch: "/mall/ydkjMallStoreProduct/deleteBatch",
          exportXlsUrl: "/mall/ydkjMallStoreProduct/exportXls",
          importExcelUrl: "/mall/ydkjMallStoreProduct/importExcel",
        },
        dictOptions:{},
      }
    },
    computed: {
      importExcelUrl: function(){
        return `${window._CONFIG['domianURL']}/${this.url.importExcelUrl}`;
      }
    },
    methods: {
      initDictConfig(){
      }
    }
  }
</script>
<style scoped>
  @import '~@assets/less/common.less';
</style>