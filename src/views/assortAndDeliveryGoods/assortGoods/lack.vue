<template>
    <div class="container-fluid">
        <div class="unify-head">
            缺货配货订单
        </div>
        <div class="query-form">
            <div class="query-header">
                <div class="checkbox-group">
                    <label class="checkbox-title">
                        缺货查询
                    </label>
                    <div class="checkbox-list">
                    </div>
                    <a href="javascript:void(0)" class="btn btn-blue-circle" data-toggle="collapse" @click="toggleItem">{{show.flog ? '收起多余选项':'展开全部选项'}}<span class="iconfont icon-up"></span></a>
                </div>
            </div>
            <div id="query-detail" class="form-inline collapse in query-detail">
                <div class="row expand-show" :class="{'show-expand' : show.flog}">
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >订单号</label>
                        <input v-model="query.Sheet | arrayToString" type="text" class="form-control" placeholder="多个订单用；隔开">
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >外部交易号</label>
                        <input v-model="query.OutSheet | arrayToString" type="text" class="form-control"  placeholder="多个订单用；隔开">
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >店铺</label>
                        <select v-model="query.ShopID" class="form-control">
                            <option value="shopList.ShopID" v-for="shopList in baseLookUp.shopLists">{{shopList.Name}}</option>
                        </select>
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >支付方式</label>
                        <select v-model="query.PayMode" class="form-control">
                            <option value="" selected>不限</option>
                            <option value="payList.Code" v-for="payList in baseLookUp.payLists">{{payList.Text}}</option>
                        </select>
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >收货人姓名</label>
                        <input v-model="query.LinkMan" type="text" class="form-control"  placeholder="">
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >收货地址</label>
                        <input v-model="query.Address" type="text" class="form-control"  placeholder="">
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >收货人电话</label>
                        <input v-model="query.Phone" type="text" class="form-control"  placeholder="">
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >仓库名称</label>
                        <select v-model="query.StockID" class="form-control">
                            <option value="" selected>不限</option>
                            <option value="stockList.StockID"  v-for="stockList in baseLookUp.stockLists">{{stockList.Name}}</option>
                        </select>
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >快递公司</label>
                        <select v-model="query.MerchantDeliveryID" class="form-control">
                            <option value="" selected>不限</option>
                            <option value="merchantDeliveryList.DeliveryID" v-for="merchantDeliveryList in baseLookUp.merchantDeliveryLists">{{merchantDeliveryList.Name}}</option>
                        </select>
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >商品编码</label>
                        <input v-model="query.GoodsCode" type="text" class="form-control"  placeholder="">
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >商品名称</label>
                        <input v-model="query.GoodsName" type="text" class="form-control"  placeholder="">
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >客户留言</label>
                        <input v-model="query.BuyerMessage" type="text" class="form-control"  placeholder="">
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >店铺留言</label>
                        <input v-model="query.SellerMemo" type="text" class="form-control"  placeholder="">
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >订单备注</label>
                        <input v-model="query.TradeMemo" type="text" class="form-control"  placeholder="">
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >颜色</label>
                        <input v-model="query.Color" type="text" class="form-control"  placeholder="">
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >尺码</label>
                        <input v-model="query.Size" type="text" class="form-control"  placeholder="">
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >需要发票</label>
                        <select v-model="query.Invoice" class="form-control">
                            <option selected value="">不限</option>
                            <option value=0>否</option>
                            <option value=1>是</option>
                        </select>
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label>数量范围</label>
                        <input v-model="query.NumBeg" type="number" min="0" class="form-control" placeholder="">
                        <span class="form-control-link">~</span>
                        <input v-model="query.NumEnd"  type="number" min="0" class="form-control" placeholder="">
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label>金额范围</label>
                        <input v-model="query.MoneyBeg"  type="number" min="0" class="form-control" placeholder="">
                        <span class="form-control-link">~</span>
                        <input v-model="query.MoneyEnd"  type="number" min="0" class="form-control" placeholder="">
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >订单类型</label>
                        <select v-model="query.OrderType" class="form-control">
                            <option value="" selected>不限</option>
                            <option value="customerOrderType.Code" v-for="customerOrderType in baseLookUp.customerOrderTypes">{{customerOrderType.Text}}</option>
                        </select>
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >业务类型</label>
                        <select v-model="query.Purchase" class="form-control">
                            <option value="" selected>不限</option>
                            <option value="customerOrderPurchase.Code" v-for="customerOrderPurchase in baseLookUp.customerOrderPurchases">{{customerOrderPurchase.Text}}</option>
                        </select>
                    </div>
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >买家昵称</label>
                        <input v-model="query.BuyerNick" type="text" class="form-control"  placeholder="">
                    </div>
                    <!--<div class="form-group col-lg-3 col-md-4 col-sm-6">-->
                        <!--<label >标题</label>-->
                        <!--<input v-model="query.Title" type="text" class="form-control"  placeholder="">-->
                    <!--</div>-->
                    <div class="form-group col-lg-3 col-md-4 col-sm-6">
                        <label >SKU</label>
                        <input v-model="query.SKU" type="text" class="form-control"  placeholder="">
                    </div>
                    <!--<div class="form-group col-lg-3 col-md-4 col-sm-6">-->
                        <!--<label >来源</label>-->
                        <!--<input v-model="query.TradeFrom" type="text" class="form-control"  placeholder="">-->
                    <!--</div>-->
                    <!--<div class="form-group col-lg-3 col-md-4 col-sm-6">-->
                        <!--<label >标记</label>-->
                        <!--<select v-model="query.Mark" class="form-control">-->
                            <!--<option selected value="">不限</option>-->
                            <!--<option  value=1>是</option>-->
                            <!--<option  value=0>否</option>-->
                        <!--</select>-->
                    <!--</div>-->
                    <div class="form-group col-lg-6 col-md-12 col-sm-12">
                        <label >付款时间</label>
                        <div class="date-wrap">
                            <div class='input-group date' id='pay-timepicker-start'>
                                <input v-model="query.PayTimeBeg | formatDate" type='text' class="form-control" />
                        <span class="input-group-addon"><span class="glyphicon glyphicon-calendar"></span>
                        </span>
                            </div>
                        </div>
                        <span class="date-link">至</span>
                        <div class="date-wrap">
                            <div class='input-group date' id='pay-timepicker-end'>
                                <input v-model="query.PayTimeEnd | formatDate" type='text' class="form-control" />
                        <span class="input-group-addon"><span class="glyphicon glyphicon-calendar"></span>
                        </span>
                            </div>
                        </div>
                    </div>
                    <div class="form-group col-lg-6 col-md-12 col-sm-12">
                        <label >审核时间</label>
                        <div class="date-wrap">
                            <div class='input-group date' id='check-timepicker-start'>
                                <input v-model="query.CheckTimeBeg | formatDate" type='text' class="form-control" />
                        <span class="input-group-addon"><span class="glyphicon glyphicon-calendar"></span>
                        </span>
                            </div>
                        </div>
                        <span class="date-link">至</span>
                        <div class="date-wrap">
                            <div class='input-group date' id='check-timepicker-end'>
                                <input v-model="query.CheckTimeEnd | formatDate" type='text' class="form-control" />
                        <span class="input-group-addon"><span class="glyphicon glyphicon-calendar"></span>
                        </span>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="query-button-group">
                    <a href="javascript:void(0)" class="btn btn-default" @click="search">搜索</a>
                    <a href="javascript:void(0)" class="btn btn-default" @click="resetForm">重置</a>
                    <!--<a href="javascript:void(0)" class="btn btn-default">导出订单</a>-->
                    <!--<a href="javascript:void(0)" class="btn btn-default">导出商品明细</a>-->
                </div>
            </div>
        </div>
        <div class="sheet-list">
            <div class="row">
                <div class="col-lg-12">
                    <div class="batch-operation">
                        <label class="batch-operation-title">批量操作:</label>
                        <div class="batch-operation-group">
                            <!--<a href="javascript:void(0)" class="btn btn-default">生成采购单</a>-->
                            <!--<a href="javascript:void(0)" class="btn btn-default" @click="recoveryCustomerOrder('')">解除缺货</a>-->
                            <a href="javascript:void(0)" class="btn btn-default" @click="setOutOfStockCustomerOrder('')">置为缺货</a>
                        </div>
                    </div>
                </div>
                <div class="col-lg-12">
                    <div class="sheet-list-table table-responsive">
                        <table class="table table-condensed panel-group" id="sheet-list" role="tablist">
                            <thead class="sheet-list-header">
                            <tr>
                                <th class="sheet-header-state"><input type="checkbox" v-model="selectAll"></th>
                                <th>状态</th>
                                <th>订单号</th>
                                <th>店铺</th>
                                <th>平台</th>
                                <th>收货人</th>
                                <th>收货地址</th>
                                <th>支付方式</th>
                                <th>实收款</th>
                                <th>交易备注</th>
                            </tr>
                            </thead>
                            <tbody v-for="customerOrder in customerOrders" class="panel panel-default">
                            <tr class="sheet-list-item">
                                <td class="sheet-item-state"><input type="checkbox" v-model="customerOrder.checked"></td>
                                <td>{{customerOrder.OrderFlag.FlagDesc}}</td>
                                <td @click="getOrderItems(customerOrder)" class="show-detail-ctrl collapsed" data-toggle="collapse" data-parent="#sheet-list"
                                    data-target="#{{customerOrder.CustomerOrderID}}"><span class="iconfont"></span><span>{{customerOrder.Sheet}}</span>
                                </td>
                                <td><span data-toggle="tooltip" data-placement="top" title="{{customerOrder.Shop.Name}}">{{customerOrder.Shop.Name | truncationString}}</span></td>
                                <td><span data-toggle="tooltip" data-placement="top" title="{{customerOrder.Shop.NetShop.Channel.Name}}">{{customerOrder.Shop.NetShop.Channel.Name | truncationString}}</span></td>
                                <td><span data-toggle="tooltip" data-placement="top" title="{{customerOrder.LinkMan}}">{{customerOrder.LinkMan | truncationString}}</span></td>
                                <td><span data-toggle="tooltip" data-placement="top" title="{{customerOrder.Country ?customerOrder.Country + ',': ''}}{{customerOrder.State ? customerOrder.State + ',' : ''}}{{customerOrder.City ? customerOrder.City + ',' : ''}}{{customerOrder.District ? customerOrder.District + ',' : ''}}{{customerOrder.Address}}">{{customerOrder.Country?customerOrder.Country + ',': ''}}{{customerOrder.State ? customerOrder.State + ',' : ''}}{{customerOrder.City ? customerOrder.City + ',' : ''}}{{customerOrder.District ? customerOrder.District + ',' : ''}}{{customerOrder.Address | truncationString}}</td></td>
                                <td><span data-toggle="tooltip" data-placement="top" title="{{customerOrder.PayModeDesc}}">{{customerOrder.PayModeDesc | truncationString}}</span></td>
                                <td>{{customerOrder.TotalAmount}}</td>
                                <td><span data-toggle="tooltip" data-placement="top" title="{{customerOrder.OrderBusiness.TradeMemo}}">{{customerOrder.OrderBusiness.TradeMemo | truncationString}}</span></td>
                            </tr>
                            <tr class="sheet-list-detail">
                                <td colspan="10">
                                    <div class="panel-collapse collapse sheet-list-collapse" role="tabpanel"
                                         id="{{customerOrder.CustomerOrderID}}">
                                        <div class="sheet-detail-wrap">
                                            <order-detail  :order-items="customerOrder.OrderItems"></order-detail>
                                        </div>
                                    </div>
                                </td>
                            </tr>
                            </tbody>
                        </table>
                        <div v-if="customerOrders.length == 0 && !$loadingRouteData " class="no-data">没有可供显示的结果</div>
                        <pagination :pagination-options="paginationOptions"></pagination>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    var servers = require('../../../js/servers/servers');
    var NProgress = servers.NProgress;
    var commonMethods = require('../../../js/common');
    module.exports = {
        data: function () {
            return {
                show: {
                    flog: false
                },
                //查询条件
                query: {
                    Status: [], //订单状态
                    Sheet: [], //订单号
                    OutSheet: [], // 外部交易号，即网店单号
                    ShopID: '',  // 网店ID
                    PayMode: '', // 支付模式
                    PayTimeBeg: '', // 付款时间开始
                    PayTimeEnd: '', // 付款时间结束
                    CheckTimeBeg: '', //审核时间开始
                    CheckTimeEnd: '', //审核时间结束
                    LinkMan: '', // 收货人
                    Address: '', // 收货地址
                    Phone: '', // 收货人电话
                    StockID: '', // 仓库ID
                    MerchantDeliveryID: '', // 快递公司ID
                    Title: '', // 标题
                    SKU: '', // SKU
                    GoodsCode: '', // 商品编号
                    Color: '', // 商品颜色
                    Size: '', // 商品尺码
                    GoodsName: '', // 商品名称
                    BuyerMessage: '', // 买家留言
                    BuyerMemo: '', // 买家备注
                    SellerMemo: '', // 卖家备注
                    TradeMemo: '', // 交易备注
                    BuyerNick: '', // 买家昵称
                    TradeFrom: '', // 来源
                    Mark: '', // 标记
                    Invoice: '', // 发票
                    Purchase: '', // 客户订单业务类型
                    OrderType: '', // 客户订单类型常量类
                    NumBeg: '', // 数量范围开始
                    NumEnd: '', // 数量范围结束
                    MoneyBeg: '', // 金额范围开始
                    MoneyEnd: '' // 金额范围结束
                },
                //下拉列表数据
                baseLookUp: {
                    shopLists: [],  //销售平台
                    payLists: [],   // 支付方式
                    stockLists: [], // 仓库
                    customerOrderTypes: [], //订单类型
                    customerOrderPurchases: [], //业务类型
                    merchantDeliveryLists: [], //快递公司
                    customerOrderStatus: []  //所有订单状态
                },
                //查询出来的订单结果
                customerOrders: [],
                //分页
                paginationOptions: {
                    all: 0,
                    cur: 1,
                    pageSize: 15,
                    totalRecords: 0
                }
            }
        },
        methods: {
            //收展多余的选项
            toggleItem: function () {
                this.show.flog = !this.show.flog;
            },
            // 重置搜索表单
            resetForm: function () {
                this.query = {
                    Status: this.query.Status,
                    Sheet: [], //订单号
                    OutSheet: [], // 外部交易号，即网店单号
                    ShopID: '',  // 网店ID
                    PayMode: '', // 支付模式
                    PayTimeBeg: '', // 付款时间开始
                    PayTimeEnd: '', // 付款时间结束
                    CheckTimeBeg: '', //审核时间开始
                    CheckTimeEnd: '', //审核时间结束
                    LinkMan: '', // 收货人
                    Address: '', // 收货地址
                    Phone: '', // 收货人电话
                    StockID: '', // 仓库ID
                    MerchantDeliveryID: '', // 快递公司ID
                    Title: '', // 标题
                    SKU: '', // SKU
                    GoodsCode: '', // 商品编号
                    Color: '', // 商品颜色
                    Size: '', // 商品尺码
                    GoodsName: '', // 商品名称
                    BuyerMessage: '', // 买家留言
                    BuyerMemo: '', // 买家备注
                    SellerMemo: '', // 卖家备注
                    TradeMemo: '', // 交易备注
                    BuyerNick: '', // 买家昵称
                    TradeFrom: '', // 来源
                    Mark: '', // 标记
                    Invoice: '', // 发票
                    Purchase: '', // 客户订单业务类型
                    OrderType: '', // 客户订单类型常量类
                    NumBeg: '', // 数量范围开始
                    NumEnd: '', // 数量范围结束
                    MoneyBeg: '', // 金额范围开始
                    MoneyEnd: '' // 金额范围结束
                };
            },
            //获取下拉表单基础数据
            getBaseData: function () {
                var baseLookUp = this.baseLookUp;
                //获取销售平台
                servers.getBaseData('ShopList',false,function (val) {
                    baseLookUp.shopLists = val;
                });
                //获取支付方式
                servers.getBaseData('PayModel',false,function (val) {
                    baseLookUp.payLists = val;
                });
                //获取仓库
                servers.getBaseData('StockList',false,function (val) {
                    baseLookUp.stockLists = val;
                });
                //获取订单类型
                servers.getBaseData('CustomerOrderType',false,function (val) {
                    baseLookUp.customerOrderTypes = val;
                });
                //获取所有业务类型
                servers.getBaseData('CustomerOrderPurchase',false,function (val) {
                    baseLookUp.customerOrderPurchases = val;
                });
                //获取所有快递公司
                servers.getBaseData('MerchantDeliveryList',false,function (val) {
                    baseLookUp.merchantDeliveryLists = val;
                });
                //获取所有客户订单状态
                servers.getBaseData('CustomerOrderStatus',false,function (val) {
                    baseLookUp.customerOrderStatus = val;
                })
            },
            /**
             * 获取订单详情
             * @param customerOrder {object} 订单数据
             */
            getOrderItems: function (customerOrder) {
                var customerOrderID = customerOrder.CustomerOrderID;     //订单ID
                var orderItems = customerOrder.OrderItems;     //订单详情
                var OrderFlag = customerOrder.OrderFlag.Flag;   //订单状态
                var isDoneItem = OrderFlag == 97 || OrderFlag == 98 || OrderFlag == 100;   //是否为已完成或已取消订单
                console.log(orderItems);
                if (orderItems.length > 0) {
                    return;
                } else {
                    NProgress.start();
                    if (isDoneItem) {       //如果是已完成或已取消订单,则用此接口获取订单详情数据
                        servers.postAjax.bind(this)({
                            url: '/BusCustomerOrder/ErpQueryCustomerOrderItemDoneByID',
                            data: {
                                ID: customerOrderID
                            }
                        }).done(function (result) {
                            NProgress.done();
                            if (result.ResultCode == 1) {
                                result.Data.forEach(function (item) {
                                    orderItems.push(item);
                                });
                            }
                        })
                    } else {
                        servers.postAjax.bind(this)({
                            url: '/BusCustomerOrder/ErpQueryCustomerOrderItemTempByID',
                            data: {
                                ID: customerOrderID
                            }
                        }).done(function (result) {
                            NProgress.done();
                            if (result.ResultCode == 1) {
                                result.Data.forEach(function (item) {
                                    orderItems.push(item);
                                });
                            }
                        })
                    }

                }
            },
            //获取订单数据
            getCustomerOrders: function () {
                var self = this;
                NProgress.start();
                servers.postAjax.bind(this)({
                    url: '/BusCustomerOrder/ErpQueryNotStockOrder',
                    data: {
                        PageSize: self.paginationOptions.pageSize,
                        PageNumber: self.paginationOptions.cur,
                        SearchWhere: self.query
                    }
                }).done(function (result) {
                    NProgress.done();
                    if (result.ResultCode == 1) {
                        self.customerOrders = [];
                        var customerOrders = result.Data.PageData;
                        if (customerOrders.length > 0) {
                            self.paginationOptions.all = result.Data.TotalPages;
                            self.paginationOptions.totalRecords = result.Data.TotalRecords;
                            //为每个订单注入checked属性,方便全选时使用
                            customerOrders.forEach(function (value) {
                                value.checked = false;
                            });
                            self.customerOrders = customerOrders;
                        } else {
//                            self.$dispatch('transmit','tip',{
//                                name: '提示',
//                                contentText: '结果为空'
//                            });
                        }

                    }
                })
            },
            //搜索
            search: function () {
                if (this.paginationOptions.cur == 1) {
                    this.getCustomerOrders();
                } else {
                    this.paginationOptions.cur = 1;
                }
            },
            //获取选中的订单
            getCustomerIDGroup: function (customerID) {
                var self = this;
                var customerIDGroup = [];
                if (customerID) {
                    customerIDGroup.push(customerID);
                } else {
                    customerIDGroup = this.checkedGroups;
                }
                if (customerIDGroup.length === 0) {
                    self.$dispatch('transmit','tip',{
                        name: '提示',
                        contentText: '请至少选中一个订单'
                    });
                    return [];
                } else {
                    return customerIDGroup;
                }
            },
            //解除缺货
            recoveryCustomerOrder: function (customerID) {
                var self = this;
                var customerIDGroup = this.getCustomerIDGroup(customerID);
                if (customerIDGroup.length > 0) {
                    self.$dispatch('transmit','tip',{
                        name: '提示:',
                        contentText: '您确定要恢复该订单吗?',
                        btnName: '恢复',
                        events: {
                            confirm: 'recoveryCustomerOrder'
                        }
                    });
                    self.$off('recoveryCustomerOrder');

                    self.$once('recoveryCustomerOrder',function () {
                        NProgress.start();
                        servers.postAjax.bind(this)({
                            url: '/BusCustomerOrder/ErpRecoveryCustomerOrder',
                            contentType: 'application/json',
                            data: JSON.stringify(customerIDGroup)
                        }).done(function (result) {
                            NProgress.done();
                            if (result.ResultCode == 1) {
                                self.$dispatch('transmit','tip',{
                                    name: '成功',
                                    contentText: '恢复客户订单成功'
                                });
                            }
                        })
                    });
                }
            },
            setOutOfStockCustomerOrder: function (customerID) {
                var self = this;
                var customerIDGroup = this.getCustomerIDGroup(customerID);
                if (customerIDGroup.length > 0) {
                    self.$dispatch('transmit','tip',{
                        name: '提示:',
                        contentText: '您确定要将该订单置为缺货吗?',
                        btnName: '置为缺货',
                        events: {
                            confirm: 'setOutOfStockCustomerOrder'
                        }
                    });
                    self.$off('setOutOfStockCustomerOrder');

                    self.$once('setOutOfStockCustomerOrder',function () {
                        NProgress.start();
                        servers.postAjax.bind(this)({
                            url: '/BusCustomerOrder/ErpSetOutOfStockCustomerOrder',
                            contentType: 'application/json',
                            data: JSON.stringify(customerIDGroup)
                        }).done(function (result) {
                            NProgress.done();
                            if (result.ResultCode == 1) {
                                self.$dispatch('transmit','tip',{
                                    name: '成功',
                                    contentText: '置为缺货成功!'
                                });
                            }
                        })
                    });
                }
            }
        },
        route: {
            data: function (transition) {
                this.getCustomerOrders();
                transition.next();
            }
        },
        events: {
            'page-change': function () {
                this.getCustomerOrders();
            }
        },
        ready: function () {
            //获取下拉列表数据
            this.getBaseData();
            //dateTimePicker
            require('../../../css/bootstrap-datetimepicker.css');
            require('../../../js/plugins/bootstrap-datetimepicker');
            $('#pay-timepicker-start,#check-timepicker-start').datetimepicker({
//                locale: 'zh-cn'
            });
            $('#pay-timepicker-end,#check-timepicker-end').datetimepicker({
//                locale: 'zh-cn'
            });
            $("#pay-timepicker-start,#check-timepicker-start").on("dp.change",function (e) {
                $('#pay-timepicker-end,#check-timepicker-end').data("DateTimePicker").minDate(e.date);
            });
            $("#pay-timepicker-end,#check-timepicker-end").on("dp.change",function (e) {
                $('#pay-timepicker-start,#check-timepicker-start').data("DateTimePicker").maxDate(e.date);
            });



            $('#pay-timepicker-start,#check-timepicker-start').datetimepicker({
//                locale: 'zh-cn'
            }).on('changeDate',function (e) {
                $('#pay-timepicker-end,#check-timepicker-end').datetimepicker('setStartDate',e.date);
            });
            $('#pay-timepicker-end,#check-timepicker-end').datetimepicker({
//                locale: 'zh-cn'
            }).on('changeDate',function (e) {
                $('#pay-timepicker-start,#check-timepicker-start').datetimepicker('setEndDate',e.date);
            });

            //显示详情页
            $(document).on('show.bs.collapse','.sheet-list-collapse', function () {
                var sheetListCollapseId = $(this).attr('id');
                $('[data-target="#' + sheetListCollapseId + '"]').parents('.sheet-list-item').addClass('detail-opened');

            });
            $(document).on('hidden.bs.collapse','.sheet-list-collapse',function () {
                var sheetListCollapseId = $(this).attr('id');
                $('[data-target="#' + sheetListCollapseId + '"]').parents('.sheet-list-item').removeClass('detail-opened');
            });

        },
        computed: commonMethods.selectAll('this.customerOrders', function (item) {
            return item.CustomerOrderID
        }),
        components: {
            orderDetail: require('./order-detail.vue'),
            modal: require('../../../components/modal.vue'),
            pagination: require('../../../components/pagination.vue'),
        }
    }
</script>

<style rel="stylesheet/less" lang="less">

</style>