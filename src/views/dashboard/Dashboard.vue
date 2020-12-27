<template>
  <v-container
    id="dashboard"
    fluid
    tag="section"
  >
    <v-row>
      <v-col
        cols="12"
      >
        <base-material-chart-card
          :data="infectedRegions.data"
          :options="infectedRegions.options"
          :responsive-options="infectedRegions.responsiveOptions"
          color="#553D67"
          hover-reveal
          type="Bar"
        >
        <template v-slot:reveal-actions>
            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn
                  v-bind="attrs"
                  color="info"
                  icon
                  v-on="on"
                >
                  <v-icon
                    color="info"
                  >
                    mdi-refresh
                  </v-icon>
                </v-btn>
              </template>

              <span>Refresh</span>
            </v-tooltip>

            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn
                  v-bind="attrs"
                  light
                  icon
                  v-on="on"
                >
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
              </template>

              <span>Change Date</span>
            </v-tooltip>
          </template>

          <template v-slot:actions>
            <v-icon
              class="mr-1"
              small
            >
              mdi-clock-outline
            </v-icon>
            <span class="caption grey--text font-weight-light">updated 10 minutes ago</span>
          </template>
        </base-material-chart-card>

        <base-material-chart-card
          :data="infectedRegions2.data"
          :options="infectedRegions2.options"
          :responsive-options="infectedRegions2.responsiveOptions"
          color="#553D67"
          hover-reveal
          type="Bar"
        >
        <template v-slot:reveal-actions>
            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn
                  v-bind="attrs"
                  color="info"
                  icon
                  v-on="on"
                >
                  <v-icon
                    color="info"
                  >
                    mdi-refresh
                  </v-icon>
                </v-btn>
              </template>

              <span>Refresh</span>
            </v-tooltip>

            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn
                  v-bind="attrs"
                  light
                  icon
                  v-on="on"
                >
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
              </template>

              <span>Change Date</span>
            </v-tooltip>
          </template>

          <h4 class="card-title font-weight-light mt-2 ml-2">
              Latest stats of covid in Tunisia in % <v-btn
              small
              color="white"
            >
              sick
            </v-btn>
            <v-btn
              small
              color="orange"
            >
              recovered
            </v-btn>
            <v-btn
              small
              color="yellow"
            >
              confirmed
            </v-btn>
          </h4>

          <template v-slot:actions>
            <v-icon
              class="mr-1"
              small
            >
              mdi-clock-outline
            </v-icon>
            <span class="caption grey--text font-weight-light">updated 10 minutes ago</span>
          </template>
        </base-material-chart-card>
      </v-col>

      <v-col
        cols="12"
      >
        <base-material-card
          color="warning"
          class="px-5 py-3"
        >
          <template v-slot:heading>
            <div class="display-2 font-weight-light">
              Users dashboard
            </div>

            <div class="subtitle-1 font-weight-light">
              Updated since 30s
            </div>
          </template>
          <v-simple-table dark>
    <template v-slot:default>
      <thead>
        <tr>
          <th
          class="text-left"
         v-for="item in headers"
          :key="item.text"
          >
            {{item.text}}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="item in items"
          :key="item"
        >
          <td
          v-for="atr in item"
          :key="atr"
          >{{ atr }}</td>
          <td>
          <div class="text-center">
          <v-dialog
          v-model="dialog"
          width="500"
          >
          <template v-slot:activator="{ on, attrs }">
          <v-btn
          color="red lighten-2"
          dark
          v-bind="attrs"
          v-on="on"
          >
          Notify User
          </v-btn>
          </template>

          <v-card>
          <v-card-title class="headline grey lighten-2">
          Privacy Policy
          </v-card-title>

        <v-card-text>
             <v-textarea
                name="input-7-1"
                filled
                v-model="msgTmp"
                label="Message content"
                auto-grow
                required
                value="Attention! Prenez soins de vous le nombre totale des nouveaux cas est 100!!"
              ></v-textarea>
              <v-btn
            depressed
            color="primary"
            v-on:click="notifyUser"
            >
            Send
          </v-btn>
          <br>
          <br>
          <div v-if="success">
          <v-alert
            border="left"
            color="indigo"
            dark
          >
           {{ success }}
          </v-alert>
          </div>
          </v-card-text>

          <v-divider></v-divider>

          <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            text
            @click="dialog = false"
          >
            I accept
          </v-btn>
          </v-card-actions>
          </v-card>
          </v-dialog>
          </div>
          </td>

        </tr>
      </tbody>
    </template>
  </v-simple-table>
            <v-card-text>
             <v-textarea
                name="input-7-1"
                filled
                v-model="msg"
                label="Message content"
                auto-grow
                required
                value="Attention! Prenez soins de vous le nombre totale des nouveaux cas est 100!!"
              ></v-textarea>
              <v-btn
            depressed
            color="primary"
            v-on:click="notifyAll"
            >
            Notify All users
          </v-btn>
          <br>
          <br>
          <div v-if="success">
          <v-alert
            border="left"
            color="indigo"
            dark
          >
           {{ success }}
          </v-alert>
          </div>
          </v-card-text>
        </base-material-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
    /* eslint-disable */

  import Axios from "axios";
  import dataJson from '@/JSON/data.json'
  export default {
    name: 'DashboardDashboard',

    data () {
      return {
        msg:"",
        msgTmp:[],
        json: dataJson,
        infectedRegions: {
          data: {
            labels: [],
            series: [
              [],
              [],
              [],
              [],
            ],
          },
          options: {
            axisX: {
              showGrid: true,
            },
            low: 0,
            high: 100,
            chartPadding: {
              top: 0,
              right: 5,
              bottom: 0,
              left: 0,
            },
          },
          responsiveOptions: [
            ['screen and (max-width: 640px)', {
              seriesBarDistance: 5,
              axisX: {
                labelInterpolationFnc: function (value) {
                  return value[0]
                },
              },
            }],
          ],
        },
        infectedRegions2: {
          data: {
            labels: [],
            series: [
              [],
              [],
              [],
              [],
            ],
          },
          options: {
            axisX: {
              showGrid: true,
            },
            low: 0,
            high: 100,
            chartPadding: {
              top: 0,
              right: 5,
              bottom: 0,
              left: 0,
            },
          },
          responsiveOptions: [
            ['screen and (max-width: 640px)', {
              seriesBarDistance: 5,
              axisX: {
                labelInterpolationFnc: function (value) {
                  return value[0]
                },
              },
            }],
          ],
        },
        headers: [
          {
            sortable: false,
            text: 'ID',
            value: 'id',
          },
          {
            sortable: false,
            text: 'Temperature',
            value: 'temperature',
          },
          {
            sortable: false,
            text: 'Cough',
            value: 'CoughState',
            align: 'right',
          },
          {
            sortable: false,
            text: 'Tired',
            value: 'tiredState',
            align: 'right',
          },
          {
            sortable: false,
            text: 'Smell/Taste',
            value: 'smellortaste',
            align: 'right',
          },
          {
            sortable: false,
            text: 'Breathing diffuculty',
            value: 'breathdifficulties',
            align: 'right',
          },
          {
            sortable: false,
            text: 'Pressure/Pain',
            value: 'cheetpainorpressure',
            align: 'right',
          },
          {
            sortable: false,
            text: 'Loss Speech/Mouvement',
            value: 'lossspeechormovement',
            align: 'right',
          },
        ],
        items: [],
        success:null,
      }
    },

    methods: {
      complete (index) {
        this.list[index] = !this.list[index]
      },
      getProfile() {
        let i =1;
      Axios.get("/userStates")
        .then(Response => {
          //console.log("RESPONSE RECEIVED : ", Response.data);
          Object.values(Response.data).forEach(element => {
            console.log(element);
            element._id=i;
            i++;
          this.items.push(element);
          });
          })
        .catch(errors => {
          Object.values(errors.response.data.errors).forEach(element => {
            console.log(element);
          });
        });
    },
    notifyAll() {
      let data ={
        msg:"",
        phone:[]
      };
      data.msg=this.msg;
      data.phone=["94777621"];
      console.log(data);
      Axios.post("/send-all",data)
        .then(Response => {
          console.log("RESPONSE RECEIVED : ", Response.data);
          this.success= Response.data.message;
          })
        .catch(errors => {
          Object.values(errors.response.data.errors).forEach(element => {
            console.log(element);
          });
        });
    },
    notifyUser() {
      let data ={
        msg:"",
        phone:[]
      };
      data.msg=this.msgTmp;
      data.phone=["94777621"];
      console.log(data);
      Axios.post("/send-all",data)
        .then(Response => {
          console.log("RESPONSE RECEIVED : ", Response.data);
          this.success= Response.data.message;
          })
        .catch(errors => {
          Object.values(errors.response.data.errors).forEach(element => {
            console.log(element);
          });
        });
        this.msgTmp ="";
    },
    },

    mounted () {
      let i=0;
      let max=0;
      let max2=0;

         Object.values(this.json).forEach(element => {
         for(i=0;i<12;i++){
                  this.infectedRegions.data.labels.push(element[i].region);
                  if(parseInt(element[i].sick)>max)
                  {
                    max = (parseInt(element[i].sick)/110000)*100;
                  }
                  this.infectedRegions.data.series[0].push((parseInt(element[i].sick)/110000)*100);
                  this.infectedRegions.data.series[1].push((parseInt(element[i].recovered)/110000)*100);
                  this.infectedRegions.data.series[2].push((parseInt(element[i].confirmed)/110000)*100);
          }
                  console.log(this.infectedRegions.options.high);
                   this.infectedRegions.options.high=max+20;
                   
         for(i=12;i<element.length;i++){
                  console.log(element[i].region);
                  this.infectedRegions2.data.labels.push(element[i].region);
                  if(parseInt(element[i].sick)>max)
                  {
                    max2 = (parseInt(element[i].sick)/110000)*100;
                  }
                  this.infectedRegions2.data.series[0].push((parseInt(element[i].sick)/110000)*100);
                  this.infectedRegions2.data.series[1].push((parseInt(element[i].recovered)/110000)*100);
                  this.infectedRegions2.data.series[2].push((parseInt(element[i].confirmed)/110000)*100);
          }
                  console.log(this.infectedRegions2.options.high);
                   this.infectedRegions2.options.high=max2+20;
     });

          this.getProfile();

    },
    watch :{
      success(val) {
      if (val) {
        setTimeout(() => (this.success = null), 2000);
        console.log(val);
      }
    }
    }
        /* eslint-disable */

  }
</script>
