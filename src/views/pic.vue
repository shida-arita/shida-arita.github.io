<template>
<ion-page>
    <ion-header>
        <ion-toolbar>
            <ion-segment @ionChange="nowTab=$event.target.value" :value="nowTab">
                <ion-segment-button value="poster">
                    封面图片生成
                </ion-segment-button>
                <ion-segment-button value="qrcode">
                    二维码生成
                </ion-segment-button>
            </ion-segment>
        </ion-toolbar>
    </ion-header>
    <template v-if="nowTab==='poster'">
    <ion-content v-if="resImg===''">
        <ion-item lines>
            <ion-label>边框：</ion-label>
            <ion-select :style="'background:'+boColor" @ionChange="boColor=$event.detail.value" :value="boColor" placeholder="颜色">
                    <ion-select-option v-for="(color,idx) in colors" :key="idx" :value="color">
                        {{color}}
                    </ion-select-option>
            </ion-select>
            <ion-select @ionChange="radius=$event.detail.value" :value="radius" placeholder="圆角">
                    <ion-select-option v-for="(size,idx) in sizes" :key="idx" :value="size">
                        {{size}}
                    </ion-select-option>
            </ion-select>
        </ion-item>
        <ion-item lines="none">
                <ion-label>标题：</ion-label>
                <ion-input @ionChange="title=$event.detail.value" :value="title" placeholder="请输入商品标题"></ion-input>
                <ion-select :style="'background:'+tColor" @ionChange="tColor=$event.detail.value" :value="tColor" placeholder="颜色">
                    <ion-select-option v-for="(color,idx) in colors" :key="idx" :value="color">
                        {{color}}
                    </ion-select-option>
                </ion-select>
                <ion-select @ionChange="tSize=$event.detail.value" :value="tSize" placeholder="大小">
                    <ion-select-option v-for="(size,idx) in sizes" :key="idx" :value="size">
                        {{size}}
                    </ion-select-option>
                </ion-select>
            </ion-item>
            <ion-item >
                <ion-checkbox :checked="tShadow" @ionChange="tShadow=$event.detail.checked"/><ion-label>阴影</ion-label>
                
                <ion-checkbox :checked="tWeight" @ionChange="tWeight=$event.detail.checked"/><ion-label>粗体：</ion-label>
                <ion-select @ionChange="tFont=$event.detail.value" :value="tFont" placeholder="字体">
                    <ion-select-option v-for="(font,idx) in fonts" :key="idx" :value="font.value">
                        {{font.label}}
                    </ion-select-option>
                </ion-select>
            </ion-item>
            <ion-item lines="none">
                <ion-label>背景图片：</ion-label>
                <input type="file" @change="chooseImg"/>
                <ion-button @click="imgUrl=''">清除</ion-button>
            </ion-item>
             <ion-item lines="none">
                <ion-label>背景颜色：</ion-label>
                <ion-select :style="'background:'+bColor" @ionChange="bColor=$event.detail.value" :value="bColor" placeholder="颜色">
                    <ion-select-option v-for="(color,idx) in colors" :key="idx" :value="color">
                        {{color}}
                    </ion-select-option>
                </ion-select>
            </ion-item>
            <ion-item>
                <ion-label>背景不透明度：</ion-label>
                <ion-input :value="opacity" @ionChange="opacity=($event.detail.value<=1 && $event.detail.value>=0)?$event.detail.value:opacity" type="number" max="1" min="0" step="0.1"/>
            </ion-item>
            <ion-item lines="none">
                <ion-input @ionChange="content=$event.detail.value" placeholder="内容描述：请用 ' ; ' 号分隔"/>
                <ion-button @click="list=content.match(/;/)?content.split(';'):content.split('；')" slot="end" expand="full" color="primary">应用</ion-button>
            </ion-item>
            <ion-item>
                <ion-select :style="'background:'+cColor" @ionChange="cColor=$event.detail.value" :value="cColor" placeholder="颜色">
                    <ion-select-option v-for="(color,idx) in colors" :key="idx" :value="color">
                        {{color}}
                    </ion-select-option>
                </ion-select>
                <ion-select @ionChange="cSize=$event.detail.value" :value="cSize" placeholder="大小">
                    <ion-select-option v-for="(size,idx) in sizes" :key="idx" :value="size">
                        {{size}}
                    </ion-select-option>
                </ion-select>
                <ion-select @ionChange="cFont=$event.detail.value" :value="cFont" placeholder="字体">
                    <ion-select-option v-for="(font,idx) in fonts" :key="idx" :value="font.value">
                        {{font.label}}
                    </ion-select-option>
                </ion-select>
                <ion-select @ionChange="icon=$event.detail.value" :value="icon" placeholder="图标">
                    <ion-select-option v-for="(ic,idx) in icons" :key="idx" :value="ic.value">
                        {{ic.label}}
                    </ion-select-option>
                </ion-select>
            </ion-item>
            <ion-item lines="none">
                <ion-label>授权店铺名：</ion-label>
                <ion-input @ionChange="name=$event.detail.value" placeholder="请输入店铺名"/>
            </ion-item>
        <div id="board" class="background" :style="'background:'+boColor">
            <ion-card  class="card" :style="'--background:'+bColor+';border-radius:'+radius+'px'">
                <img :style="{opacity:opacity}" @load="removeUrl(imgUrl)" v-if="imgUrl" class="cardPic" :src="imgUrl"/>
                <ion-card-header class="cardHeader">
                    <ion-card-title>
                        <h1 :style="'font-family:\''+tFont+'\',sans-serif;color:'+tColor+';font-size:'+tSize+'px;'+(tShadow?'text-shadow:2px 2px 3px #8696a7;':'')+(tWeight?'font-weight: bold;':'')">{{title}}</h1>
                    </ion-card-title>
                </ion-card-header>
                <ion-card-content>
                    <ion-label :style="'height:'+cSize*1.6+'px'" v-for="(item,idx) in list" :key="idx" ><h2 :style="'font-family:'+cFont+';color:'+cColor+';font-size:'+cSize+'px'"><ion-icon style="margin-right:0.4em" :icon="iconList[icon]"/>{{item}}</h2></ion-label>
                </ion-card-content>
                <div class="shuiyin" style="opacity: 0.4">
                    <img class="icon" src='@/image/shida.jpg'/>
                    <span style="position:absolute;left:80px;line-height:60px;white-space:nowrap">授权销售：{{name}}</span>
                </div>
            </ion-card>
        </div>
    </ion-content>
    <ion-content v-else>
        <img @load="removeUrl(resImg)" :src="resImg"/>
    </ion-content>
    </template>
    <ion-footer>
        <ion-row v-if="resImg===''">
            <ion-col size="6">
                <ion-item @click="generate" button color="primary">
                    <ion-title class="textC" >生成图片</ion-title>
                </ion-item>
            </ion-col>
            <ion-col size="6">
                <ion-item @click="saveoption" button color="primary">
                    <ion-title class="textC" >保存内容</ion-title>
                </ion-item>
            </ion-col>
        </ion-row>
        <ion-item v-else @click="resImg=''" button color="primary">
            <ion-title class="textC" >返回</ion-title>
        </ion-item>
    </ion-footer>
