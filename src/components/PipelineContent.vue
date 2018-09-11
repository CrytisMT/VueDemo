<template>
    <div :class="prefixCls" v-show="show">
        <slot></slot>
    </div>
</template>

<script>
    export default {
        name: "PipelineContent",
        data() {
            return {
                prefixCls: 'pipeline-content',
                show: true
            }
        }
    }
</script>

<style scoped>
    .pipeline-content {
        display: none;
        /*opacity: 0;*/
        /*height: 0;*/
    }
</style>