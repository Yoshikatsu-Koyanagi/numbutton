# numbutton
‣example

----index.html----
````
<div id="AppPlus">
      <mycomp
      :width = 500
      :height = 300
      :number = 5 //表示する数字
      @numclick="nmu"></mycomp> //@numclick: ボタンが押されたら発火
</div>
  ````
  
  ----index.js----
  ````
import Vue from 'vue';
import AppNum from 'numbutton';

new Vue({
    el: '#AppNum',
    components: {
        'mycomp': AppNum
    },
    methods: {
        num(){
            console.log("num")
        }
    }
})
````