</ion-page>
</template>
<style scoped>
.background{
    
    height:400px;
    width:400px;
    
    padding-top:30px;
    padding-bottom:30px;
    padding-left:30px;
    padding-right:30px;
}
.card{
    position: relative;
    height:340px;
    width:340px;
    margin:0;
}
.cardHeader{
    padding-bottom:0
}
.cardPic{
    position: absolute;
    top:0px;
    bottom:0px;
    left:0px;
    right:0px;
    height:340px;
    width:340px;
    object-fit: cover;
}
.icon{
    margin:10px 20px;height:40px;width: 40px;border-radius: 20px;
}
.shuiyin{
    position: absolute;
    bottom:0px;
    right:0px;
    width:100%;
    background:#fff;
    height:60px;
    color:#888;
    font-size:16px;
    font-style:italic;
    font-family: 'Monospace', sans-serif;
    font-weight: bold;
}
.textC{
    text-align:center;
    
}
.textS{
    text-shadow: 2px 2px 3px #8696a7;
}
.textW{
    font-weight: bold;
}
h1{
    white-space: nowrap;
}
h2{
    white-space: nowrap;
}
</style>
<script lang="ts">
import { IonPage, IonHeader, IonContent, IonToolbar, IonFooter, IonCard, IonCardContent, IonCardHeader } from '@ionic/vue';
import { defineComponent, onMounted, onUnmounted, reactive, ref, toRefs } from 'vue';
import { starSharp, checkmarkCircle, flashSharp, thumbsUpSharp } from 'ionicons/icons';
import { getEntity, downEntity, saveEntity } from '@/api/api'
import { useRouter } from 'vue-router';
import html2canvas from "html2canvas"
import { setOptions, getOptions } from "@/utils/cookies"
export default defineComponent({
  name: 'App',
  components: {
      IonCard, IonCardContent, IonCardHeader, IonPage, IonContent, IonHeader
  },
  setup() {
      const cookieState = getOptions()
      let stateObj = {
          title:'明日之后钢琴辅助',
          opacity:0.4,
          content:'',
          tColor:'#fffaf4',
          bColor:'#c1cbd7',
          boColor:'#8696a7',
          tSize:38,
          tFont:'KaiTi',
          cColor:'#fffaf4',
          tShadow:true,
          tWeight:true,
          cSize:30,
          cFont:'SimHei',
          list:[],
          name:'',
          icon:'starSharp',
          radius:30
      }
      if (cookieState){
          stateObj = JSON.parse(cookieState)
      }
      const state = reactive(stateObj)
      const nowTab=ref('poster')
      const imgUrl=ref('')
      const resImg=ref('')
      const colors = [
          '#c1cbd7','#afb0b2','#969391','#bfbfbf','#8696a7','#9ca8b8','#fffaf4','#e0e5df','#b5c4b1','#ecacea','#7b8b6f','#e0cdcf','#b7b1a5','#965454','#faead3'
      ]
      const sizes = [ 40, 38, 36, 34, 32, 30, 28, 26, 24, 22, 20, 18, 16]
      const fonts = [
          {label:'楷体',value:'KaiTi'},
          {label:'黑体',value:'SimHei'},
          {label:'宋体',value:'SimSun'},
          {label:'仿宋',value:'FangSong'}
      ]
      const icons = [
          {label:'星星',value:'starSharp'},
          {label:'勾',value:'checkmarkCircle'},
          {label:'闪电',value:'flashSharp'},
          {label:'赞',value:'thumbsUpSharp'}
      ]
      const saveoption = ()=>{
          setOptions(JSON.stringify(state))
      }
      const iconList = {
          starSharp,
          checkmarkCircle,
          flashSharp,
          thumbsUpSharp
      }
      const chooseImg = (file: any)=>{
        const reader = new FileReader();
        reader.readAsDataURL(file.target.files[0]);
        reader.onload = function (e: any) {
            imgUrl.value = e.target.result;
        };
        /*
        const URL = window.URL || window.webkitURL
        if (URL) {
           imgUrl.value=URL.createObjectURL(file.target.files[0])
        }*/
      }
      const generate = ()=>{
          const cloneDom = (document.getElementById('board') as any).cloneNode(true)
        cloneDom.style.position = 'absolute'
            cloneDom.style.top = '0px'
            cloneDom.style.zIndex = '-1'
            cloneDom.style.width = '400px'
      document.body.appendChild(cloneDom)
        html2canvas(cloneDom,{
            useCORS:true,
            proxy:'http://127.0.0.1/'
        }).then(canvas => {
            resImg.value =canvas.toDataURL("image/png");
            document.body.removeChild(cloneDom)
        });
        cloneDom.style.display = 'none'
      }
      const removeUrl = (url: string)=>{
        const URL = window.URL || window.webkitURL
        if (URL) {
            URL.revokeObjectURL(url)
        }
      }
      return {
          chooseImg,
          colors,
          sizes,
          fonts,
          icons,
          generate,
          resImg,
          removeUrl,
          iconList,
          nowTab,
          imgUrl,
          saveoption,
          ...toRefs(state)
      }
  }
})
</script>