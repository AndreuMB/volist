<template>
  <ion-page>
    <ion-content :fullscreen="true">
      <div class="row p-4 text-center center">
        <div class="col-12"> <h1 class="display-1">INICIO</h1> </div>
        <div class="col-12"><v-calendar :attributes='attributes' is-expanded/></div>
        <div class="col-12 p-5 border rounded">
          LIST JOINED CLASSES
        </div>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { IonPage, IonContent } from '@ionic/vue';

export default defineComponent({
  name: 'Tab2Page',
  components: { IonPage, IonContent },
  mounted(){
    this.primaryColor="orange";
  },
  setup() {
    return {
      date:"20/10/2000",
      primaryColor:"orange",
      secondaryColor:"grey",
      attributes:[
      {
        // An optional key can be used for retrieving this attribute later,
        // and will most likely be derived from your data object
        key: "today",
        // Attribute type definitions
        highlight: false,  // Boolean, String, Object
        dot: {
          color: 'orange',
          fillMode: 'solid',
        }, // Boolean, String, Object
        bar: false, // Boolean, String, Object
        content: false, // Boolean, String, Object
        // popover: { ... }, // Only objects allowed
        // Your custom data object for later access, if needed
        // customData: { ... },
        // We also need some dates to know where to display the attribute
        // We use a single date here, but it could also be an array of dates,
        //  a date range or a complex date pattern.
        dates: new Date(),
        // You can optionally provide dates to exclude
        excludeDates: null,
        // Think of `order` like `z-index`
        order: 0
      },
      {
        // An optional key can be used for retrieving this attribute later,
        // and will most likely be derived from your data object
        key: "class",
        // Attribute type definitions
        highlight: "green",  // Boolean, String, Object
        dot: false,        // Boolean, String, Object
        bar: false,        // Boolean, String, Object
        content: false,   // Boolean, String, Object
        dates: [new Date(2022, 8, 19),new Date(2022, 8, 21),new Date(2022, 8, 23)],
        // Think of `order` like `z-index`
        order: 1
      }
    ],
    }
  }
});
</script>
<style>
  .center{
    height: 100%;
    align-content: center;
    display: flex;
    flex-wrap: wrap;
  }
  .center > div {
    margin-top: 2em;
    margin-bottom: 2em;
  }
</style>
