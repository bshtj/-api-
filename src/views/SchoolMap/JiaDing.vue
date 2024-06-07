<template>
    <el-container style="height: 100%;border: 1px solid #eee">

        <el-aside width="55px" style="background-color: rgb(170,176,182); ">

            <div class="button-container">
                <!-- 同济大学校园地图 -->
                <el-button type="primary" icon="el-icon-search" circle @click="drawer_primary = true"></el-button>
                <el-drawer title="同济大学校园地图" :visible.sync="drawer_primary" size="40%" @opened="initPOIComplete"
                    z-index=900>
                    <div>
                        <input v-model="input" placeholder="请输入想要搜索的地点" id="tipinput" style="width: 80%;height: 30px" />
                        <el-button type="primary" @click="drawer_primary_inner = true">导航</el-button>
                        <div>
                            <el-drawer title="步行路径规划" :append-to-body="true" :visible.sync="drawer_primary_inner"
                                @opened="initWalkingComplete">
                                <el-input v-model="input1" placeholder="起点" id="from"></el-input>
                                <el-input v-model="input2" placeholder="终点" id="to"></el-input>
                                <el-button type="primary" @click="showRoute">出发</el-button>
                                <div id="panel2"></div>

                            </el-drawer>
                        </div>

                    </div>
                    <!-- 存放关键字搜索同济大学(嘉定校区)详细信息的表格 -->
                    <div id="panel1"></div>
                </el-drawer>
                <!-- 同济大学校园通 -->
                <el-button type="info" icon="el-icon-question" circle @click="drawer_info = true"></el-button>
                <el-drawer title="同济大学校园通" :visible.sync="drawer_info" size="50%" z-index=900>
                    <el-container style="height: 100%;border: 1px solid #eee">
                        <el-aside width="100px" style="background-color: white">
                            <el-tabs :tab-position="tabPosition" v-model="activeTab" @tab-click="handleTabClick"
                                style="height: 100%;">
                                <el-tab-pane label="生活活动" name="1"></el-tab-pane>
                                <el-tab-pane label="场馆服务" name="2"></el-tab-pane>
                                <el-tab-pane label="医疗咨询" name="3"></el-tab-pane>
                                <el-tab-pane label="交通出行" name="4"></el-tab-pane>
                            </el-tabs>
                        </el-aside>
                        <!-- 使用Collpase折叠面板 -->
                        <el-container>
                            <div v-if="activeTab === '1'" style="width:100%">
                                <el-collapse v-model="activeNamesFor1" @change="handleChange">
                                    <el-collapse-item title="请问哪里可以预约HPV?" name="1">
                                        <div>四平社区医院即可，携带身份证与学生证。</div>
                                    </el-collapse-item>
                                    <el-collapse-item title="请问无一卡通权限可以使用学生医保吗?" name="2">
                                        <div>可以。</div>

                                    </el-collapse-item>
                                    <el-collapse-item title="请问旧衣服可以怎么处理?" name="3">
                                        <div>可前往同济大学慈善爱心屋捐赠。</div>
                                        <div>(1)四平路校区:经纬楼底楼长条展厅</div>
                                        <div>(2)嘉定校区:甘其食旁小亭子</div>
                                        <div>(3)沪北校区:第一宿舍楼旁小卖部</div>
                                        <div>捐赠热线、联系电话:021-65981145</div>
                                        <div>微信公众号“同济勤助”</div>

                                    </el-collapse-item>
                                    <el-collapse-item title="请问哪里可以修改衣服?" name="4">
                                        <div>(1)同济新村绿色门面的那家</div>
                                        <div>(2)赤峰路93号小区门口有裁缝店</div>
                                        <div>(3)彰武路衣生缘干洗</div>
                                    </el-collapse-item>
                                </el-collapse>
                            </div>
                            <div v-else-if="activeTab === '2'" style="width:100%">
                                <!-- 标签页2的内容 -->
                                <el-collapse v-model="activeNamesFor2" @change="handleChange">
                                    <el-collapse-item title="请问游泳馆的开放资讯?" name="1">
                                        <div>(1)教学时段：仅供学生游泳课教学使用；</div>
                                        <div>(2)开放时段：可对符合学校防疫要求的校内教职员工和学生开放；</div>
                                        <div>(3)开放最大人数:限流250人;</div>
                                        <div>(4)周日暂不开放，后续根据前期开放情况和学校防疫总体情况而定;</div>
                                        <div>(5)泳客进入场馆前需扫描门口“场所码”和“同学码”。</div>
                                    </el-collapse-item>
                                    <el-collapse-item title="请问体育馆的预约网址是?" name="2">
                                        <div><a href="https://stadium.tongji.edu.cn/">https://stadium.tongji.edu.cn/</a>
                                        </div>
                                        <div> <a
                                                href="https://sports.tongji.edu.cn/info/1046/1592.htm">https://sports.tongji.edu.cn/info/1046/1592.htm</a>
                                        </div>
                                    </el-collapse-item>
                                </el-collapse>
                            </div>
                            <div v-else-if="activeTab === '3'" style="width:100%">
                                <!-- 标签页3的内容 -->
                                <el-collapse v-model="activeNamesFor3" @change="handleChange">
                                    <el-collapse-item title="请问哪里可以验光测度数" name="1">
                                        <div>四平路校区大礼堂北侧爱康眼镜店可以验光；</div>
                                    </el-collapse-item>
                                    <el-collapse-item title="请问校医院可以查过敏原吗" name="2">
                                        <div>可以，请挂皮肤科，约四个工作日出结果；</div>
                                    </el-collapse-item>
                                    <el-collapse-item title="请问校医院有CT吗" name="3">
                                        <div>有的，若遇到校医院无法进行的检测可以请医生开转诊单，转到瑞金医院或者新华医院做检查，凭转诊单可报销部分费用。</div>
                                    </el-collapse-item>
                                    <el-collapse-item title="请问校医院的眼科开放时间?" name="4">
                                        <div>仅周六上午。</div>
                                    </el-collapse-item>
                                </el-collapse>
                            </div>
                            <div v-else-if="activeTab === '4'" style="width:100%">
                                <!-- 标签页4的内容 -->
                                <el-collapse v-model="activeNamesFor4" @change="handleChange">
                                    <el-collapse-item title="请问学生如何购买火车票" name="1">
                                        <div>前往火车站窗口或者自助售票机处办理学生证优惠资质。</div>
                                    </el-collapse-item>
                                </el-collapse>
                            </div>
                        </el-container>
                    </el-container>
                </el-drawer>
                <!-- 个人课表 -->
                <el-button type="warning" icon="el-icon-school" circle @click="drawer_courseware = true"></el-button>
                <el-drawer title="个人课表" :visible.sync="drawer_courseware" size="40%" z-index=900>
                </el-drawer>
                <!-- 同济大学校历 -->
                <el-button type="success" icon="el-icon-date" circle @click="drawer_calendar = true"></el-button>
                <el-drawer title="同济大学校历" :visible.sync="drawer_calendar" size="40%" z-index=900>
                    <el-calendar v-model="value">
                    </el-calendar>
                </el-drawer>
                <div style="position:absolute;bottom: 10px;display: flex;align-items: center;">
                    <el-avatar icon="el-icon-user-solid"></el-avatar>
                </div>
            </div>

        </el-aside>
        <el-container>
            <el-main style="padding : 0">
                <div id="container"></div>
            </el-main>
        </el-container>

    </el-container>
