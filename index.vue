<template>
    <div id="main">
        <MainHeader></MainHeader>
        <MainWrapper></MainWrapper>
    </div>
</template>

<script>
    import './index.css'
    import MainHeader from './main-header.vue'
    import MainWrapper from './main-wrapper.vue'
    var vueObj = new Vue({
        //这里面是选项对象，里面有很多值
        el: "#main",//挂在点，挂在到main上
        components: {
            MainHeader,
            MainWrapper
        }
    });
</script>