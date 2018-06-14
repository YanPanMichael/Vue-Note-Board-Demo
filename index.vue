<style>
    * {
        margin: 0;
        padding: 0
    }

    li {
        list-style: none;
    }

    a {
        text-decoration: none;
        color: #333;
    }

    img {
        border: none;
        display: block;
    }

    .clearFix:after {
        content: '';
        display: block;
        clear: both;
        height: 0;
        width: 0;
    }

    .clearFix {
        zoom: 1
    }

    .fl {
        float: left;
    }

    .fr {
        float: right;
    }

    .wrap {
        width: 980px;
        margin: 0 auto;
    }
</style>

<template>
    <div id="main">
        <MainHeader></MainHeader>
        <MainWrapper></MainWrapper>
    </div>
</template>

<script>
    import MainHeader from './main-header.vue'
    import MainWrapper from './main-wrapper.vue'
    new Vue({
        //这里面是选项对象，里面有很多值
        el: "#main",//挂在点，挂在到main上
        data: {
        },
        components: {
            MainHeader,
            MainWrapper
        }
    });
</script>