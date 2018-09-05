<template>

    <div>
        <div class="pipeline-wrapper">
            <div class="pipeline">
                <span>开始</span>
            </div>
            <Icon :size=20 :color="'#20a0ff'" type="md-arrow-forward"/>
        </div>

        <div class="pipeline-wrapper" v-for="pipe in dynamicPipes">
            <div class="pipeline-node" :class="{'disable-node':!pipe.checked}" @click="clickNode">
                <input type="checkbox" class="checkbox" v-model="pipe.checked"></input>
                <span>{{pipe.name}}</span>
            </div>
            <Icon :size=20 :color="'#20a0ff'" type="md-arrow-forward"/>
        </div>

        <div class="pipeline">
            <span>完成</span>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Pipeline",
        data() {
            return {
                pipes: [
                    {
                        name: 'aaa',
                        checked: true,
                        dependsOn: []
                    },
                    {
                        name: 'bbb',
                        checked: true,
                        dependsOn: ['aaa']
                    }]
            }
        },
        methods: {
            clickNode(e) {
                console.log(e)
            }
        },
        computed: {
            dynamicPipes() {
                if (this.pipes.length <= 1) {
                    return this.pipes;
                }
                for (let i = 1; i < this.pipes.length; i++) {
                    let pipe = this.pipes[i];
                    if (pipe.dependsOn && pipe.dependsOn.length > 0) {
                        for (let j = 0; j < i; j++) {
                            if (pipe.dependsOn.includes(this.pipes[j].name) && !this.pipes[j].checked) {
                                pipe.checked = false;
                            }
                        }
                    }
                }

                //todo some tips?
                return this.pipes;
            }
        }
    }
</script>

<style scoped lang="stylus">
    .pipeline, .pipeline-node
        position: relative
        display: inline-block
        width: 100px
        height: 50px
        border-radius: 5px
        transition .3s
        &:hover
            input
                display inline-block
        .checkbox
            position: absolute
            top: 0px
            right: 0px
            border-radius: 5px;
            display none
            background-color white
        span
            vertical-align: middle
            display inline-block
            line-height 50px //与div一致
            font-size 14px

    .pipeline-node
        cursor pointer
        background: #20a0ff;
        span
            color: white

    .pipeline
        background white
        border solid 1px #20a0ff
        span
            color #20a0ff

    .pipeline-wrapper
        display inline-block

    .disable-node
        background gray


</style>
