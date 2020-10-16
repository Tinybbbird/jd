<template>
	<div class="home_ban">
		<div class="m_banner clearfix" id="my_banner" @mouseenter="isPlaying=false"  @mouseleave="isPlaying=true" @touchstart="start()"
            @touchmove="move()" @touchend="end()"
        >
                <ul class="banner_box" :style="{marginLeft:`${-index*width}px`}" style="transition:.5s"  ref="liWidth">
                    <li v-for="(pic,i) in picList" :key="i"><img :src="require('../assets/'+pic.src)" alt=""></li>
                </ul>
                <ul class="point_box">
                    <li v-for="(pic,i) in picList" :key="i" :class="{now:index==i}" @mouseenter="moveTo(i)"></li>
                </ul>
            </div>
	</div>
</template>
<script>
export default {
    data:()=>{
        return{
            index:0,
            picList:[
                {src:"images/l8.jpg"},
                {src:"images/l1.jpg"},
                {src:"images/l2.jpg"},
                {src:"images/l3.jpg"},
                {src:"images/l4.jpg"},
                {src:"images/l5.jpg"},
                {src:"images/l6.jpg"},
                {src:"images/l7.jpg"},
            ],
            timer:null,
            isPlaying:true,
            width:0,
            startX:0,
            moveX:0,
            endX:0
        }
    },
    methods:{
        moveTo(i){
            this.index=i;
        },
    getWidth(){
        this.$nextTick(() => {  
            this.width=this.$refs.liWidth.offsetWidth/8;
            console.log(this.$refs.liWidth.offsetWidth); 
            })

    },
    start(e){
        var event=e||window.event;
        this.startX=event.touches[0].clientX;
    },
    move(e){
        this.isPlaying=false;
        var event=e||window.event;
        event.preventDefault();
        this.endX=event.touches[0].clientX;
        this.moveX=this.startX-this.endX;
    },
    end(){
        if(Math.abs(this.moveX)>(1/3*this.width)&&this.endX!=0){
            if(this.moveX>0){
                this.index++;
            }else   
                this.index--;
            if(this.index>=this.picList.length){
                this.index=0
            }else if(this.index<=0){
                this.index=this.picList.length-1
            }
        }
        this.startX=0;
        this.endX=0;
        this.isPlaying=true;
    }
    },
    mounted(){
        this.getWidth();
        this.timer=setInterval(()=>{
            if(this.isPlaying){
                this.index++
                if(this.index>=this.picList.length){
                this.index = 0
            }
            }
        },2000);
        
    },   
    destroyed(){
        clearInterval(this.timer)
    }
}
</script>

<style>

</style>