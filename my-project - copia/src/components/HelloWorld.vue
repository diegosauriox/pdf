<template>
    <div class="pdf" style="height:100%">
     <button class="btn" @click="generar">Download PDF</button>
     <input  @v-model="archivo" type="file" @change="img64" > 
     
    </div>
</template>

<script>

import jsPDF from 'jspdf'
import imageToBase64 from 'image-to-base64/browser';
export default {
  name: 'pdf',
  
  data () {
    return {
     archivo:[],
     logo:"",
     pdf: new jsPDF(),
    }
  },
  methods:{
    
    generar() {
      var pdf= this.pdf;
      this.newImage();
      //aca instale una mini libreria que se llamaba imagetobase64
    /*   imageToBase64("../assets/logo.png")
      .then((response)=>{
         this.logo="data:image/png;base64,"+response;
        
         console.log(this.logo);
         console.log(this.archivo[0]);
       }
       ).catch((error)=>{
         console.log(error);
       })
    
        
       pdf.addImage(this.logo,'JPEG',40,40,50,50); */
      
      var titulo="Reporte Especial de Actividad Volcánica (REAV)"
      var subtitulo="Región Del Maule, Complejo Volcánico Laguna del Maule"
      var fecha= "15 de abril de 2021, 06:25 Hora local (Chile continental)"
      
      var parrafo1="El Servicio Nacional de Geología y Minería de Chile (Sernageomin) da a conocer la siguiente información "+
      "PRELIMINAR, obtenida a través de los equipos de monitoreo de la Red Nacional de Vigilancia Volcánica (RNVV), "+
      "procesados y analizados en el Observatorio Volcanológico de los Andes del Sur (Ovdas):";
      
      var parrafo2="Hoy jueves 15 de abril a las 05:09 hora local (09:09 UTC), las estaciones de monitoreo instaladas en las \n"+
      "inmediaciones del Complejo Volcánico Laguna del Maule registraron un sismo asociado tanto al fracturamiento \n"+
      "de roca como a la dinámica de fluidos al interior del sistema volcánico (Híbrido)."


      
      
      

      //pdf.setFontType("bold");
      pdf.setFontSize(13);
      this.centrarTexto(titulo,40);

      //pdf.setFontType("normal");
      pdf.setFontSize(12);
      this.centrarTexto(subtitulo,45);
      this.centrarTexto(fecha,50);

      pdf.setFontSize(10);
      //pdf.text(10, 60, parrafo1);
      pdf.text(10, 80, parrafo2);
      pdf.text(parrafo1, 10,110, {maxWidth: 150, align: "justify"});
      //pdf.addImage(this.archivo[0], 'JPEG', 15, 100, 50, 50);
      
      pdf.save('my_pdf.pdf');  
      



    },centrarTexto(texto,y){
      var textWidth = this.pdf.getStringUnitWidth(texto) * this.pdf.internal.getFontSize() / this.pdf.internal.scaleFactor;
      var textOffset = (this.pdf.internal.pageSize.width - textWidth) / 2;
      this.pdf.text(textOffset, y, texto);
    
    },img64 (e) {
      var fileReader = new FileReader();
        fileReader.readAsDataURL(e.target.files[0]);
        fileReader.onload = e => {
        
          this.archivo.push(e.target.result);
          
    };


    //intento loco pero el .onload no pesca!!!!!
  },newImage(){
    var img = new Image();
      img.crossOrigin = 'Anonymous';
    
    // The magic begins after the image is successfully loaded
    img.onload = function () {
      
      var canvas = document.createElement('canvas'),
        ctx = canvas.getContext('2d');

      canvas.height = img.naturalHeight;
      canvas.width = img.naturalWidth;
      ctx.drawImage(img, 0, 0);

      // Unfortunately, we cannot keep the original image type, so all images will be converted to PNG
      // For this reason, we cannot get the original Base64 string
      var uri = canvas.toDataURL('../assets/logo.png'),
        b64 = uri.replace(/^data:image.+;base64,/, '');

      console.log(b64); //-> "iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWP4z8DwHwAFAAH/q842iQAAAABJRU5ErkJggg=="
    };
   
  }
  }
}
</script>
