<template>
    <div id="Content" style="border-radius: 5px;">
        <b-container>
            <b-row>
                <b-col cols="6" id="rowss">
                    <el-card>
                        <div style="height: 290px;">
                            <el-image
                                    :src="url_1"
                                    class="image_1"
                                    :preview-src-list="srcList"
                                    style="border-radius: 3px 3px 0 0"
                            >
                                <div slot="error">
                                    <div slot="placeholder" class="error">
                                        <el-button
                                                v-show="showbutton"
                                                type="primary"
                                                icon="el-icon-upload"
                                                class="download_bt"
                                                v-on:click="true_upload"
                                        >
                                            上传图像
                                            <input
                                                    ref="upload"
                                                    style="display: none"
                                                    name="file"
                                                    type="file"
                                                    @change="update"
                                                    id="file-input"
                                            />
                                        </el-button>
                                    </div>
                                </div>
                            </el-image>
                        </div
                        >
                        <div class="img_info_1" style="border-radius: 0 0 5px 5px">
                            <span style="color: white; letter-spacing: 6px">原始图像</span>
                        </div>
                    </el-card>
                    <el-card>
                        <div style="height: 290px;">
                            <el-image
                                        :src="url_2"
                                        class="image_1"
                                        :preview-src-list="srcList1"
                                        id="file-input"
                                >
                                    <div slot="error">
                                        <div slot="placeholder" class="error" >{{ wait_return }}</div>
                                    </div>
                                </el-image>
                        </div>
                        <div class="img_info_1">
                            <span style="color: white; letter-spacing: 4px">检测结果</span>
                        </div>
                    </el-card>
                </b-col>
                <b-col cols="6" id="rowss">
                    <el-card style="border-radius: 8px">
                        <div slot="header" class="clearfix">
                            <span style="padding: 10px">检测目标</span>
                            <el-button
                                    v-show="!showbutton"
                                    type="primary"
                                    icon="el-icon-upload"
                                    class="download_bt"
                                    v-on:click="true_upload2"
                            >
                                重新选择图像
                                <input
                                        ref="upload2"
                                        name="file"
                                        type="file"
                                        style="display: none; padding: 5px"
                                        @change="update"
                                />
                            </el-button>
                        </div>
                        <el-tabs v-model="activeName">
                            <el-tab-pane label="检测到的目标" name="first">
                                <!-- 表格存放特征值 -->
                                <el-table
                                        :data="feature_list"
                                        height="390"
                                        border
                                        style="text-align: center"
                                        v-loading="loading"
                                        element-loading-text="数据正在处理中，请耐心等待"
                                        element-loading-spinner="el-icon-loading"
                                        lazy
                                >
                                    <el-table-column label="目标类别">
                                        <template slot-scope="scope">
                                            <span>{{ scope.row[2] }}</span>
                                        </template>
                                    </el-table-column>
                                    <el-table-column label="目标大小">
                                        <template slot-scope="scope">
                                            <span>{{ scope.row[0] }}</span>
                                        </template>
                                    </el-table-column>
                                    <el-table-column label="置信度">
                                        <template slot-scope="scope">
                                            <span>{{ scope.row[1] }}</span>
                                        </template>
                                    </el-table-column>
                                </el-table>
                            </el-tab-pane>
                        </el-tabs>
                    </el-card>
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>

<script>
import axios from "axios";

