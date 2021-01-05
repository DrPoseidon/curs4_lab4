<template>
  <div class="v-main-component">
    <div class="error_div" v-if="error">
      <span class="error_text">Проверьте правильность введенных значений</span>
    </div>
    <div class="inputs">
      <div class="input1">
      <span>Сд = </span><input type="number" v-model="c_d">
      </div>
      <div class="input1">
      <span>Сх = </span><input type="number" v-model="c_x">
      </div>
      <div class="input1">
      <span>Т = </span><input type="number" v-model="T">
      </div>
      <div class="input1">
      <span>Wmax = </span><input type="number" v-model="w_max">
      </div>
      <div class="input1">
      <span>Wmin = </span><input type="number" v-model="w_min">
      </div>
      <div class="input1">
      <span>tср = </span><input type="number" v-model="t_avg">
      </div>
      <div class="input1">
      <span>p = </span><input type="number" v-model="risk">
      </div>
      <div class="input1">
      <span>tпр = </span><input type="number" v-model="t_pr">
      </div>
      <button @click="solution">Решение</button>
      <button @click="testSolution(100,5,100,1100,900,3,0.1,2)">Тест 1</button>
                          <!-- Сд Сх Т Wmax Wmin tср p tпр -->
      <button @click="testSolution(100,5,100,10000,1000,1,0.05,0)">Тест 2</button>
    </div>
    <div class="results" v-if="solut">
      <div class="result1">
        <span>Vmax = </span><span>{{v_max}}</span>
      </div>
      <div class="result1">
        <span>Vmin = </span><span>{{v_min}}</span>
      </div>
      <div class="result1">
        <span>k = </span><span>{{k}}</span>
      </div>
      <div class="result1">
        <span>b = </span><span>{{b}}</span>
      </div>
      <div class="result1">
        <span>P = </span><span>{{p}}</span>
      </div> 
      <div class="result1">
        <span>Q = </span><span>{{q}}</span>
      </div>   
      <div class="result1">
        <span>S = </span><span>{{s}}</span>
      </div>                       
    </div>

  </div>
</template>

<script>
export default {
  data(){
    return{
      c_d: null,
      c_x: null,
      T: null,
      w_max: null,
      w_min: null,
      t_avg: null,
      risk: null,
      t_pr: null,
      v_max: null,
      v_min: null,
      p_hatch: null,
      k: null,
      b: null,
      p: null,
      q: null,
      s: null,
      solut: false,
      error: false,
    }
  },
  methods:{
    solution(){
      if(this.cd !== null && this.c_x !== null && this.T !== null 
      && this.w_max !== null && this.w_min !== null 
      && this.t_avg !== null && this.risk !== null && this.t_pr !== null){
        const c_d = this.c_d;
        const c_x = this.c_x;
        const T = this.T;
        const w_max = this.w_max;
        const w_min = this.w_min;
        const t_avg = this.t_avg;
        const risk = this.risk;
        const t_pr = this.t_pr;

        const w_avg = (w_max + w_min) / 2;
        const v_max = w_max * t_avg;
        const v_min = w_min * t_avg;
        const p_hatch = (v_max +v_min) / 2;
        const k = Math.sqrt(12) * (0.5 - risk);
        const b = (w_max * t_avg  - w_min * t_avg) / Math.sqrt(12) * k;
        const p = p_hatch + b;
        const q = Math.sqrt(2 * w_avg *c_d /c_x);
        const s = w_avg * c_d / q + q * c_x * T / 2;
        this.v_max = v_max.toFixed(2);
        this.v_min = v_min.toFixed(2);
        this.p_hatch = p_hatch.toFixed(2);
        this.k = k.toFixed(2);
        this.b = b.toFixed(2);
        this.p = p.toFixed(2);
        this.q = q.toFixed(2);
        this.s = s.toFixed(2);
        console.log(t_pr,p,s)
        this.solut = true;
      } else this.error = true;
    },
    testSolution(c_d,c_x,T,w_max,w_min,t_avg,risk,t_pr){
    this.c_d = c_d;
    this.c_x = c_x;
    this.T = T;
    this.w_max = w_max;
    this.w_min = w_min;
    this.t_avg = t_avg;
    this.risk = risk;
    this.t_pr = t_pr;

    const w_avg = (w_max + w_min) / 2;
    const v_max = w_max * t_avg;
    const v_min = w_min * t_avg;
    const p_hatch = (v_max +v_min) / 2;
    const k = Math.sqrt(12) * (0.5 - risk);
    const b = (w_max * t_avg  - w_min * t_avg) / Math.sqrt(12) * k;
    const p = p_hatch + b;
    const q = Math.sqrt(2 * w_avg *c_d /c_x);
    const s = w_avg * c_d / q + q * c_x * T / 2;
    this.v_max = v_max.toFixed(2);
    this.v_min = v_min.toFixed(2);
    this.p_hatch = p_hatch.toFixed(2);
    this.k = k.toFixed(2);
    this.b = b.toFixed(2);
    this.p = p.toFixed(2);
    this.q = q.toFixed(2);
    this.s = s.toFixed(2);
    console.log(t_pr,p,s)
    this.solut = true;
    }
  }
}
</script>

<style>
.v-main-component{
  display: flex;
  flex-direction: column;
  align-items: center;
}
.inputs{
  align-items:center;
  display: flex;
  flex-direction: column;
  border: 2px solid black;
  padding: 20px;
  width: 300px;
}
input{
  margin: 5px;
}
.input1{
  justify-content: space-between;
  align-items: center;
  display: flex;
  width: 250px;
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    /* display: none; <- Crashes Chrome on hover */
    -webkit-appearance: none;
    margin: 0; /* <-- Apparently some margin are still there even though it's hidden */
}
.results{
  border: 2px solid black;
  padding: 20px;
  width: 300px;
  margin-top: 10px;
}
button{
  margin: 5px;
  width: 100px;
}
.error_text{
  color: red;
  font-size: 20px;
}
.error_div{
  border: 2px solid red;
  padding: 5px;
  margin-bottom: 5px;
}
</style>