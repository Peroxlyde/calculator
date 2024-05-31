<template>
    <div class="calculator">
        <div class="answer">{{answer||""}}</div>
        <div class="display">{{calculation.join("") + current}}</div>
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
        <div @click="append('a')" class="btn">a</div>
        <div @click="equal" class="op">=</div>
        <div @click="append('b')" class="btn">b</div>
        <div @click="append('c')" class="btn">c</div>
        <div @click="append('d')" class="btn">d</div>
        <div @click="to2" class="op">Bin</div>
        <div @click="append('e')" class="btn">e</div>
        <div @click="append('f')" class="btn">f</div>
        <div @click="to8" class="op">Oct</div>
        <div @click="to10" class="op">Dec</div>
        <div @click="to16" class="op h">Hex</div>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                calculation:[],
                current:"",
                answer:"",
                opClick:true,
                //ตัวcalculationจะเก็บค่าหลังเรากดตัวopertator
                //opclick เช็คว่ากดopไปรึยัง
                openBracket:false,
                closeBracket:false,
                baseval:"",
                number:['.','9','8','5','7','6','5','4','3','2','1','('],
                dotclick:false,
                numclick:false,
                base1:10,
                base2:10,
                evaluate:[],
                evalclick:false
            }
        },
        methods:{
            clear(){
                this.current="";
                this.answer="";
                this.opClick=true;
                this.openBracket=false;
                this.closeBracket=false;
                this.baseval="";
                this.dotclick=false;
                this.numclick=false;
                this.calculation=[];
                this.base1=10;
                this.base2=10;
                this.evalclick=false
                this.evaluate=[];
            },
            opBrac(){
                if((this.numclick == false) && !(this.openBracket)){
                    this.calculation.push('(')
                    //this.current = `${this.current}${'('}`;
                    this.openBracket=true;
                    this.closeBracket=false;
                    this.opClick=false;
                }
            },
            clBrac(){
                if(this.openBracket && !(this.closeBracket) && (this.current !="" )){
                this.calculation.push(')')
                //this.current = `${this.current}${')'}`;
                this.closeBracket=true;
                this.openBracket=false;
                
                }
            },
            append(num){
                if(this.opClick){
                    this.current="";
                    this.opClick=false;
                }
                if(this.base1==2){
                    if(num=='1'||num=='0'||num=='.')
                    this.current =  `${this.current}${num}`;
                    this.numclick =true;
                }
                if(this.base1==8){
                    if(num=='0'||num=='1'||num=='2'||num=='3'||num=='4'||num=='5'||num=='6'||num=='7'||num=='.')
                    this.current =  `${this.current}${num}`;
                    this.numclick =true;
                }
                if(this.base1==10){
                    if(num=='0'||num=='1'||num=='2'||num=='3'||num=='4'||num=='5'||num=='6'||num=='7'||num=='8'||num=='9'||num=='.')
                    this.current =  `${this.current}${num}`;
                    this.numclick =true;
                }
                if(this.base1==16){
                    this.current =  `${this.current}${num}`;
                    this.numclick =true;
                }
                //this.current =  `${this.current}${num}`;
                ///this.numclick =true;
                //เช็คว่ากดopไปรึยังถ้ากดแล้วก็จะclearตัวในcurrentแล้วเปลี่ยนopclickเป็นfalse จึงค่อยเอาตัวที่เรากดไปต่อท้ายตัวที่มีอยู่
            },
            dot(){
                if(this.current.indexOf('.')===-1 && this.current[this.current.length-1]!=')'){
                    this.append('.');
                    this.dotclick = true;
                    this.numclick =true;
                    //เช็คว่ามี.รึ้ปล่าถ้าไม่มีให้เพิ่ม.ไปได้
                }
            },
            addcal(op){
               if(this.opClick === false && this.current[this.current.length-1]!='('&& this.current!=""){
                    this.calculation.push(this.current)
                    this.calculation.push(op)
                    this.current="";
                    this.opClick=true;
                    this.dotclick = false;
                    this.numclick = false;
                    this.evalclick = false;
                    //สำหรับเพิ่มตัวoperator เช็คว่ากดopไปรึยัง ถ้าไม่ก็ เก็บค่าcurrentพร้อมตัวopเข้าไปยังcalculation ล้วก็จะclearตัวในcurrentแล้วเปลี่ยนopclickเป็นtrue
                }
                else if(this.opClick === true){
                    this.calculation.pop()
                    this.calculation.push(op)
                }
            },
            ze(){
                if(this.current == ""){
                    this.append('0')
                }
                else if(this.number.some(el=>this.current.includes(el))) {
                    this.append('0')
                    /*this.current.includes('.')|| this.current.includes('(')||this.current.includes('7')|| this.current.includes('6')|| this.current.includes('5')|| this.current.includes('4')|| this.current.includes('3')|| this.current.includes('2')|| this.current.includes('1')
                    this.number.some(el=>this.current.includes(el)*/
                }
                else if(this.current[this.current.length-1]=='0.'){
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
                /*if(this.opClick===false && (this.current.indexOf('()')==-1) && (this.calculation.indexOf('()')==-1) && (this.calculation+this.current).includes('(00')==false){
                    this.answer= eval(this.calculation+this.current);
                    this.baseval = this.answer;
                    //evalทั้งตัวcalculationกับcurrentพร้อมกันแล้วเก็บค่าเข้าanswer
                    
                }
                else{
                    this.clear()
                    this.answer= "Error";
                }
                if(eval(this.calculation+this.current)==0){
                    this.answer= 0;
                    this.baseval = 0;
                }*/
                //if(this.evalclick==false){
                    
                try{
                    this.evaluate=Array.from(this.calculation)
                    for(let x = 0; x<this.evaluate.length;x++){
                    if (this.evaluate[x]!='('&& this.evaluate[x]!=')'&& this.evaluate[x]!='+'&& this.evaluate[x]!='-'&& this.evaluate[x]!='*'&& this.evaluate[x]!='/') {
                        this.evaluate[x]=parseInt(this.evaluate[x],this.base1).toString(10)
                        }
                    }
                    //this.answer =this.evaluate.join("")+this.current
                    //this.answer =this.evaluate.join("")+this.current
                    //this.answer= eval(this.evaluate.join("")+parseInt(this.current,this.base1).toString(10));
                    this.answer= parseInt(eval(this.evaluate.join("")+parseInt(this.current,this.base1).toString(10)),10).toString(this.base1);
                    this.baseval= this.answer
                    this.base2=this.base1
                    if(eval(this.evaluate.join("")+parseInt(this.current,this.base1).toString(10))== 0 ){this.answer='0'}
                }
                    
                catch(er){ this.clear()
                    this.answer= "Error";}
                    this.evalclick=true;
                //}
    
            },
            /*baseTobase(base){
                for(let i = 0; i<this.calculation.length;i++){
                    if (this.calculation[i]!='('&& this.calculation[i]!=')'&& this.calculation[i]!='+'&& this.calculation[i]!='-'&& this.calculation[i]!='*'&& this.calculation[i]!='/') {
                        this.calculation[i]=parseInt(this.calculation[i],base).toString(base)
                    }
                }
            },*/
            to2(){
                if(this.calculation!=[]){
                for(let i = 0; i<this.calculation.length;i++){
                    if (this.calculation[i]!='('&& this.calculation[i]!=')'&& this.calculation[i]!='+'&& this.calculation[i]!='-'&& this.calculation[i]!='*'&& this.calculation[i]!='/') {
                        this.calculation[i]=parseInt(this.calculation[i],this.base1).toString(2)
                    }
                }}
                if(this.current!=''){
                this.current=parseInt(this.current,this.base1).toString(2)}
                //this.evaluate=this.calculation
                if(this.answer!=''){
                this.answer= parseInt(this.baseval,this.base2).toString(2)}
                this.base1 = 2
                
            },
            to8(){
                if(this.calculation!=[]){
                for(let i = 0; i<this.calculation.length;i++){
                    if (this.calculation[i]!='('&& this.calculation[i]!=')'&& this.calculation[i]!='+'&& this.calculation[i]!='-'&& this.calculation[i]!='*'&& this.calculation[i]!='/') {
                        this.calculation[i]=parseInt(this.calculation[i],this.base1).toString(8)
                    }
                }}
                if(this.current!=''){
                this.current=parseInt(this.current,this.base1).toString(8)}
                //this.evaluate=this.calculation
                if(this.answer!=''){
                this.answer= parseInt(this.baseval,this.base2).toString(8)}
                this.base1 = 8
            },
            to10(){
                if(this.calculation!=[]){
                for(let i = 0; i<this.calculation.length;i++){
                    if (this.calculation[i]!='('&& this.calculation[i]!=')'&& this.calculation[i]!='+'&& this.calculation[i]!='-'&& this.calculation[i]!='*'&& this.calculation[i]!='/') {
                        this.calculation[i]=parseInt(this.calculation[i],this.base1).toString(10)
                    }
                }}
                if(this.current!=''){
                this.current=parseInt(this.current,this.base1).toString(10)}
                //this.evaluate=this.calculation
                if(this.answer!=''){
                this.answer= parseInt(this.baseval,this.base2).toString(10)}
                this.base1 = 10
            },
            to16(){
                if(this.calculation!=[]){
                for(let i = 0; i<this.calculation.length;i++){
                    if (this.calculation[i]!='('&& this.calculation[i]!=')'&& this.calculation[i]!='+'&& this.calculation[i]!='-'&& this.calculation[i]!='*'&& this.calculation[i]!='/') {
                        this.calculation[i]=parseInt(this.calculation[i],this.base1).toString(16)
                    }
                }}
                if(this.current!=''){
                this.current=parseInt(this.current,this.base1).toString(16)}
                //this.evaluate=this.calculation
                if(this.answer!=''){
                this.answer= parseInt(this.baseval,this.base2).toString(16)}
                this.base1 = 16
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
.h{
    grid-column: 1/5;
    
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