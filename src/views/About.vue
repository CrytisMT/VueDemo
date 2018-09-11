<template>
    <Pipeline :pipes="pipes">
        <pipeline-content>aaa</pipeline-content>
        <pipeline-content>bbb</pipeline-content>
    </Pipeline>
</template>



<script>


    import Pipeline from "../components/Pipeline";
    import PipelineContent from "../components/PipelineContent";

    export default {
        name: 'About',
        components: {
            PipelineContent,
            Pipeline
        },
        data(){
            return{
                pipes: [
                    {
                        name: 'aaa',
                        checked: true,
                        dependsOn: [],
                        contentName:'aaa'
                    },
                    {
                        name: 'bbb',
                        checked: true,
                        dependsOn: ['aaa']
                    }]
            }
        }
    }
</script>
