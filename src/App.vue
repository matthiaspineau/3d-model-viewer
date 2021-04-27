<template>
  <div id="app" class="container">

    <div class="formulaire__model-viewer">
      <h2>View 3d model</h2>


      <div class="d-flex justify-space">
        <div class="">
          <h4>etape : 1 (upload your model)</h4>
          <div class="wrap__model wrapper border">

            <!-- Model loader -->

            <!-- Model STL -->
            <model-stl v-if="modelStl"
            ref="modelstl"
            :width="300"
            :height="300" 
            :backgroundAlpha="0" 
            :glOptions="{ preserveDrawingBuffer: true }"
            :src="stlBase64"></model-stl>

            <!-- Model OBJ -->
            <model-obj v-if="modelObj"
            ref="modelobj"
            :width="300"
            :height="300" 
            :backgroundAlpha="0" 
            :glOptions="{ preserveDrawingBuffer: true }"
            :src="objBase64"></model-obj>

          </div>
          <!-- Form load file -->
          <form action="" enctype="multipart/form-data" class="form">
            <input type="file" id="file" name="file" ref="fichier" @change="previewFile">
          </form>
        </div>

        <div class="">
          <h4>etape : 2 (take screenshot)</h4>
          <button id="btn" @click="screenshot">screenshot</button>
          <div class="wrap__300 border">
            <!-- Image -->
            <div class="wrap__img">
              <img class="screenshot" v-if="screenshotFile" :src="screenshotFile" />
            </div>
          </div>
        </div>

        <div class="">
          <h4>etape : 3 (download screenshot) </h4>
          <div class="wrap__300 d-block border">
            <div class="">
              <label for="">nom : </label>
              <input type="text" v-model="fileName">
            </div>
            <button @click="saveScreenshot">download screenshot</button>
            <a id="download" href=""></a>
          </div>
        </div>
      </div>
    </div> 

  </div>


</template>

<script>
import ModelStl from '@/components/ModelStl.vue';
import ModelObj from '@/components/ModelObj.vue';

export default {
  data() {
      return {
        base64: null,
        screenshotFile: null,
        fileName: 'mon-fichier.png',
        modelStl: false,
        modelObj: false,
        stlBase64: null,
        objBase64: null
      };
    },
  methods: {
    previewFile() {
      let str = this.$refs.fichier.files[0].name
     
      str = str.substring((str.length - 3))


      if (str != 'stl' &&  str != 'obj') {
        console.log('erreur de fichier')
        return 
      }

      let reader = new FileReader()

      if (str == 'stl') {
        // console.log('fichier stl')
        this.modelStl = true;
        this.modelObj = false;
        this.objBase64 = null;

        reader.onload = (e) => {      
          this.stlBase64 = e.target.result;
        }

      } else  {
        // console.log('fichier obj')
        this.modelStl = false;
        this.modelObj = true;
        this.stlBase64 = null;

        reader.onload = (e) => {      
          this.objBase64 = e.target.result;
        }
      } 
      

      reader.readAsDataURL(this.$refs.fichier.files[0]);

    },
    screenshot() {
       let link = document.getElementById('download')
       

      if (this.modelStl) {
        this.screenshotFile = this.$refs.modelstl.renderer.domElement.toDataURL('image/png', 1);
        link.href = this.$refs.modelstl.renderer.domElement.toDataURL();
      }

      if (this.modelObj) {
        this.screenshotFile = this.$refs.modelobj.renderer.domElement.toDataURL('image/png', 1);
        link.href = this.$refs.modelobj.renderer.domElement.toDataURL();
      }

      link.download = this.fileName
      
    },
    saveScreenshot() {
     
      document.getElementById('download').click()
    },
  },
  name: 'App',
  components: {
    ModelStl,
    ModelObj
  }
}
</script>

<style src="@/assets/scss/app.scss" lang="scss"></style>
