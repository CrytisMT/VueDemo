<template>

    <div>
        <div>
            <div class="pipeline-wrapper">
                <div class="pipeline">
                    <span>开始</span>
                </div>
                <Icon :size=20 :color="'#20a0ff'" type="md-arrow-forward"/>
            </div>

            <div class="pipeline-wrapper" v-for="(pipe,index) in dynamicPipes" :index="index">
                <div class="pipeline-node" :class="{'disable-node':!pipe.checked}" @click="clickNode(index)">
                    <div class="node-checkbox">
                        <label>
                            <input type="checkbox" v-model="pipe.checked"/>
                            <span></span>
                            <div class="show-box"></div>
                        </label>
                    </div>
                    <span>{{pipe.name}}</span>
                </div>
                <Icon :size=20 :color="'#20a0ff'" type="md-arrow-forward"/>
            </div>

            <div class="pipeline">
                <span>完成</span>
            </div>
        </div>
        <div ref="contents">
            <slot></slot>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Pipeline",
        props: ['pipes'],//todo 只勾选后面的 有bug
        data() {
            return {
                activeKey: undefined
            }
        },
        methods: {
            clickNode(index) {
                console.log(index);
                console.log(...this.$refs.contents.children);

                [...this.$refs.contents.children].forEach((child, i) => {
                    if (index === i) {
                        child.style.display = 'inline-block'
                    } else {
                        child.style.display = 'none'
                    }
                })
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
        &:hover
            div
                label
                    .show-box
                        display inline

    .pipeline
        background white
        border solid 1px #20a0ff
        span
            color #20a0ff

    .pipeline-wrapper
        display inline-block

    .disable-node
        background gray

    .node-checkbox
        position: absolute
        top: 0px
        right: 0px
        width: 14px;
        height: 14px;

        /*border-radius: 5px;*/

        label
            position: relative
            cursor: pointer
        input:checked + span + .show-box
            background: #ec6337
        input
            display none
        .show-box
            position: absolute
            top: -2px
            left: -7px
            width: 14px
            height: 14px
            border-radius: 5px
            /*border: 1px solid #d8d8d8*/
            background: white
            display none

            &:before
                content: ''
                position: absolute
                top: 1px
                left: 4px
                width: 6px
                height: 10px
                border: solid white
                border-width: 0 2px 2px 0
                transform: rotate(45deg)

</style>
