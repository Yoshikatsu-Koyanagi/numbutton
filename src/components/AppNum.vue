<template>
   <canvas class="canvas" id="cv3" ref="canv3" :width="width" :height="height"></canvas>
</template>


<style scoped>

 .canvas{
 }
</style>


<script>
export default { 
  props: ["width","height","number"],
  data() {
    return {
      num_width_ratio: 0.8,
      num_height_ratio: 0.5,
      flame_weight_ratio: 0.01, //キャンバスの横幅に対するボタンの枠の太さの割合
      font_ratio: 0.8, //数字ボタンの高さに対するフォントサイズの割合
      font_name: "meirio", //フォントの書体
      r: 10, //数字ボタンの枠の角の半径
    }
  },
  methods: {
    draw_1_btn(num_color){
      //数字ボタンの枠
      this.context3.globalCompositeOperation = "destination-out";
      this.context3.beginPath();
      this.context3.moveTo(this.num_side_margin+this.r,this.num_top_margin);
      this.context3.arc(this.num_side_margin+this.r, this.num_top_margin+this.r, this.r, Math.PI*1.5, Math.PI, true);  
      this.context3.lineTo(this.num_side_margin, this.num_top_margin+this.num_height-this.r);
      this.context3.arc(this.num_side_margin+this.r, this.num_top_margin+this.num_height-this.r, this.r, Math.PI, Math.PI*0.5, true); 
      this.context3.lineTo(this.num_side_margin+this.num_width-this.r,this.num_top_margin+this.num_height);
      this.context3.arc(this.num_side_margin+this.num_width-this.r, this.num_top_margin+this.num_height-this.r, this.r, Math.PI*0.5, Math.PI*0, true); 
      this.context3.lineTo(this.num_side_margin+this.num_width,this.num_top_margin+this.r);
      this.context3.arc(this.num_side_margin+this.num_width-this.r, this.num_top_margin+this.r, this.r, Math.PI*0, Math.PI*1.5, true); 
      this.context3.closePath();
      this.context3.fillStyle = "rgba(0,0,0,1)";
      this.context3.fill();

      this.context3.globalCompositeOperation = "source-over";
      this.context3.fillStyle = num_color;
      this.context3.fill();

      //数字ボタンの"数字"
      this.context3.font = this.font;
      this.context3.fillStyle = "rgba(0,0,0,1)";
      this.context3.textAlign = "center";
      this.context3.fillText(this.number, this.nbw*0.5, this.num_top_margin+this.num_height*0.8,this.num_width);
    
      this.context3.lineWidth = this.flame_weight;	
      this.context3.strokeStyle = "rgb(0,0,0)"; 
      this.context3.stroke();
    },
  },
  mounted() {
    this.flame_weight = this.width*this.flame_weight_ratio //ボタンの枠の太さ

    this.nbw = this.width;
    this.nbh = this.height;
 
    
    this.num_width = this.nbw*this.num_width_ratio;
    this.num_height = this.nbh*this.num_height_ratio;
    this.num_side_margin = this.nbw*(1-this.num_width_ratio)*0.5;
    this.num_top_margin = this.nbh*(1-this.num_height_ratio)*0.5;

   
    let c = this.$refs.canv3;
    this.context3 = c.getContext('2d');
  

    this.gradient3_b = this.context3.createLinearGradient(this.nbw*0.5, 0, this.nbw*0.5, this.nbh);
    this.gradient3_b.addColorStop(0.0 , 'rgba(0,50,255,0.05)');
    this.gradient3_b.addColorStop(0.5 , 'rgba(0,50,255,0.125)');
    this.gradient3_b.addColorStop(1.0 , 'rgba(0,50,255,0.25)');
    this.context3.fillStyle = this.gradient3_b;
    this.context3.fillRect(0,0,this.nbw,this.nbh);

  
    
    //数字ボタンを作成
    this.font_size = this.num_height*this.font_ratio; //フォントサイズ
    this.font = `${this.font_size}px ${this.font_name}`; //フォントサイズと書体    
    let num_color; //数字ボタンの色
    this.gradient3 = this.context3.createLinearGradient(this.nbw*0.5,this.num_top_margin,this.nbw*0.5,this.num_top_margin+this.num_height);
    this.gradient3.addColorStop(0.0 , 'rgba(0,0,0,0.35)');
    this.gradient3.addColorStop(0.8 , 'rgba(0,0,0,0.05)');
    num_color = this.gradient3;
    this.draw_1_btn(num_color);
    let num_down = false;

    c.onmousedown = (e) => {
          let gradient6 = this.context3.createLinearGradient(this.nbw*0.5,this.num_top_margin,this.nbw*0.5,this.num_top_margin+this.num_height);
          gradient6.addColorStop(0.0 , 'rgba(0,0,0,0.5)');
          gradient6.addColorStop(0.8 , 'rgba(0,0,0,0.3)');
          num_color = gradient6;
          this.draw_1_btn(num_color);
          this.$emit("numclick");
          return num_down = true;
    }
  
    
    c.onmouseup = (e) => {
        num_color = this.gradient3;
        this.draw_1_btn(num_color);  
    } 
        //数字ボタンが押されて且つ数字ボタン上で離されたとき        
    if (num_down == true) {
      
    }
  
    c.onmouseout = (e) => {
        num_color = this.gradient3;
        this.draw_1_btn(num_color);
    }       
  }
}
</script>