<template>
  <div class="hello">
    <div
    id="drop_zone"
    @drop="dropHandler"
    @dragenter.prevent="entered"
    @dragover.prevent="dragOverHandler">
        <p>Drag one or more files to this <i>drop zone</i>.</p>
    </div>

    <div class="info_zone" v-if="this.info != null">
      <h3>Here is what we found</h3>
      <p> Episode: {{this.info.result[0].episode}}</p>
      <p> Filename: {{this.info.result[0].filename}}</p>
      <p> Similarity: {{this.info.result[0].similarity}}</p>
      <p> See more</p>
      
      <video width="320" height="240" controls>
        <source :src="this.info.result[0].video" type="video/mp4">
        <source src="movie.ogg" type="video/ogg">
        Your browser does not support the video tag.
    </video> 

    </div>
  </div>
</template>

<script>
//import axios from "axios";

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return{
      info : null,
      info2 : null,
    }
  },
  methods:{

    
    async dropHandler(ev) {
    console.log('File(s) dropped');
  
    // Prevent default behavior (Prevent file from being opened)
    ev.preventDefault();
  
    if (ev.dataTransfer.items) {
      // Use DataTransferItemList interface to access the file(s)
      // -- If it is a File show it // If it is a Folder, go through and show files
      [...ev.dataTransfer.items].forEach((item, i) => {
        // If dropped items aren't files, reject them
        if (item.kind === 'file') {
          const file = item.getAsFile();
          console.log(`… file[${i}].name = ${file.name}`);


          const formData = new FormData();
          formData.append("image", file);

          var result = fetch("https://api.trace.moe/search", {
            method: "POST",
            body: formData,
          }).then( e => {
            //console.log(e.json());
            return e.json();
          });

          // var result2 = fetch(
          //     `https://api.trace.moe/search?anilistInfo&url=${encodeURIComponent(
          //       "https://images.plurk.com/32B15UXxymfSMwKGTObY5e.jpg"
          //     )}`
          //   ).then((e) => {return e.json()});


          const printAnime = async() =>{
            this.info = await result;
            //this.info2 = await result2;
            console.log(this.info)
            //console.log(this.info2)
          };
          printAnime() 
        }
      });
    } else {
      // Use DataTransfer interface to access the file(s)
      [...ev.dataTransfer.files].forEach((file, i) => {
        console.log(`… file[${i}].name = ${file.name}`);
      });
    }
  },
  //Handles default browser's drag behaviour (showing the picture in current page)
  dragOverHandler(ev) {
    console.log('File(s) in drop zone');

    // Prevent default behavior (Prevent file from being opened)
    ev.preventDefault();
  },
  entered(){
    console.log("entereeeeeed")
  }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .body{
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
  .hello{
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;

  }
  #drop_zone {
      border: 5px solid blue;
      width: 200px;
      height: 100px;

    }
    
</style>
