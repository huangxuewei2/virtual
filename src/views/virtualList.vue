<template>
    <div>
        <ul id="app">
            <div class="list-wrap" ref="listWrap" @scroll="fnScroll">
                <div class="scroll-bar" ref="scrollBar">
                    <ul class="list" ref="list">
                        <li v-for="val in showList">{{val}}</li>
                    </ul>
                </div>

            </div>
        </ul>


    </div>
</template>
<script>
    export default {
        name: "virtualList",
        data() {
            return {
                list: [],
                itemHeight: 30,
                showNum: 20,
                start: 0,
                end: 20,
                fnScroll: () => { }
            };
        },
        watch: {
            start: {
                deep: true,
                handler(val) {
                    this.add();
                    this.dele()
                }
            },
            end: {
                deep: true,
                handler(val) {
                    this.add();
                    this.dele()
                }
            }
        },
        computed: {
            showList() {
                return this.list.slice(this.start, this.end);
            }
        },
        created() {
            this.fnScroll = this.throttle(this.scrollListener, 20)
        },
        mounted() {
            for (let i = 0; i < 100; i++) {
                this.list.push(i)
            }
            this.$refs.listWrap.style.height = this.itemHeight * this.showNum + 'px';
            this.$refs.scrollBar.style.height = this.itemHeight * this.list.length + 'px';

        },
        methods: {
            add() {
                if (this.start == 0) {

                } else {
                    let index = this.list[this.length0-1]
                    for (let i = index; i < index+10; i++) {
                        this.list.push(i)
                    }
                }

            },
            dele() {

            },
            throttle(func, time) {
                var timer = null;
                return function () {
                    var context = this;
                    var args = arguments;
                    if (!timer) {
                        timer = setTimeout(function () {
                            func()
                            timer = null;
                        }, time);
                    }
                }
            },
            scrollListener() {
                let scrollTop = this.$refs.listWrap.scrollTop;
                this.start = Math.floor(scrollTop / this.itemHeight);
                this.end = this.start + this.showNum;
                this.$refs.list.style.top = this.start * this.itemHeight + 'px';
            }
        }
    };
</script>

<style lang="scss" scoped>
    .list-wrap {
        position: relative;
        overflow-y: scroll;
        width: 300px;
        margin: 100px auto;
        box-sizing: border-box;
        border: solid 1px black;
    }

    .list {
        position: absolute;
        top: 0;
        left: 0;
    }

    .list li {
        border: solid 1px black;
        line-height: 30px;
    }
</style>