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
                <ion-datetime class="col-6" v-model="form.time.start" presentation="time"></ion-datetime>
                <ion-datetime class="col-6" v-model="form.time.end" presentation="time"></ion-datetime>
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
import { format, utcToZonedTime } from 'date-fns-tz';
import moment from 'moment';

export default defineComponent({
  name: 'Tab2Page',
  components: { IonPage, IonContent, IonDatetime, IonButton, IonSelect }, // IonInput

  mounted(){
    console.log("no ralles");
    // console.log("days",this.days);
    // console.log("dates",this.dates);
    // console.log("attributes",this.attributes);
  },

  data() {
    return {
      days: [{id:"",date:{dateString: new Date().toLocaleDateString('en-CA') , color:"orange"}}],
      form:{
        weekDays:[null],
        time:{ start: "", end:"" },
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
      }));
    },
  },

  methods: {
    onDayClick(day:any) {      
      const idx = this.days.findIndex(d => d.id === day.id);
      if (idx >= 0) {
        this.days.splice(idx, 1);
      } else {
        this.days.push({
          id: day.id,
          date: { dateString:day.date.toLocaleDateString('en-CA'), color:"" },
        });
      }
    },

    setDatesCalendar(weekDays:Array<any>):Array<string>{
      let dates:Array<string> = [];

      // go over the selected days
      weekDays.forEach(element => {
        if (element == null) return
        let d = new Date();
        let year = d.getFullYear();

        while (d.getDay() !== element) {
          d.setDate(d.getDate() + 1);
        }

        while (d.getFullYear() === year) {
          var pushDate = new Date(d.getTime());
          dates.push(pushDate.toLocaleDateString('en-CA'));
          d.setDate(d.getDate() + 7);
        }

      });

      dates.forEach((element,index) => {

        this.days.push(
          {
            id:element,
            date:{dateString:element, color:""}
          }
        );
      });

      return dates;
    },

    validateTime(time:string,validationTime=""):boolean{
      // console.log("time",time);
      let d = new Date(time);

      if (validationTime != "") {
        let d2 = new Date(validationTime);
        // valida
        // console.log(d.getHours() + ">=" + new Date(validationTime).getHours());
        // console.log("validationTime",validationTime);
        // if (d.getHours()<=new Date(validationTime).getHours()) {
        //   console.log("error!!");
        //   return false;
        // }
        if (d<=d2) {
          console.log("error!!");
          return false;
        }
        
      }

      // console.log(d.getHours() + ">=" + new Date(validationTime).getHours() );
      // if (validationTime != "" && d.getHours()<=new Date(validationTime).getHours()) {
      //   console.log("error");
      //   return "";
      // }

      // // Get the time zone set on the user's device
      // const userTimeZone = Intl.DateTimeFormat().resolvedOptions().timeZone;

      // // Create a date object from a UTC date string
      // // const date = new Date('2014-10-25T10:46:20Z');

      // // Use date-fns-tz to convert from UTC to a zoned time
      // const zonedTime = utcToZonedTime(d, userTimeZone);

      // return zonedTime.toISOString();

      // Create a formatted string from the zoned time
      // return format(zonedTime, 'yyyy-MM-dd HH:mm:ssXXX', { timeZone: userTimeZone });


      // return d.getHours()+":"+d.getMinutes();
      // var tzoffset = d.getTimezoneOffset() * 60000; //offset in milliseconds
      // var localISOTime = new Date(d.getMilliseconds()).toISOString();
      // return localISOTime;
      // d = time.end ? new Date(time.end): new Date();
      // console.log(d.getHours()+":"+d.getMinutes());
      // console.log("FOCUS",d.toLocaleDateString());

      // var x = (new Date()).getTimezoneOffset() * 60000; 
      // var localISOTime = (new Date(Date.now() - x)).toISOString().slice(0,-1);
      
      return true;
    },

    mountDatesTime(dates:Array<string>,time:{start:string,end:string}){
      let datesArrayBd = [];
      dates.forEach(date => {
        console.log("element date = ",date+"T"+time.start.split("T")[1]);
        console.log("element date = ",date+"T"+time.end.split("T")[1]);
      });
    },

    onSubmitDays(){
      // if empty string datetime with format ISO 2022-09-29T17:43:00+02:00
      if (this.form.time.start == "") this.form.time.start = moment().format();
      if (this.form.time.end == "") this.form.time.end = moment().format();

      // validate
      if (!this.validateTime(this.form.time.start)) return;
      if (!this.validateTime(this.form.time.end, this.form.time.start)) return;

      // set schenduled days in calendar and mount array with dates
      let datesForm = this.setDatesCalendar(this.form.weekDays);

      // mount array with dates and times
      this.mountDatesTime(datesForm,this.form.time);

      console.log("save db time", this.form.time);
      
      // console.log("FOCUS",this.form.time);
      // this.setDatesCalendar(this.form.weekDays);
      // let timeStart = this.setTime(this.form.time.start);
      // console.log("local timeStart",timeStart); // 2022-09-29T17:43:00+02:00 2022-09-29T09:24:47.508Z 2022-09-29T23:38:00+02:00
      // this.form.time.end = timeStart;
      // let timeEnd = this.setTime(this.form.time.end,this.form.time.start)
      // let testDate = new Date().toString();
      // console.log("testDate",testDate);
      // this.form.time.end = testDate;
      // console.log("testDate2",this.form.time.end);
      // let m = moment();
      // let nowDateFormat = m.format();
      
      
      // console.log("this.form.time start",new Date().toLocaleTimeString()); // 2022-09-29T16:11:00+02:00
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
