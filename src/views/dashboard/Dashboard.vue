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
          color="#E91E63"
          hover-reveal
          type="Bar"
        >
        </base-material-chart-card>

        <base-material-chart-card
          :data="infectedRegions2.data"
          :options="infectedRegions2.options"
          :responsive-options="infectedRegions2.responsiveOptions"
          color="#E91E63"
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
              Employees Stats
            </div>

            <div class="subtitle-1 font-weight-light">
              New employees on 15th September, 2016
            </div>
          </template>
          <v-card-text>
            <v-data-table
              :headers="headers"
              :items="items"
            />
          </v-card-text>
        </base-material-card>
      </v-col>

      <v-col
        cols="12"
        md="6"
      >
        <base-material-card class="px-5 py-3">
          <template v-slot:heading>
            <v-tabs
              v-model="tabs"
              background-color="transparent"
              slider-color="white"
            >
              <span
                class="subheading font-weight-light mx-3"
                style="align-self: center"
              >Tasks:</span>
              <v-tab class="mr-3">
                <v-icon class="mr-2">
                  mdi-bug
                </v-icon>
                Bugs
              </v-tab>
              <v-tab class="mr-3">
                <v-icon class="mr-2">
                  mdi-code-tags
                </v-icon>
                Website
              </v-tab>
              <v-tab>
                <v-icon class="mr-2">
                  mdi-cloud
                </v-icon>
                Server
              </v-tab>
            </v-tabs>
          </template>

          <v-tabs-items
            v-model="tabs"
            class="transparent"
          >
            <v-tab-item
              v-for="n in 3"
              :key="n"
            >
              <v-card-text>
                <template v-for="(task, i) in tasks[tabs]">
                  <v-row
                    :key="i"
                    align="center"
                  >
                    <v-col cols="1">
                      <v-list-item-action>
                        <v-checkbox
                          v-model="task.value"
                          color="secondary"
                        />
                      </v-list-item-action>
                    </v-col>

                    <v-col cols="9">
                      <div
                        class="font-weight-light"
                        v-text="task.text"
                      />
                    </v-col>

                    <v-col
                      cols="2"
                      class="text-right"
                    >
                      <v-icon class="mx-1">
                        mdi-pencil
                      </v-icon>
                      <v-icon
                        color="error"
                        class="mx-1"
                      >
                        mdi-close
                      </v-icon>
                    </v-col>
                  </v-row>
                </template>
              </v-card-text>
            </v-tab-item>
          </v-tabs-items>
        </base-material-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  import dataJson from '@/JSON/data.json'
  export default {
    name: 'DashboardDashboard',

    data () {
      return {
        json: dataJson,
        dataCompletedTasksChart: {
          data: {
            labels: ['12am', '3pm', '6pm', '9pm', '12pm', '3am', '6am', '9am'],
            series: [
              [230, 750, 450, 300, 280, 240, 200, 190],
            ],
          },
          options: {
            lineSmooth: this.$chartist.Interpolation.cardinal({
              tension: 0,
            }),
            low: 0,
            high: 10000, // creative tim: we recommend you to set the high sa the biggest value + something for a better look
            chartPadding: {
              top: 0,
              right: 0,
              bottom: 0,
              left: 0,
            },
          },
        },
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
            text: 'Name',
            value: 'name',
          },
          {
            sortable: false,
            text: 'Salary',
            value: 'salary',
            align: 'right',
          },
          {
            sortable: false,
            text: 'Country',
            value: 'country',
            align: 'right',
          },
          {
            sortable: false,
            text: 'City',
            value: 'city',
            align: 'right',
          },
        ],
        items: [
          {
            id: 1,
            name: 'Dakota Rice',
            country: 'Niger',
            city: 'Oud-Tunrhout',
            salary: '$35,738',
          },
          {
            id: 2,
            name: 'Minerva Hooper',
            country: 'Curaçao',
            city: 'Sinaai-Waas',
            salary: '$23,738',
          },
          {
            id: 3,
            name: 'Sage Rodriguez',
            country: 'Netherlands',
            city: 'Overland Park',
            salary: '$56,142',
          },
          {
            id: 4,
            name: 'Philip Chanley',
            country: 'Korea, South',
            city: 'Gloucester',
            salary: '$38,735',
          },
          {
            id: 5,
            name: 'Doris Greene',
            country: 'Malawi',
            city: 'Feldkirchen in Kārnten',
            salary: '$63,542',
          },
        ],
        tabs: 0,
        tasks: {
          0: [
            {
              text: 'Sign contract for "What are conference organizers afraid of?"',
              value: true,
            },
            {
              text: 'Lines From Great Russian Literature? Or E-mails From My Boss?',
              value: false,
            },
            {
              text: 'Flooded: One year later, assessing what was lost and what was found when a ravaging rain swept through metro Detroit',
              value: false,
            },
            {
              text: 'Create 4 Invisible User Experiences you Never Knew About',
              value: true,
            },
          ],
          1: [
            {
              text: 'Flooded: One year later, assessing what was lost and what was found when a ravaging rain swept through metro Detroit',
              value: true,
            },
            {
              text: 'Sign contract for "What are conference organizers afraid of?"',
              value: false,
            },
          ],
          2: [
            {
              text: 'Lines From Great Russian Literature? Or E-mails From My Boss?',
              value: false,
            },
            {
              text: 'Flooded: One year later, assessing what was lost and what was found when a ravaging rain swept through metro Detroit',
              value: true,
            },
            {
              text: 'Sign contract for "What are conference organizers afraid of?"',
              value: true,
            },
          ],
        },
        list: {
          0: false,
          1: false,
          2: false,
        },
      }
    },

    methods: {
      complete (index) {
        this.list[index] = !this.list[index]
      },
    },
    /* eslint-disable */

    mounted () {
      let i=0;
      let max=0;
      let max2=0;

         Object.values(this.json).forEach(element => {
         for(i=0;i<12;i++){
                  //console.log(element[i].region);
                  this.infectedRegions.data.labels.push(element[i].region);
                  if(parseInt(element[i].sick)>max)
                  {
                    max = (parseInt(element[i].sick)/110000)*100;
                    //console.log("MAXXXXX",max);
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
                    //console.log("MAXXXXX",max);
                  }
                  this.infectedRegions2.data.series[0].push((parseInt(element[i].sick)/110000)*100);
                  this.infectedRegions2.data.series[1].push((parseInt(element[i].recovered)/110000)*100);
                  this.infectedRegions2.data.series[2].push((parseInt(element[i].confirmed)/110000)*100);
          }
                  console.log(this.infectedRegions2.options.high);
                   this.infectedRegions2.options.high=max2+20;


     });
    
    },
        /* eslint-disable */

  }
</script>
