<template>
    <div class="editor-box rel">
        <div class="line-box abs">
            <canvas ref="canvas"></canvas>
        </div>
        <div class="dom-box rel">
            <a-row type="flex" justify="end">
                <div class="ml-10"><a-button type="primary" @click="reset">自动排列</a-button></div>
            </a-row>
            <div class="dom-item abs" v-for="(item, key) in list" :key="key" :style="{left: item.pos[0] + 'px', top: item.pos[1] + 'px'}">
                <div class="item-abs-top abs" @click="remove(item)">-</div>
                <div class="item-abs-bottom abs" @click="add(item)">+</div>
                <div class="dom-item-cont" @mousedown="mouseDown($event, key)">
                    <p>{{ item.title }}</p>
                    <p>{{ item.id }}</p>
                </div>
            </div>
        </div>
        <div class="editor-mask" :class="{move: move}" v-show="showMask" @mousemove="mouseMove($event)" @mouseup="mouseUp($event)"></div>
    </div>
</template>

<script>
const WIDTH = 1000
const HEIGHT = 1000
let totalHeight = 150
let pen = null
const mousePos = {
    mx: 0,
    my: 0,
    ex: 0,
    ey: 0,
    ele: null
}
let list = []
let tid = 10
export default {
    components: {

    },
    data() {
        return {
            showMask: false,
            move: false,
            list: [],
            linePos: []
        }
    },
    mounted () {
        this.init()
    },
    created() {
        list = [
            {
                id: 1,
                pid: 0,
                title: '1',
                name: '1',
            },
            {
                id: 6,
                pid: 5,
                title: '6',
                name: '6',
            },
            {
                id: 5,
                pid: 1,
                title: '5',
                name: '5',
            },
            {
                id: 3,
                pid: 1,
                title: '3',
                name: '3',
            }
        ]
        window.ve = this
        this.initPos()
    },
    methods: {
        initPos(){
            list = list.sort((a, b) => a.id - b.id)
            list = list.sort((a, b) => a.pid - b.pid)
            let length = {}
            let pid = 0
            let levelIndex = -1
            list.forEach(ele => {
                list.forEach(element => {
                    if(ele.pid === element.id){
                        ele.pele = element
                    }
                })
            })
            list.forEach(ele => {
                if(!ele.pele){
                    ele.level = 1
                }else{
                    ele.level = ele.pele.level + 1
                }
                if(length[ele.level]){
                    length[ele.level] ++
                    levelIndex ++
                }else{
                    length[ele.level] = 1
                    levelIndex = 0
                }
                ele.levelIndex = levelIndex
            })

            this.list = list.map(ele => {
                let level = ele.level
                let len = length[level]
                let levelIndex = ele.levelIndex
                let x = WIDTH / len * (levelIndex + 0.5)
                let y = totalHeight * level
                ele.pos = [x, y]
                return ele
            })
        },
        init(){
            let canvas = this.$refs.canvas
            canvas.width = WIDTH
            canvas.height = HEIGHT
            pen = canvas.getContext('2d')
            pen.strokeStyle = '#999'
            this.draw()
        },
        draw(){
            pen.clearRect(0, 0, WIDTH, HEIGHT)
            for(let i = 0; i < this.list.length; i++){
                let end = this.list[i]
                let x1 = end.pos[0] + 60
                let y1 = end.pos[1]
                let cx1 = x1
                let cy1 = y1 - 60
                let start = this.list[i].pele
                if(start && end.pid === start.id){
                    let x2 = start.pos[0] + 60
                    let y2 = start.pos[1] + 60
                    let cx2 = x2
                    let cy2 = y2 + 60
                    pen.beginPath()
                    pen.moveTo(x1, y1)
                    pen.bezierCurveTo(cx1, cy1, cx2, cy2, x2, y2)
                    pen.stroke()
                    pen.closePath()
                }                    
            
            }
        },
        minAndMax(min, value, max){
            return value > max ? max : value < min ? min : value
        },
        mouseDown(event, key){
            if(event.buttons !== 1){
                return false
            }
            this.showMask = true
            mousePos.ex = event.pageX
            mousePos.ey = event.pageY
            mousePos.ele = this.list[key]
            mousePos.mx = mousePos.ele.pos[0]
            mousePos.my = mousePos.ele.pos[1]
        },
        mouseMove(event){
            let ex = event.pageX
            let ey = event.pageY
            let _ex = mousePos.ex - ex
            let _ey = mousePos.ey - ey
            if(Math.abs(_ex) >= 1 || Math.abs(_ey) >= 1){
                this.move = true
            }
            let x = this.minAndMax(0, mousePos.mx - _ex, WIDTH - 120)
            let y = this.minAndMax(0, mousePos.my - _ey, HEIGHT - 60)
            mousePos.ele.pos = [x, y]
            this.draw()
        },
        mouseUp(event){
            this.showMask = false
            this.move = false
            let ex = event.pageX
            let ey = event.pageY
            let _ex = Math.abs(mousePos.ex - ex)
            let _ey = Math.abs(mousePos.ey - ey)
            if(_ex < 1 && _ey < 1){
                this.$router.push('/SQLEditor?id=' + mousePos.ele.id)
            }
            mousePos.ex = 0
            mousePos.ey = 0
            mousePos.ele = null
            mousePos.mx = 0
            mousePos.my = 0
        },
        reset(){
            this.initPos()
            this.draw()
        },
        remove(item){
            list = list.filter(ele => ele.id !== item.id)
            list.forEach(ele => {
                if(ele.pid === item.id){
                    ele.pid = item.pid
                }
            })
            this.initPos()
            this.draw()
        },
        add(item){
            let id = tid++
            let _item = {
                id: id,
                pid: item.id,
                title: id,
                name: id,
            }
            list.push(_item)
            this.initPos()
            this.draw()
        },
    }
}
</script>

<style lang="scss" scoped>
.rel{
    position: relative;
}
.abs{
    position: absolute;
}
.dom-box{
    width: 1000px;
    height: 1000px;
}
.dom-item{
    .item-abs-top,
    .item-abs-bottom{
        height: 20px;
        width: 20px;
        text-align: center;
        line-height: 20px;
        background: #fff;
        border: 1px solid #ddd;
        display: none;
        cursor: pointer;
    }
    .item-abs-top{
        right: 0;
        top: -20px;
    }
    .item-abs-bottom{
        left: 50%;
        margin-left: -10px;
        bottom: -20px;
    }
    &:hover{
        .item-abs-top,
        .item-abs-bottom{
            display: block;
        }
    }
    .dom-item-cont{
        padding: 10px;
        background: #f2f6fa;
        border: 1px solid #eee;
        width: 120px;
        height: 60px;
        text-align: center;
        overflow: hidden;
        line-height: 20px;
        cursor: pointer;
        p{
            margin: 0;
        }
    }
}
.editor-mask{
    position: fixed;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
    z-index: 999;
    cursor: pointer;
    &.move{
        cursor: move;
    }
}
.ml-10{
    margin-left: 10px;
}
</style>