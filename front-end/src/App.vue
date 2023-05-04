<template>
    <div id="app" >
        <b-container>
            <b-row>
                <b-col cols="12">
                    <app-header></app-header>
                </b-col>
                <b-col cols="12">
                    <app-content></app-content>
                </b-col>
                <b-col cols="12">
                    <app-footer></app-footer>
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>

<script>
import Header from "./components/Header";
import Footer from "./components/Footer";
import Content from "./components/Content";

export default {
    name: "yolo",
    data() {
        return {};
    },
    components: {
        "app-header": Header,
        "app-footer": Footer,
        "app-content": Content,
    },
    methods: {},
};
</script>

<style>
#app {
    /*font-family: 'Avenir', Helvetica, Arial, sans-serif;*/
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    /*text-align: center;*/
    /*color: #2c3e50;*/
    /*margin-top: 60px;*/
    background-image: linear-gradient(to top, #fff1eb 0%, #ace0f9 100%);
}

</style>
