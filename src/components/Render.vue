<script setup lang="ts">
import {ref, onMounted } from 'vue';

const canvas = ref<HTMLCanvasElement>()
var context: CanvasRenderingContext2D

var sampleRect: Rect[] = []


onMounted(function(){

    // adding sample
    sampleRect = []
    sampleRect.push(new Rect(0, 0, 100, 200))
    sampleRect.push(new Rect(0, 0, 400, 300))
    sampleRect.push(new Rect(0, 0, 250, 100))
    sampleRect.push(new Rect(0, 0, 400, 500))
    sampleRect.push(new Rect(0, 0, 350, 250))
    sampleRect.push(new Rect(0, 0, 450, 150))



    setupCanvas()

})


function setupCanvas(){
    
    let ctx = canvas.value?.getContext("2d")
    if(ctx == null){
        alert("Something went wrong!")
    }else{
        context = ctx
        startRendering()
    }
}


function startRendering(){

    sampleRect.forEach(element => {
        drawRect(element)
    });

    drawLine(new Rect(0, 0, 100, 400))

}




// --------------------- drawing function ----------------------

function drawRect(rect: Rect){
    context.moveTo(rect.left, rect.top)
    context.rect(rect.left, rect.top, rect.width, rect.height)
    context.stroke()
}

function drawLine(rect: Rect){
    context.moveTo(rect.left, rect.top)
    context.lineTo(rect.right, rect.bottom)
    context.stroke()
}




class Rect{

    private _left: number
    private _top: number
    private _right: number
    private _bottom: number

    private _width: number = 0
    private _height: number = 0
    private _centerX: number = 0
    private _centerY: number = 0


    constructor(left: number, top: number, right: number, bottom: number){

        this._left = left
        this._top = top
        this._right = right
        this._bottom = bottom
        this.setWidth()
        this.setHeight()
        this.setCenterX()
        this.setCenterY()
    }

    private setWidth(){
        this._width = Math.abs(this._right - this._left)
    }

    private setHeight(){
        this._height = Math.abs(this._bottom - this._top)
    }

    private setCenterX(){
        this._centerX = this._width / 2 + this._left
    }

    private setCenterY(){
        this._centerY = this._height / 2 + this._top
    }



    // ------------  Setter ----------------
    set left(left: number){
        this._left = left
        this.setWidth()
        this.setCenterX()
    }

    set right(right: number){
        this._right = right
        this.setWidth()
        this.setCenterX()
    }

    set top(top: number){
        this._top = top
        this.setHeight()
        this.setCenterY()
    }

    set bottom(bottom: number){
        this._bottom = bottom
        this.setHeight()
        this.setCenterY()
    }



    // --------------- Getter ------------------

    get left(): number{
        return this._left
    }

    get right(): number{
        return this._right
    }

    get top(): number{
        return this._top
    }

    get bottom(): number{
        return this._bottom
    }

    get width(): number{
        return this._width
    }

    get height(): number{
        return this._height
    }

    get centerX(): number{
        return this._centerX
    }

    get centerY(): number{
        return this._centerY
    }



}
















</script>

<template>
    <div id="canvas-area">
        <canvas ref="canvas" width="700" height="400">Canvas Not Support</canvas>
    </div>
  
</template>

<style scoped>

#canvas-area{
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
}
canvas{
    border: 1px solid rgb(206, 206, 206);
}

</style>
