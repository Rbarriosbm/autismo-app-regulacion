<template>
  <v-container fluid class="mt-2">
    <v-card elevation="0" class="mb-5">
      <v-card-title
        style="font-size: 30px"
        class="font-weight-bold justify-center"
      >
        Herramientas
      </v-card-title>
      <v-card-text class="text-center" style="font-weight: 800">
        Encuentra ayuda e información sobre temáticas de tu interés haciendo click en cualquiera de las tarjetas. 
      </v-card-text>
    </v-card>


    <v-row class="px-5">
      <v-col
        :cols="$vuetify.breakpoint.smAndDown ? 12 : 6"
        v-for="(item, i) in cardItems"
        :key="i"
      >
        <v-dialog fullscreen hide-overlay transition="dialog-bottom-transition">
          <template v-slot:activator="{ on, off }">
            <v-card
              v-bind="off"
              v-on="on"
              :color="item.color"
              height="90"
              class="rounded-lg d-flex justify-space-between"
              elevation="4"
            >
              <v-card-title
                class="white--text font-weight-bold"
                style="word-break: break-word"
                >{{ item.title }}</v-card-title
              >
              <v-icon color="white" size="30" class="px-5">{{
                item.icon
              }}</v-icon>
            </v-card>
          </template>

          <template v-slot:default="dialog">
            <v-card>
              <v-toolbar dark :color="item.color">
                <v-icon>
                  {{ item.icon }}
                </v-icon>
                <v-spacer></v-spacer>
                <v-icon @click="dialog.value = false"> mdi-close </v-icon>
              </v-toolbar>

              <v-card-title
                class="justify-center text-center"
                style="
                  font-weight: 800;
                  word-break: break-word;
                  font-size: 25px
                "
              >
                {{ item.title }}
              </v-card-title>

              <div class="px-5 mt-4" v-for="(subItem, i) in item.qans" :key="i">
                <v-card
                  class="rounded-xl px-2 py-2"
                  :ripple="false"
                  elevation="2"
                  height="auto"
                  @click="subItem.expand = !subItem.expand"
                >
                  <v-icon :color="item.color" class="px-2">
                    {{ !subItem.expand ? "mdi-plus" : "mdi-minus" }}
                  </v-icon>
                  <span>{{ subItem.qTitle }}</span>
                </v-card>
                <v-expand-transition>
                  <v-sheet v-show="subItem.expand" height="auto" class="pa-2">
                    <div class="d-flex flex-column">
                      <p v-html="subItem.content"></p>
                    </div>
                  </v-sheet>
                </v-expand-transition>
              </div>
            </v-card>
          </template>
        </v-dialog>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { onValue , ref } from "firebase/database";
import firebase from "@/firebase/index";

export default {
  

  mounted(){
      onValue(ref(firebase.db, "tools"), (snapshot) => {
        const data = snapshot.val(); 
        this.cardItems = data
      });
  },

  methods:{
  },

  data() {
    return {
      cardItems: [],
      // {
      //     title: "No se como me siento",
      //     icon: "mdi-lightbulb-question-outline",
      //     color: "purple",
      //     qans:[
      //         {
      //           expand: false,
      //           qTitle: 'No me siento con energía',
      //           content: 'Causas del por qué no te sientes con <b>energía</b>',
               
      //       },
      //       {
      //           expand: false,
      //           qTitle: 'Me encuentro inseguro de mi mismo',
      //           cause: 'Causas del por qué no te sientes seguro de ti mismo',
      //           solution: 'Lorem ipsum dolor sit, amet consectetur adipisicing elit. Fugiat illum cum pariatur? Aperiam nam perferendis, ullam mollitia debitis ut hic, quis quaerat dolorem assumenda tempora id, corrupti cum fuga corporis!'
      //       },
      //     ]
      //   },
      //   {
      //     title: "Depresión",
      //     icon: "mdi-weather-lightning-rainy",
      //     color: "red",
      //     qans:[
      //         {
      //           expand: false,
      //           qTitle: 'No me siento con energía',
      //           cause: 'Causas del por qué no te sientes con energía',
      //           solution: 'Lorem ipsum dolor sit, amet consectetur adipisicing elit. Fugiat illum cum pariatur? Aperiam nam perferendis, ullam mollitia debitis ut hic, quis quaerat dolorem assumenda tempora id, corrupti cum fuga corporis!'
      //       },
      //       {
      //           expand: false,
      //           qTitle: 'Me encuentro inseguro de mi mismo',
      //           cause: 'Causas del por qué no te sientes seguro de ti mismo',
      //           solution: 'Lorem ipsum dolor sit, amet consectetur adipisicing elit. Fugiat illum cum pariatur? Aperiam nam perferendis, ullam mollitia debitis ut hic, quis quaerat dolorem assumenda tempora id, corrupti cum fuga corporis!'
      //       },
      //     ]
      //   },
      //   {
      //     title: "Estrés",
      //     icon: "mdi-lightning-bolt-outline",
      //     color: "orange",
      //   },
      //   {
      //     title: "Ansiedad",
      //     icon: "mdi-sine-wave",
      //     color: "blue",
      //   },
      //   {
      //     title: "Problemas de comunicación",
      //     icon: "mdi-lightbulb-group-outline",
      //     color: "green",
      //   },

    };
  },
};
</script>
<style scoped>
.v-card--link:before {
  background: none;
}
</style>