<style>
    .page-top {
        background-color: #e8593c;
        height: 40px;
        border-top: 1px solid #b56553;
        border-bottom: 1px solid #b56553;
    }

    .page-top .page-content h2 {
        width: 100%;
        line-height: 40px;
        color: #fff;
        text-align: center;
        font-size: 22px;
    }
</style>
<template id="mainHead">
    <div class="page-top">
        <div class="page-content wrap">
            <h2>计划任务表</h2>
        </div>
    </div>
</template>
<script>
export default {
    name: 'main-header',
    template: '#mainHead',
    data () {
        return { }
    }
}
</script>
