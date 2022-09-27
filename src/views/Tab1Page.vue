<template>
  <ion-page>
    <ion-content :fullscreen="false">
      <div class="row p-4 text-center center">
        <div class="col-12"> <div class="display-5">ADMIN</div> </div>
        <div class="col-12"><v-calendar :attributes='attributes' is-expanded @dayclick="onDayClick"/></div>
        <div class="col-12">
          <form class="row">
            <div class="input-group mb-3">
              <ion-select class="form-control d-flex" v-model="form.weekDays" placeholder="Selecciona los dias" :multiple="true">
                <ion-select-option v-for="day in daysName" :key="day" :value="day.id">{{ day.name }}</ion-select-option>
              </ion-select>
            </div>
            <div class="input-group mb-3 col">
              <div class="input-group col-12 border rounded no-margin">
                <div class="col-6 lead">EMPIEZA</div>
                <div class="col-6 lead">FINALIZA</div>
                <ion-datetime class="col-6" presentation="time"></ion-datetime>
                <ion-datetime class="col-6" presentation="time"></ion-datetime>
              </div>
            </div>
            <div class="input-group mb-3">
              <ion-button class="col-12" color="primary" @click="onSubmitDays">Submit</ion-button>
            </div>
          </form>
        </div>
        
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { IonPage, IonContent, IonDatetime, IonButton, IonInput, IonSelect } from '@ionic/vue';

export default defineComponent({
  name: 'Tab2Page',
  components: { IonPage, IonContent, IonDatetime, IonButton, IonSelect }, // IonInput

  mounted(){
    console.log("no ralles");
    console.log("days",this.days);
    console.log("dates",this.dates);
    console.log("attributes",this.attributes);
  },

  data() {
    return {
      days: [{id:0,date:{dateString: new Date().toDateString() , color:"orange"}}],
      form:{
        weekDays:[0],
        time:"",
      },
      daysName: [
        { id:1, name: 'Lunes' }, { id:2, name: 'Martes' },
        { id:3, name: 'Miercoles' }, { id:4, name: 'Jueves' },
        { id:5, name: 'Viernes' }, { id:6, name: 'Sabado' },
        { id:0, name: 'Domingo' }
      ],
    }
  },

  computed: {
    dates():Array<any>{      
      return this.days.map(day => day.date);
    },

    attributes():object {
      return this.dates.map((date: any) => ({

          highlight: date.color !== "" ? date.color : "green",
          dates: date.dateString,
        
        // date.color !== "" ? date.color : 
      }));
    },
  },

  methods: {
    onDayClick(day:any) {      
      console.log("day",day);
      
      const idx = this.days.findIndex(d => d.id === day.id);
      if (idx >= 0) {
        this.days.splice(idx, 1);
      } else {
        this.days.push({
          id: day.id,
          date: { dateString:day.date, color:"" },
        });
      }
      console.log(this.attributes);
      
    },

    onSubmitDays(){
      // let d = new Date();
      // let year = d.getFullYear();
      let dates:Array<string> = [];

      // go over the selected days
      this.form.weekDays.forEach(element => {
        let d = new Date();
        let year = d.getFullYear();

        while (d.getDay() !== element) {
          console.log("before" + d.getDay() + " !== " + element);
          d.setDate(d.getDate() + 1);
          console.log("after" + d.getDay() + " !== " + element);
        }

        while (d.getFullYear() === year) {
          var pushDate = new Date(d.getTime());
          dates.push(pushDate.getFullYear() + '-' + (pushDate.getMonth()+1) + '-' + pushDate.getDate());
          d.setDate(d.getDate() + 7);
        }

      });

      dates.forEach((element,index) => {
        this.days.push(
          {
            id:index,
            date:{dateString:element, color:""}
          }
        );
      });

      console.log("this.days",this.days);
      
      
    },
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
  .no-margin>*{
    margin-left: 0px !important;
  }
</style>