export default {
    name: "Content",
    data() {
        return {
            server_url: "http://127.0.0.1:5003",
            activeName: "first",
            active: 0,
            centerDialogVisible: true,
            url_1: "",
            url_2: "",
            textarea: "",
            srcList: [],
            srcList1: [],
            feature_list: [],
            feature_list_1: [],
            feat_list: [],
            url: "",
            visible: false,
            wait_return: "等待上传",
            wait_upload: "等待上传",
            loading: false,
            table: false,
            isNav: false,
            showbutton: true,
            percentage: 0,
            fullscreenLoading: false,
            opacitys: {
                opacity: 0,
            },
            dialogTableVisible: false,
        };
    },
    created: function () {
        document.title = "YOLOv5目标检测WEB端";
    },
    methods: {
        true_upload() {
            this.$refs.upload.click();
        },
        true_upload2() {
            this.$refs.upload2.click();
        },
        next() {
            this.active++;
        },
        // 获得目标文件
        getObjectURL(file) {
            var url = null;
            if (window.createObjcectURL != undefined) {
                url = window.createOjcectURL(file);
            } else if (window.URL != undefined) {
                url = window.URL.createObjectURL(file);
            } else if (window.webkitURL != undefined) {
                url = window.webkitURL.createObjectURL(file);
            }
            return url;
        },
        // 上传文件
        update(e) {
            this.percentage = 0;
            this.dialogTableVisible = true;
            this.url_1 = "";
            this.url_2 = "";
            this.srcList = [];
            this.srcList1 = [];
            this.wait_return = "";
            this.wait_upload = "";
            this.feature_list = [];
            this.feat_list = [];
            this.fullscreenLoading = true;
            this.loading = true;
            this.showbutton = false;
            let file = e.target.files[0];
            this.url_1 = this.$options.methods.getObjectURL(file);
            let param = new FormData(); //创建form对象
            param.append("file", file, file.name); //通过append向form对象添加数据
            var timer = setInterval(() => {
                this.myFunc();
            }, 30);
            let config = {
                headers: {"Content-Type": "multipart/form-data"},
            }; //添加请求头
            axios
                .post(this.server_url + "/upload", param, config)
                .then((response) => {
                    this.percentage = 100;
                    clearInterval(timer);
                    this.url_1 = response.data.image_url;
                    this.srcList.push(this.url_1);
                    this.url_2 = response.data.draw_url;
                    this.srcList1.push(this.url_2);
                    this.fullscreenLoading = false;
                    this.loading = false;

                    this.feat_list = Object.keys(response.data.image_info);

                    for (var i = 0; i < this.feat_list.length; i++) {
                        response.data.image_info[this.feat_list[i]][2] = this.feat_list[i];
                        this.feature_list.push(response.data.image_info[this.feat_list[i]]);
                    }

                    this.feature_list.push(response.data.image_info);
                    this.feature_list_1 = this.feature_list[0];
                    this.dialogTableVisible = false;
                    this.percentage = 0;
                    this.notice1();
                });
        },
        myFunc() {
            if (this.percentage + 33 < 99) {
                this.percentage = this.percentage + 33;
            } else {
                this.percentage = 99;
            }
        },
        drawChart() {
        },
        notice1() {
            this.$notify({
                title: "预测成功",
                message: "点击图片可以查看大图",
                duration: 3000,
                type: "success",
            });
        },
    },
    mounted() {
        this.drawChart();
    },
};
</script>

<style>
.el-button {
    padding: 12px 20px !important;
}

#hello p {
    font-size: 15px !important;
    /*line-height: 25px;*/
}

.n1 .el-step__description {
    padding-right: 20%;
    /*font-size: 14px;*/
    /*line-height: 20px;*/
    /* font-weight: 400; */
}
</style>

<style scoped>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}
#rowss{
    padding: 35px
}

.clearfix:before,
.clearfix:after {
    display: table;
    content: "";
}

.clearfix:after {
    clear: both;
}

file-input {
    width: 100%;
    height: 100%;
    object-fit: contain;
}

.image_1 {
    width: 100%;
    height: 100%;
    background: #ffffff;
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
}

.img_info_1 {
    object-fit: contain;
    text-align: center;
    background-color: #21b3b9;
    line-height: 30px;
}


.error {
    margin: 100px auto;
    width: 50%;
    padding: 10px;
    text-align: center;
}


.block-sidebar .block-sidebar-item {
    /*font-size: 50px;*/
    color: lightblue;
    text-align: center;
    /*line-height: 50px;*/
    /*margin-bottom: 20px;*/
    cursor: pointer;
    display: block;
}

.block-sidebar .block-sidebar-item:hover {
    color: #187aab;
}

.download_bt {
    padding: 10px 16px !important;
}


#Content {
    /*background-color: #f0ffff;*/
    background-image: linear-gradient(-20deg, #616161 0%, #9bc5c3 100%);
    display: flex;
    min-width: 100%;
    margin: 30px auto 15px;
}


</style>


