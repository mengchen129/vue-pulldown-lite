<template>
    <div id="app">
        <ul ref="container" :class="{transition: yDiff == 0}" :style="{transform: 'translate3d(0, ' + yDiff + 'px, 0)'}"
            @touchstart="onTouchstart" @touchmove="onTouchmove" @touchend="onTouchend">
            <slot></slot>
        </ul>
        <div class="pull-tip">
            <div class="loading-icon" v-show="canRefresh" :style="{'margin-top': (yDiff / 2) - 20 + 'px'}"></div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'pull-down',
        data () {
            return {
                startY: 0,
                moveY: 0,
                enablePullDown: false,
                yDiff: 0,
                canRefresh: false
            }
        },
        methods: {
            onTouchstart: function(e) {
                if (this.$refs.container.scrollTop == 0) {
                    this.enablePullDown = true;
                    this.startY = e.touches[0].clientY;
                }
            },
            onTouchmove: function(e) {
                if (this.enablePullDown) {
                    this.moveY = e.touches[0].clientY;
                    var yDiff = this.moveY - this.startY;
                    if (yDiff > 0) {
                        e.preventDefault();
                        this.yDiff = parseInt(yDiff / 2);
                        this.canRefresh = this.yDiff >= 40;
                    }
                }
            },
            onTouchend: function(e) {
                if (this.$refs.container.scrollTop == 0) {
                    this.enablePullDown = false;
                    this.yDiff = 0;
                }

                if (this.canRefresh) {
                    this.$emit('refresh');
                    this.canRefresh = false;
                }
            }
        }
    }
</script>

<style>
    ul {
        position: fixed;
        top: 0;
        left: 0;
        bottom: 0;
        width: 100%;
        overflow: auto;
        z-index: 2;
        background-color: #fff;
        -webkit-overflow-scrolling: touch;
    }

    .transition {
        transition: transform 0.5s ease-out;
    }

    .pull-tip {
        height: 50px;
        line-height: 50px;
        font-size: 16px;
        text-align: center;
        color: #666;
        position: fixed;
        left: 0;
        top: 0;
        bottom: 0;
        width: 100%;
        z-index: 1;
    }

    @keyframes loading-rotate {
        0% {
            transform: rotate3d(0, 0, 1, 0deg);
        }
        100% {
            transform: rotate3d(0, 0, 1, 360deg);
        }
    }

    .loading-icon {
        width: 40px;
        height: 40px;
        margin: 0 auto;
        animation: loading-rotate 1s steps(12, end) infinite;
        background: url(data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxMjAiIGhlaWdodD0iMTIwIiB2aWV3Qm94PSIwIDAgMTAwIDEwMCI+PHBhdGggZmlsbD0ibm9uZSIgZD0iTTAgMGgxMDB2MTAwSDB6Ii8+PHJlY3Qgd2lkdGg9IjciIGhlaWdodD0iMjAiIHg9IjQ2LjUiIHk9IjQwIiBmaWxsPSIjRTlFOUU5IiByeD0iNSIgcnk9IjUiIHRyYW5zZm9ybT0idHJhbnNsYXRlKDAgLTMwKSIvPjxyZWN0IHdpZHRoPSI3IiBoZWlnaHQ9IjIwIiB4PSI0Ni41IiB5PSI0MCIgZmlsbD0iIzk4OTY5NyIgcng9IjUiIHJ5PSI1IiB0cmFuc2Zvcm09InJvdGF0ZSgzMCAxMDUuOTggNjUpIi8+PHJlY3Qgd2lkdGg9IjciIGhlaWdodD0iMjAiIHg9IjQ2LjUiIHk9IjQwIiBmaWxsPSIjOUI5OTlBIiByeD0iNSIgcnk9IjUiIHRyYW5zZm9ybT0icm90YXRlKDYwIDc1Ljk4IDY1KSIvPjxyZWN0IHdpZHRoPSI3IiBoZWlnaHQ9IjIwIiB4PSI0Ni41IiB5PSI0MCIgZmlsbD0iI0EzQTFBMiIgcng9IjUiIHJ5PSI1IiB0cmFuc2Zvcm09InJvdGF0ZSg5MCA2NSA2NSkiLz48cmVjdCB3aWR0aD0iNyIgaGVpZ2h0PSIyMCIgeD0iNDYuNSIgeT0iNDAiIGZpbGw9IiNBQkE5QUEiIHJ4PSI1IiByeT0iNSIgdHJhbnNmb3JtPSJyb3RhdGUoMTIwIDU4LjY2IDY1KSIvPjxyZWN0IHdpZHRoPSI3IiBoZWlnaHQ9IjIwIiB4PSI0Ni41IiB5PSI0MCIgZmlsbD0iI0IyQjJCMiIgcng9IjUiIHJ5PSI1IiB0cmFuc2Zvcm09InJvdGF0ZSgxNTAgNTQuMDIgNjUpIi8+PHJlY3Qgd2lkdGg9IjciIGhlaWdodD0iMjAiIHg9IjQ2LjUiIHk9IjQwIiBmaWxsPSIjQkFCOEI5IiByeD0iNSIgcnk9IjUiIHRyYW5zZm9ybT0icm90YXRlKDE4MCA1MCA2NSkiLz48cmVjdCB3aWR0aD0iNyIgaGVpZ2h0PSIyMCIgeD0iNDYuNSIgeT0iNDAiIGZpbGw9IiNDMkMwQzEiIHJ4PSI1IiByeT0iNSIgdHJhbnNmb3JtPSJyb3RhdGUoLTE1MCA0NS45OCA2NSkiLz48cmVjdCB3aWR0aD0iNyIgaGVpZ2h0PSIyMCIgeD0iNDYuNSIgeT0iNDAiIGZpbGw9IiNDQkNCQ0IiIHJ4PSI1IiByeT0iNSIgdHJhbnNmb3JtPSJyb3RhdGUoLTEyMCA0MS4zNCA2NSkiLz48cmVjdCB3aWR0aD0iNyIgaGVpZ2h0PSIyMCIgeD0iNDYuNSIgeT0iNDAiIGZpbGw9IiNEMkQyRDIiIHJ4PSI1IiByeT0iNSIgdHJhbnNmb3JtPSJyb3RhdGUoLTkwIDM1IDY1KSIvPjxyZWN0IHdpZHRoPSI3IiBoZWlnaHQ9IjIwIiB4PSI0Ni41IiB5PSI0MCIgZmlsbD0iI0RBREFEQSIgcng9IjUiIHJ5PSI1IiB0cmFuc2Zvcm09InJvdGF0ZSgtNjAgMjQuMDIgNjUpIi8+PHJlY3Qgd2lkdGg9IjciIGhlaWdodD0iMjAiIHg9IjQ2LjUiIHk9IjQwIiBmaWxsPSIjRTJFMkUyIiByeD0iNSIgcnk9IjUiIHRyYW5zZm9ybT0icm90YXRlKC0zMCAtNS45OCA2NSkiLz48L3N2Zz4=) no-repeat;
        background-size: 100%;
    }
</style>
