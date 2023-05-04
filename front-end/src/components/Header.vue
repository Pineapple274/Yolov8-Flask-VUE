<template>
    <div id="Header" style="margin-top: 15px;border-radius: 5px;">
        <b-container>
            <b-row>
                <b-col cols="6">
                    <div id="left">
                        <h1><i class="el-icon-star-off" style="font-size: 30px; font-weight: bold">线路巡检</i
                        ></h1>
                        <h2><i class="el-icon-time" style="font-size: 30px;color: #21b3b9; font-weight: bold">{{ dateFormat(date) }}</i>
                        </h2>
                    </div>
                </b-col>
                <b-col cols="6" style="padding: 5px;margin-top: 5px">
                    <div class="carousel-container">
                        <div class="carousel-slide">
                            <img src="../../pic/Bus.jpg" alt="Image 1" />
                            <img src="../../pic/OIP.jpg" alt="Image 2" />
                            <img src="../../pic/R.jpg" alt="Image 3" />
                            <img src="../../pic/Bus.jpg" alt="Image 1" />
                            <img src="../../pic/OIP.jpg" alt="Image 2" />
                            <img src="../../pic/R.jpg" alt="Image 3" />
                        </div>
                    </div>
                </b-col>
            </b-row>
        </b-container>

    </div>
</template>
<script>
export default {
    data() {
        return {
            date: new Date(),
        }
    },
    methods: {

        dateFormat(time) {
            var date = new Date(time);
            var year = date.getFullYear();
            var month = date.getMonth() + 1 < 10 ? "0" + (date.getMonth() + 1) : date.getMonth() + 1;
            var day = date.getDate() < 10 ? "0" + date.getDate() : date.getDate();
            var hours = date.getHours() < 10 ? "0" + date.getHours() : date.getHours();
            var minutes = date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes();
            var seconds = date.getSeconds() < 10 ? "0" + date.getSeconds() : date.getSeconds();
            // 拼接
            return year + "-" + month + "-" + day + " " + hours + ":" + minutes + ":" + seconds;
        }
    },
    mounted() {
        setInterval(() => {
            this.currentIndex = (this.currentIndex + 1) % this.imageList.length;
        }, 3000);
        //显示当前日期时间
        let _this = this// 声明一个变量指向Vue实例this，保证作用域一致
        this.timer = setInterval(() => {
            _this.date = new Date(); // 修改数据date
        }, 1000)
    },
    beforeDestroy() {
        if (this.timer) {
            clearInterval(this.timer); // 在Vue实例销毁前，清除我们的定时器
        }
    },
}


</script>

<style scoped>
#Header {
    width: 100%;
    height: 120px;
    background-color: #E0FFFF;
}

#left {
    text-align:center;
}


h1 {
    /*text-align: center;*/
    color: #21b3b9;
    letter-spacing: 35px;
    font-size: 2.3em;
    margin-top: 20px;
}
h2 {
    color: #21b3b9;
}

.top-left-edition span i {
    float: left;
    margin-right: 10px;
}

i,
input,
label {
    vertical-align: middle;
}

i {
    border: 0;
    display: block;
    cursor: pointer;
}

.top-left-edition span {
    float: left;
    font-size: 16px;
    color: #999999;
    line-height: 24px;
    margin-right: 40px;
}
.carousel-container {
    position: relative;
    height: 100px;
    overflow: hidden;

}

.carousel-slide {
    display: flex;
    animation: slide 10s infinite;
}

.carousel-slide img {
    height: 100px;
    object-fit: cover;
}

@keyframes slide {
    0% {
        transform: translateX(0);
    }
    100% {
        transform: translateX(-50%);
    }
}
</style>


