<template>
    <h1 @click="click">cilck</h1>
    <div id="unity-container" class="unity-desktop">
        <canvas id="unity-canvas" width=200 height=200></canvas>
        <div id="unity-loading-bar">
            <div id="unity-logo"></div>
            <div id="unity-progress-bar-empty">
                <div id="unity-progress-bar-full"></div>
            </div>
        </div>
        <div id="unity-mobile-warning">
            WebGL builds are not supported on mobile devices.
        </div>
    </div>
</template>
<script>
    export default {
        mounted() {
            // this.initUnity()
        },
        beforeDestroy() {
          this.initUnity()
        },
        methods:{
            click(){
                this.initUnity()
            },
            initUnity(){
                console.log("sad")
                var buildUrl = "webGL/Build";
                var loaderUrl = buildUrl + "/Midterm-Final.loader.js";
                var config = {
                    dataUrl: buildUrl + "/Midterm-Final.data",
                    frameworkUrl: buildUrl + "/Midterm-Final.framework.js",
                    codeUrl: buildUrl + "/Midterm-Final.wasm",
                    streamingAssetsUrl: "StreamingAssets",
                    companyName: "DefaultCompany",
                    productName: "Midterm-Final",
                    productVersion: "0.1",
                };

                var container = document.querySelector("#Unity-container");
                var canvas = document.querySelector("#Unity-canvas");
                var loadingBar = document.querySelector("#Unity-loading-bar");
                var progressBarFull = document.querySelector("#Unity-progress-bar-full");
                var fullscreenButton = document.querySelector("#Unity-fullscreen-button");
                var mobileWarning = document.querySelector("#Unity-mobile-warning");

                // By default Unity keeps WebGL canvas render target size matched with
                // the DOM size of the canvas element (scaled by window.devicePixelRatio)
                // Set this to false if you want to decouple this synchronization from
                // happening inside the engine, and you would instead like to size up
                // the canvas DOM size and WebGL render target sizes yourself.
                // config.matchWebGLToCanvasSize = false;

                if (/iPhone|iPad|iPod|Android/i.test(navigator.userAgent)) {
                    container.className = "Unity-mobile";
                    // Avoid draining fillrate performance on mobile devices,
                    // and default/override low DPI mode on mobile browsers.
                    config.devicePixelRatio = 1;
                    mobileWarning.style.display = "block";
                    setTimeout(() => {
                        mobileWarning.style.display = "none";
                    }, 5000);
                } else {
                    canvas.style.width = "960px";
                    canvas.style.height = "600px";
                }
                loadingBar.style.display = "block";

                var script = document.createElement("script");
                script.src = loaderUrl;
                script.onload = () => {
                    createUnityInstance(canvas, config, (progress) => {
                        progressBarFull.style.width = 100 * progress + "%";
                    }).then((unityInstance) => {
                        loadingBar.style.display = "none";
                        fullscreenButton.onclick = () => {
                            unityInstance.SetFullscreen(1);
                        };
                    }).catch((message) => {
                        alert(message);
                    });
                };
                document.body.appendChild(script);
            }
        }
    }
</script>