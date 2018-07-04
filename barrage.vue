<template>
    <div id="barrageWrapper" ref="barrageWrapper">

    </div>
</template>
<style lang="scss">
    #barrageWrapper{
        width: 100%;
        height: 90px;
        position:absolute;
        top:0;
        left:0;
        width:100%;
        overflow-x: hidden;
        
        .barrageItem{
            width:auto;
            text-align: center;
            height: 30px;
            background-color: rgba(0,59,147,.7);
            font-size: 11px;
            color:#FEED64;
            display: inline-block;
            line-height: 30px;
            padding: 0px 4px;
            border-radius: 30px;
            position: absolute;
            white-space: nowrap;
            &.move{
                transition: transform 4.5s linear;
            }
        }
    }
</style>
<script>
    import { Component, life } from 'miox-vue2x-classify';

    const bodyWidth = window.document.body.clientWidth;
    @Component({
        props: {
            data: Array,
            line: { // 行数
                default: 3,
                type: Number,
            },
            rate: { // 速度单位s
                default: 1,
                type: Number,
            },
        },
    })
    export default class Barrage {
        current = -1;
        @life
        mounted() {
            this.move();
        }
        move() {
            this.createDom();
            setInterval(() => {
                this.createDom();
            }, this.rate * 1000);
        }
        createDom() {
            const wrapperItem = this.$refs.barrageWrapper;
            const div = window.document.createElement('div');
            const nowDiv = wrapperItem.querySelectorAll('.barrageItem');
            if (nowDiv.length === Math.ceil(4.5 / this.rate)) {
                wrapperItem.removeChild(nowDiv[0]);
            }
            this.current += 1;
            if (this.current === this.data.length) {
                this.current = 0;
            }
            div.innerHTML = `${this.data[this.current].text}`;
            div.classList.add('barrageItem');
            wrapperItem.appendChild(div);
            const divH = div.clientHeight * (Math.floor(Math.random() * this.line));
            div.style.width = `${div.clientWidth}px`;
            div.style.webkitTransform = `translate3d(${window.parseInt((bodyWidth * 100) / div.clientWidth)}%, ${divH}px, 0)`;
            setTimeout(() => {
                div.style.webkitTransform = `translate3d(-100%, ${divH}px, 0)`;
                div.classList.add('move');
            }, 10);
        }
    }
</script>
