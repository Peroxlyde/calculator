<template>
    <div class="calculator">
        <div class="answer">{{answer||""}}</div>
        <div class="display">{{calculation+current}}</div>
        <div @click="clear" class="op">C</div>
        <div @click="opBrac" class="bracket">( </div>
        <div @click="clBrac" class="bracket">) </div>
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
        <div @click="to2" class="op">Bin</div>
        <div @click="to8" class="op">Oct</div>
        <div @click="to10" class="op">Dec</div>
        <div @click="to16" class="op">Hex</div>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                calculation:"",
                current:"",
                answer:"",
                opClick:true,
                //ตัวcalculationจะเก็บค่าหลังเรากดตัวopertator
                //opclick เช็คว่ากดopไปรึยัง
                openBracket:false,
                closeBracket:false,
                baseval:""
            }
        },
        methods:{
            clear(){
                this.current="";
                this.answer="";
                this.calculation="";
                this.opClick=true;
                this.openBracket=false;
                this.closeBracket=false;
                this.baseval="";

            },
            opBrac(){
                if((this.current=="") && !(this.openBracket)){
                    this.append('(');
                    this.openBracket=true;
                    this.closeBracket=false;
                }
            },
            clBrac(){
                if(this.openBracket && !(this.closeBracket) && (this.current !="" )){
                this.current = `${this.current}${')'}`;
                this.closeBracket=true;
                this.openBracket=false;
                
                }
            },
            append(num){
                if(this.opClick){
                    this.current="";
                    this.opClick=false;
                }
                this.current =  `${this.current}${num}`;
                //เช็คว่ากดopไปรึยังถ้ากดแล้วก็จะclearตัวในcurrentแล้วเปลี่ยนopclickเป็นfalse จึงค่อยเอาตัวที่เรากดไปต่อท้ายตัวที่มีอยู่
            },
            dot(){
                if(this.current.indexOf('.')===-1){
                    this.append('.');
                    //เช็คว่ามี.รึ้ปล่าถ้าไม่มีให้เพิ่ม.ไปได้
                }
            },
            addcal(op){
                if(this.opClick === false){
                    this.calculation +=  `${this.current}${op}`;
                    this.current="";
                    this.opClick=true;
                    //สำหรับเพิ่มตัวoperator เช็คว่ากดopไปรึยัง ถ้าไม่ก็ เก็บค่าcurrentพร้อมตัวopเข้าไปยังcalculation ล้วก็จะclearตัวในcurrentแล้วเปลี่ยนopclickเป็นtrue
                }
                else if(this.opClick === true){
                    this.calculation = this.calculation.slice(0,-1)
                    this.calculation = `${this.calculation}${op}`;
                }
            },
            ze(){
                if(this.current == ""){
                    this.append('0')
                }
                else if(this.current.includes('.')|| this.current.includes('(')||this.current.includes('7')|| this.current.includes('6')|| this.current.includes('5')|| this.current.includes('4')|| this.current.includes('3')|| this.current.includes('2')|| this.current.includes('1') ) {
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
                if(this.opClick===false && (this.current.indexOf('()')==-1) && (this.calculation.indexOf('()')==-1)){
                    this.answer= eval(this.calculation+this.current);
                    this.baseval = this.answer;
                    //evalทั้งตัวcalculationกับcurrentพร้อมกันแล้วเก็บค่าเข้าanswer
                    
                }
                else{
                    this.clear()
                    this.answer= "Error";
                }
            },
            decTobase(base){
                this.answer = this.baseval.toString(base)
            },
            to2(){
                this.decTobase(2)
            },
            to8(){
                this.decTobase(8)
            },
            to10(){
                this.decTobase(10)
            },
            to16(){
                this.decTobase(16)
            }
        }
        // margin: 0 auto = top and bottom margins are 0  right and left margins are auto
    }
</script>
  
<style scoped>
.calculator{ 
    border: 1px solid black;
    padding: 30px;
    margin: 0 auto; 
    width: 450px;
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
.op,.bracket{
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
.op:hover,.bracket:hover{
    cursor: pointer;
    background-color: rgb(177, 222, 250);
}
.btn:active,.zero:active,.op:active,.bracket:active{
    transform: translate(0.05rem,0.05rem);
}

</style>