</template>

<script>
import AMapLoader from "@amap/amap-jsapi-loader";

export default {
    data() {
        return {
            drawer_primary: false,
            drawer_primary_inner: false,
            drawer_info: false,
            drawer_courseware: false,
            drawer_calendar: false,

            input: '',
            input1: '',
            input2: '',
            tabPosition: 'left',
            activeNamesFor1: [],
            activeNamesFor2: [],
            activeNamesFor3: [],
            activeNamesFor4: [],
            activeTab: '1', // 初始显示的标签页

            value: new Date(),
        }
    },
    name: "map-view",
    // 为确保能访问到AMap，可以在加载完成时保存AMap引用
    mounted() {
        this.initAMap();

    },
    unmounted() {
        this.map?.destroy();
    },
    methods: {

        initAMap() {
            window._AMapSecurityConfig = {
                securityJsCode: "84a5c415561ec7c4ee215e4a77fe15ec",
            };

            AMapLoader.load({
                key: "4ede47e3906dd5d8a2e45126b491b770", // 申请好的Web端开发者Key，首次调用 load 时必填
                version: "2.0", // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
                plugins: ["AMap.ToolBar", "AMap.Scale"], //需要使用的的插件列表，如比例尺'AMap.Scale'，支持添加多个如：['...','...']
            })
                .then((AMap) => {
                    this.AMap = AMap;
                    this.map = new AMap.Map("container", {
                        rotateEnable: true,//支持旋转
                        pitchEnable: true,//支持倾斜
                        rotation: -15,//初始旋转角度为逆时针15度
                        pitch: 50,//初始倾斜50度
                        viewMode: '3D', //默认使用 2D 模式
                        zoom: 17, //地图级别
                        center: [121.21416, 31.286012], //地图中心点
                    });
                    var toolbar = new AMap.ToolBar(); //创建工具条插件实例
                    this.map.addControl(toolbar); //添加工具条插件到页面
                    var scale = new AMap.Scale();
                    this.map.addControl(scale);


                })
                .catch((e) => {
                    console.log(e);
                });
        },
        initPOIComplete() {
            // 确保AMap加载完成
            if (!this.AMap) {
                console.error("AMap is not loaded");
                return;
            }

            this.AMap.plugin(['AMap.PlaceSearch', 'AMap.AutoComplete'], () => {
                var auto = new this.AMap.AutoComplete({ input: "tipinput" });
                var placeSearch = new this.AMap.PlaceSearch({
                    map: this.map
                });  // 构造地点查询类
                var placeSearch_shanghai = new this.AMap.PlaceSearch({
                    pageSize: 9, // 单页显示结果条数
                    pageIndex: 1, // 页码
                    city: "021", // 兴趣点城市设置为上海
                    citylimit: true,  //是否强制限制在设置的城市内搜索
                    map: this.map, // 展现结果的地图实例
                    panel: "panel1", // 结果列表将在此容器中进行展示。
                    autoFitView: true // 是否自动调整地图视野使绘制的 Marker点都处于视口的可见范围
                });
                placeSearch_shanghai.search("同济大学(嘉定校区)");
                auto.on("select", select);  // 注册监听，当选中某条记录时会触发
                function select(e) {

                    placeSearch.setCity(e.poi.adcode);
                    placeSearch.search(e.poi.name);  // 关键字查询
                    placeSearch_shanghai.search(e.poi.name);

                }

            });
        },
        initWalkingComplete() {
            // 确保AMap加载完成
            if (!this.AMap) {
                console.error("AMap is not loaded");
                return;
            }
            // 换成function()就不行，必须()=>
            this.AMap.plugin(['AMap.Walking', 'AMap.PlaceSearch', 'AMap.AutoComplete'], () => {

                var auto1 = new this.AMap.AutoComplete({ input: "from" });
                var auto2 = new this.AMap.AutoComplete({ input: "to" });
                var placeSearchForFrom = new this.AMap.PlaceSearch({
                    map: this.map
                });  // 构造地点查询类
                var placeSearchForTo = new this.AMap.PlaceSearch({
                    map: this.map
                });
                auto1.on("select", select1);  // 注册监听，当选中某条记录时会触发
                auto2.on("select", select2);  // 注册监听，当选中某条记录时会触发
                var points = [];
                this.points = points;
                function select1(e) {
                    placeSearchForFrom.setCity(e.poi.adcode);
                    placeSearchForFrom.search(e.poi.name);  // 关键字查询
                    points[0] = { keyword: e.poi.name, city: "上海" };
                }
                function select2(e) {
                    placeSearchForTo.setCity(e.poi.adcode);
                    placeSearchForTo.search(e.poi.name);  // 关键字查询
                    points[1] = { keyword: e.poi.name, city: "上海" };
                }
            });
        },
        showRoute() {
            var walking = new this.AMap.Walking({
                map: this.map,
                panel: "panel2"
            });
            walking.search(this.points, function (status, result) {
                // result即是对应的步行路线数据信息，相关数据结构文档请参考  https://lbs.amap.com/api/javascript-api/reference/route-search#m_WalkingResult
                if (status === 'complete') {
                    console.log('绘制步行路线完成')
                } else {
                    console.log('步行路线数据查询失败' + result)
                }
            });
        },
        handleChange(val) {
            console.log(val);
        },
        handleTabClick(tab) {
            this.activeTab = tab.name;
        },
    },
};
</script>
<style scoped>
#container {
    display: flex;
    width: 100%;
    height: 800px;

}

.button-container {
    display: flex;
    flex-direction: column;
    /* 设置子元素沿列方向排列 */
    align-items: center;
    /* 水平居中 */
}

.el-button {
    margin: 5px;
}

#panel {
    background-color: white;
    max-height: 90%;
    /* 超出高度设置滚动条 */
    overflow-y: auto;
    width: 100%;
    height: 100%;
}
</style>