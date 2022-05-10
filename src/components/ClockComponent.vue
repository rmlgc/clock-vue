<script setup>
import { onMounted,reactive,computed } from 'vue'

const props = defineProps({
    efect: {
        type: String,
        required: true,
        validator(value) {
        // The value must match one of these strings
        console.log
            let isValidate = ['opacity', 'cubeX', 'cubeY', 'flip', 'slide'].includes(value);
            if(isValidate ===false)
            console.warn(`efect required once of this -> 'opacity', 'cubeX', 'cubeY', 'flip', 'slide'`);

            return isValidate
        },
        default() {
            return 'opacity'
        }
    }
})

    let obj = reactive({
        date:0,
        hour:0, 
        min:0, 
        second:0,
        actualHour:0,
        actualMin:0,
        actualSecond:0,
        isChangedSecond:true,
        isChangedMinute:true,
        isChangedHour:true
    });
    obj.date = new Date();
    obj.actualHour = String(obj.date.getHours()).padStart(2, '0');;
    obj.actualMin = String(obj.date.getMinutes()).padStart(2, '0');;
    obj.actualSecond = String(obj.date.getSeconds()).padStart(2, '0');;
    clockStart()
    setInterval(() => clockStart(), 1000)

    function numberChanged(actual, newNumber){
        return actual !== newNumber ? newNumber : actual;
    }
    function isChanged(actual, newNumber, state){
        return actual !== newNumber ? !state : state ;
    }
    function clockStart(){
        obj.date = new Date();
        obj.hour = String(obj.date.getHours()).padStart(2, '0');
        obj.min = String(obj.date.getMinutes()).padStart(2, '0');
        obj.second = String(obj.date.getSeconds()).padStart(2, '0');

        obj.isChangedHour =isChanged(obj.actualHour, obj.hour ,obj.isChangedHour);
        obj.isChangedMinute = isChanged(obj.actualMin, obj.min, obj.isChangedMinute );
        obj.isChangedSecond = isChanged(obj.actualSecond, obj.second, obj.isChangedSecond );

        obj.actualHour =numberChanged(obj.actualHour, obj.hour );
        obj.actualMin = numberChanged(obj.actualMin, obj.min);
        obj.actualSecond = numberChanged(obj.actualSecond, obj.second );        
    }

</script>
<template>
<h1>{{$props.efect === 'opacity' ? $props.efect + ' - default option' : $props.efect}}</h1>
    <div class="clock__bg">
        <p class="clock">
            <span class="clock-block">
                <Transition :name=$props.efect>
                    <span class="clock-nunber clock-nunber__seconds" v-if="obj.isChangedHour">
                        {{obj.hour}}
                    </span> 
                </Transition>
                <Transition :name=$props.efect>
                    <span class="clock-nunber clock-nunber__seconds" v-if="!obj.isChangedHour">
                        {{obj.hour}}
                    </span> 
                </Transition>
            </span>
            <span class="clock-block">
                :
            </span>
            <span class="clock-block">
                <Transition :name=$props.efect>
                    <span class="clock-nunber clock-nunber__seconds" v-if="obj.isChangedMinute">
                        {{obj.min}}
                    </span> 
                </Transition>
                <Transition :name=$props.efect>
                    <span class="clock-nunber clock-nunber__seconds" v-if="!obj.isChangedMinute">
                        {{obj.min}}
                    </span> 
                </Transition>
            </span>
            <span class="clock-block">
                :
            </span>
            <span class="clock-block">
                <Transition :name=$props.efect>
                    <span class="clock-nunber clock-nunber__seconds" v-if="obj.isChangedSecond">
                        {{obj.second}}
                    </span> 
                </Transition>
                <Transition :name=$props.efect>
                    <span class="clock-nunber clock-nunber__seconds" v-if="!obj.isChangedSecond">
                        {{obj.second}}
                    </span> 
                </Transition>
            </span>
        </p>
    </div>
</template>
<style lang="scss" >
    .clock{
        font-size: 3rem;
        line-height: 0.9;
        width:100%;
        display:flex;
        gap: 8px;
        height:100%;
        justify-content: center;
        align-items: center;
        overflow:hidden;
        &__bg{
            background-color: rgb(201, 255, 181);
        }
        &-block{
            height:50vh;
            position:relative;
            flex:100%;
            max-width:300px;
            min-height:400px;
            display:inline-flex;
            align-items:center;
            justify-content:center;
        }
        &-nunber{
            // position:absolute;
                display:inline-flex;
            &__seconds{
            }
        }
    }
    .slide-enter-active {
        transition: all 1s ease-in-out;
    }

    .slide-leave-active {
        transition: all 1s ease-in-out;
    }

    .slide-enter-from{
        transform: translateY(-100%);
        color:cyan;
        position:absolute;
    }
    .slide-leave-to {
        transform: translateY(100%);
        color:red;
        opacity: 0;
        position:absolute;
        
    }

    .cubeY-enter-active {
        transition: all 1s ease-in-out;
        transform:matrix(1,0,0,1,0,0)
    }

    .cubeY-leave-active {
        transition: all 1s ease-in-out;
        transform:matrix(1,0,0,1,0,0)
    }

    .cubeY-enter-from{
        transform: translateY(0%);
        color:cyan;
        opacity: 0;
        position:absolute;
        transform:matrix(1,0,0,0.01,0,-100);
    }
    .cubeY-leave-to {
        transform: translateY(0%);
        color:red;
        opacity: 0;
        position:absolute;
        transform:matrix(1,0,0,0.01,0,100)
    }

    .cubeX-enter-active {
        transition: all 1s ease-in-out;
        transform:matrix(1,0,0,1,0,0)
    }

    .cubeX-leave-active {
        transition: all 1s ease-in-out;
        transform:matrix(1,0,0,1,0,0)
    }

    .cubeX-enter-from{
        transform: translateY(0%);
        color:cyan;
        opacity: 0;
        position:absolute;
        transform:matrix(0.0001,0,0,1,-100,0);
    }
    .cubeX-leave-to {
        transform: translateY(0%);
        color:red;
        opacity: 0;
        position:absolute;
        transform:matrix(0.0001,0,0,1,100,0)
    }

    .opacity-enter-active {
        transition: all 1s ease-in-out;
        
    }

    .opacity-leave-active {
        transition: all 1s ease-in-out;
        
    }

    .opacity-enter-from{
        transform: translateY(0%);
        color:cyan;
        opacity: 0;
        position:absolute;
        
    }
    .opacity-leave-to {
        transform: translateY(0%);
        color:red;
        opacity: 0;
        position:absolute;
        
    }

    .flip-enter-active {
        transition: all 1s ease-in-out;
        clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%);
                transform:matrix(1,0,0,1,0,0)
    }

    .flip-leave-active {
        transition: all 0.8s ease-in-out;
        // clip-path: polygon(100% 0%, 100% 100%, 0% 100%, 0% 0%);
                transform:matrix(1,0,0,1,0,0)
    }

    .flip-enter-from{
        color:cyan;
        opacity: 0;
        position:absolute;
        // clip-path: polygon(0% 50%, 100% 50%, 100% 100%, 0% 100%);
        clip-path: polygon(0% 0%, 100% 0%, 100% 50%, 0% 50%);
        transform:matrix(1,0,0,0.01,0,-60)
    }
    .flip-leave-to {
        color:red;
        opacity: 0;
        position:absolute;
        // transform:matrix(1,0,0,0.01,0,60)
    }


</style>