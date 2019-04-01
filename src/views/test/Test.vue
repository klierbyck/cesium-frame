<template>
    <div>
        <div id="cesiumBox"></div>
    </div>
</template>
<script>
    export default {
        data () {
            return {
                
            }
        },
        mounted () {
            new this.Cesium.Viewer('cesiumBox')
        }
    };
</script>
<style lang="less">
    @import url("./test.less");
</style>