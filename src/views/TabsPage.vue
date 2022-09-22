<template>
  <ion-page>
    <ion-tabs>
      <ion-router-outlet></ion-router-outlet>
      <ion-tab-bar>
        <slot name="button"></slot>
        <ion-tab-button tab="tab1" href="/tabs/tab1">
          <font-awesome-icon icon="fa-solid fa-lock" size="2x" />
        </ion-tab-button>
          
        <ion-tab-button tab="tab2" href="/tabs/tab2">
          <font-awesome-icon icon="fa-solid fa-volleyball" size="3x" />
        </ion-tab-button>
        
        <ion-tab-button tab="tab3" href="/tabs/tab3">
          <font-awesome-icon icon="fa-solid fa-user" size="2x" />
          <!-- <ion-label>Tab 3</ion-label> -->
        </ion-tab-button>
      </ion-tab-bar>
    </ion-tabs>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { IonTabBar, IonTabButton, IonTabs, IonLabel, IonPage, IonRouterOutlet } from '@ionic/vue';

export default defineComponent({
  name: 'TabsPage',
  components: { IonTabs, IonTabBar, IonTabButton, IonPage, IonRouterOutlet }, // IonLabel
  setup() {
    return {
    }
  }
});
</script>
