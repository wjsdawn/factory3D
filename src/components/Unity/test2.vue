<template>
    <div class="flood-plan-page">
        <unity src="./src/assets/webGL/Build/Midterm-Final.data" width="1000" height="600" unityLoader="./src/assets/webGL/Build/Midterm-Final.loader.js" ref="unityvue" class="main-unity"></unity>
    </div>
</template>

<script>
    import Unity from 'vue-unity-webgl'
    export default {
        components:{
            Unity
        },
        methods:{

        }
    }
</script>