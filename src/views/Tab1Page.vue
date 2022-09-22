<template>
  <ion-page>
    <ion-content :fullscreen="false">
      <div class="row p-4 text-center center">
        <div class="col-12"> <h1 class="display-1">ADMIN</h1> </div>
        <div class="col-12"><v-calendar :attributes='attributes' is-expanded @dayclick="onDayClick"/></div>
        <div class="col-12">
          <form class="row">
            <div class="input-group mb-3">
              <ion-select class="form-control" placeholder="Selecciona los dias" :multiple="true">
                <ion-select-option v-for="day in daysName" :key="day" :value="day">{{ day }}</ion-select-option>
              </ion-select>
              <ion-input class="form-control" type="number" placeholder="Duración"></ion-input>
              <span class="input-group-text">min</span>
            </div>
              
                
              <!-- </template> -->
              <ion-datetime class="col-12" presentation="time"></ion-datetime>
            <!-- </div> -->
          </form>
        </div>
        
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { IonPage, IonContent, IonDatetime } from '@ionic/vue';

export default defineComponent({
  name: 'Tab2Page',
  components: { IonPage, IonContent, IonDatetime },

  data() {
    return {
      days: [{id:null,date:[null]}],
      date: {},
      daysName: ["Lunes","Martes","Miercoles","Jueves","Viernes","Sabado","Domingo"]
      // daysName: [{ name: 'Lunes' }, { name: 'Martes' }],
    }
  },

  computed: {
    dates():Array<Array<any>>{
      return this.days.map(day => day.date);
    },

    attributes():object {
      return this.dates.map((date: object) => ({
        highlight: "green",
        dates: date,
      }));
    },
  },
  

  // computed: {
  //   dates():String {
  //       // if (this.days) return this.days.map(day => day.date);
  //       console.log(this.msgTest);
  //       return this.msgTest + "test";
        
  //     }
  //   },
  //   attributes() {
  //     return this.dates.map(date => ({
  //       highlight: true,
  //       dates: date,
  //     }));
  //   },
  // },

  methods: {
    onDayClick(day:any) {
      console.log(day);
      
      const idx = this.days.findIndex(d => d.id === day.id);
      if (idx >= 0) {
        this.days.splice(idx, 1);
      } else {
        this.days.push({
          id: day.id,
          date: day.date,
        });
      }
    }
  },

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
  }
</style>
