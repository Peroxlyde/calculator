<template>
    <div class="calculator">
        <div class="answer">{{answer||""}}</div>
        <div class="display">{{calculation+current}}</div>
        <div @click="clear" class="op">C</div>
        <div @click="sign" class="op">+/-</div>
        <div @click="percent" class="op">%</div>
        <div @click="divide" class="op">/</div>
        <div @click="append('7')" class="btn">7</div>
        <div @click="append('8')" class="btn">8</div>
        <div @click="append('9')" class="btn">9</div>
        <div @click="multiply" class="op">*</div>
        <div @click="append('4')" class="btn">4</div>
        <div @click="append('5')" class="btn">5</div>
        <div @click="append('6')" class="btn">6</div>
        <div @click="minus" class="op">-</div>
        <div @click="append('1')" class="btn">1</div>
        <div @click="append('2')" class="btn">2</div>
        <div @click="append('3')" class="btn">3</div>
        <div @click="plus" class="op">+</div>
        <div @click="ze" class="zero">0</div>
        <div @click="dot" class="btn">.</div>
        <div @click="equal" class="op">=</div>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                calculation:"",
                current:"",
                answer:"",
                opClick:true
                
            }
        },
        methods:{
            clear(){
                this.current="";
                this.answer="";
                this.calculation="";
                this.opClick=true;

            },
            sign(){
                if(this.current != ""){
                this.current = this.current.charAt(0)=== "-" ?
                    this.current.slice(1) : `-${this.current}`;
                    
                }
            },
            percent(){
                if(this.current != ""){
                this.current = `${parseFloat(this.current)/100}`;
                
                }
            },
            append(num){
                if(this.opClick){
                    this.current="";
                    this.opClick=false;
                }
                this.current =  `${this.current}${num}`;
               
            },
            dot(){
                if(this.current.indexOf('.')===-1){
                    this.append('.');
                    
                }
            },
            addcal(op){
                if(this.opClick === false){
                    this.calculation +=  `${this.current}${op}`;
                    this.current="";
                    this.opClick=true;
                    
                }
            },
            ze(){
                if(this.current == ""){
                    this.append('0')
                }
                else if(this.current.includes('.') ) {
                    this.append('0')
                }
            },
            divide(){
                this.addcal('/')
            },
            multiply(){
                this.addcal('*')
            },
            minus(){
                this.addcal('-')
            },
            plus(){
                this.addcal('+')
            },
            equal(){
                if(this.opClick===false){
                    this.answer= eval(this.calculation+this.current);
                    
                    
                }
            },
            
        }
    
    }
</script>
  
<style scoped>
.calculator{ 
    border: 1px solid black;
    padding: 20px;
    margin: 0 auto; 
    width: 400px;
    font-size: 40px;
    display: grid;
    grid-template-columns: repeat(4,1fr);
    grid-auto-rows: minmax(60px, auto);
    grid-gap: 12px;
    background-color: rgb(233, 227, 227);
}
.answer,.display{
    grid-column: 1/5;
    display: flex;
    align-items: center;
}
.display{
    font-size: 20px;
    margin-bottom: 10px;
    border-bottom: 1px solid black;
}
.zero{
    grid-column: 1/3;
}
.btn,.zero{
    background-color: whitesmoke;
    border: 1px solid black;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: center;
}
.op{
    background-color: lightskyblue;
    border: 1px solid black;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: center;
}
.btn:hover,.zero:hover{
    cursor: pointer;
    background-color: white;
    
}
.op:hover{
    cursor: pointer;
    background-color: rgb(177, 222, 250);
}
.btn:active,.zero:active,.op:active{
    transform: translate(0.05rem,0.05rem);
}

</style>