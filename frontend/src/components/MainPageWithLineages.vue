<template>
  <div>
    <v-card width="100%" color="white" style="padding: 50px; min-height: 90.5vh">
        <v-row justify="center" align="center">
          <v-card width="95%" style="padding: 50px; margin-top: 50px; margin-bottom: 50px" :color="background_card_color">
             <v-card-text>
               <v-layout row wrap justify-center style="padding: 30px;">
                 <v-flex class="no-horizontal-padding xs12 d-flex" style="justify-content: center;">
                   <v-card width="95%" :color="menu_color">
                     <v-layout row wrap justify-center style="padding: 30px;">
                       <v-flex class="no-horizontal-padding xs12 d-flex" style="justify-content: center; margin-top: 10px">
                       <h2 style="color: white">DEFINE ANALYSIS:</h2>
                      </v-flex>
                      <v-flex class="no-horizontal-padding xs12 md4 d-flex" style="justify-content: center;">
                        <v-layout row wrap justify-center>
                          <v-flex class="no-horizontal-padding xs12 d-flex" style="justify-content: center; padding-top: 0; padding-bottom: 0">
                            <span style="color: white">Granularity:</span>
                          </v-flex>
                          <v-flex class="no-horizontal-padding xs12 d-flex" style="justify-content: center; padding-top: 0; padding-bottom: 0">
                            <v-select
                              v-model="selectedGeo"
                              :items="possibleGeo"
                              label="Granularity"
                              solo
                              hide-details
                            ></v-select>
                          </v-flex>
                        </v-layout>
                      </v-flex>
                      <v-flex class="no-horizontal-padding xs12 md4 d-flex" style="justify-content: center;">
                        <v-layout row wrap justify-center>
                          <v-flex class="no-horizontal-padding xs12 d-flex" style="justify-content: center; padding-top: 0; padding-bottom: 0">
                            <span style="color: white">Location:</span>
                          </v-flex>
                          <v-flex class="no-horizontal-padding xs12 d-flex" style="justify-content: center; padding-top: 0; padding-bottom: 0">
                            <v-autocomplete
                              v-model="selectedSpecificGeo"
                              :items="possibleSpecificGeo"
                              label="Place"
                              solo
                              hide-details
                              :disabled="selectedGeo === null || selectedGeo === 'world'"
                            >
                              <template slot="item" slot-scope="data">
                                  <span>{{getFieldText(data.item)}}</span>
                              </template>
                            </v-autocomplete>
                          </v-flex>
                        </v-layout>
                      </v-flex>
                       <v-flex class="no-horizontal-padding xs12 md4 d-flex" style="justify-content: center;">
                         <v-layout row wrap justify-center>
                          <v-flex class="no-horizontal-padding xs12 d-flex" style="justify-content: center; padding-top: 0; padding-bottom: 0">
                            <span style="color: white">Lineage (optional) :</span>
                          </v-flex>
                           <v-flex class="no-horizontal-padding xs12 d-flex" style="justify-content: center; padding-top: 0; padding-bottom: 0">
                              <v-autocomplete
                                v-model="selectedLineage"
                                :items="possibleLineage"
                                label="Lineage"
                                solo
                                clearable
                                hide-details
                              >
                              </v-autocomplete>
                           </v-flex>
                         </v-layout>
                      </v-flex>
                       <v-flex class="no-horizontal-padding xs12 md12 d-flex" style="justify-content: center;">
                       </v-flex>
                       <v-flex class="no-horizontal-padding xs12 md4 d-flex" style="justify-content: center;">
                         <v-layout row wrap justify-center>
                          <v-flex class="no-horizontal-padding xs12 d-flex" style="justify-content: center; padding-top: 0; padding-bottom: 0">
                            <span style="color: white">Date:</span>
                          </v-flex>
                           <v-flex class="no-horizontal-padding xs12 d-flex" style="justify-content: center; padding-top: 0; padding-bottom: 0">
                             <v-menu
                                ref="menu"
                                v-model="menu"
                                :close-on-content-click="false"
                                :return-value.sync="selectedDate"
                                transition="scale-transition"
                                offset-y
                                min-width="auto"
                              >
                                <template v-slot:activator="{ on, attrs }">
                                  <v-text-field
                                    v-model="selectedDate"
                                    label="Choose a date"
                                    readonly
                                    solo
                                    hide-details
                                    v-bind="attrs"
                                    v-on="on"
                                    append-icon="mdi-calendar"
                                    @click:append="menu=true"
                                  ></v-text-field>
                                </template>
                                <v-date-picker
                                  v-model="selectedDate"
                                  first-day-of-week="1"
                                  no-title
                                  scrollable
                                >
                                  <v-spacer></v-spacer>
                                  <v-btn
                                    text
                                    color="primary"
                                    @click="menu = false"
                                  >
                                    Cancel
                                  </v-btn>
                                  <v-btn
                                    text
                                    color="primary"
                                    @click="$refs.menu.save(selectedDate)"
                                  >
                                    OK
                                  </v-btn>
                                </v-date-picker>
                              </v-menu>
                           </v-flex>
                         </v-layout>
                       </v-flex>
                       <v-flex class="no-horizontal-padding xs12 md4 d-flex" style="justify-content: center;">
                         <v-layout row wrap justify-center>
                          <v-flex class="no-horizontal-padding xs12 d-flex" style="justify-content: center; padding-top: 0; padding-bottom: 0">
                            <span style="color: white"># Week:</span>
                          </v-flex>
                          <v-flex class="no-horizontal-padding xs12 d-flex" style="justify-content: center; padding-top: 0; padding-bottom: 0">
                            <v-select
                              v-model="selectedWeekNum"
                              :items="possibleWeekNum"
                              label="# Week"
                              solo
                              hide-details
                            ></v-select>
                          </v-flex>
                         </v-layout>
                       </v-flex>
                       <v-flex class="no-horizontal-padding xs12 d-flex" style="justify-content: center;">
                       <v-btn
                               @click="doAnalysis();"
                               color="#011936"
                               class="white--text"
                               :disabled="selectedGeo !== 'world' && (selectedGeo === null || selectedSpecificGeo === null)"
                        >
                            START ANALYSIS
                        </v-btn>
                     </v-flex>
                     </v-layout>
                   </v-card>
                 </v-flex>

                 <v-flex class="no-horizontal-padding xs12 d-flex" style="justify-content: center; margin-top: 50px" v-if="rowsTable.length > 0">
                   <h2>RESULTS: </h2>
                  </v-flex>
                 <v-flex class="no-horizontal-padding xs12 d-flex" style="justify-content: center;">
                   <v-expansion-panels
                    v-model="expansionPanels"
                    :value="expansionPanels"
                    multiple>
                      <v-expansion-panel style="margin-bottom: 10px" v-for="(array_rows, index) in rowsTable" v-bind:key="index">
                        <v-expansion-panel-header :color="toolbar_color">
                            <span style="width: 80%; color: white;">{{expansionPanelsSingleInfo[index]['location']}} /
                                              {{expansionPanelsSingleInfo[index]['lineage']}} /
                                              {{expansionPanelsSingleInfo[index]['date']}} /
                                              {{expansionPanelsSingleInfo[index]['weekNum']}} weeks
                            </span>
                        </v-expansion-panel-header>
                        <v-expansion-panel-content :color="menu_color">
                          <TablesComponent
                            :rowsTable="rowsTable[index]"
                            :singleInfo = expansionPanelsSingleInfo[index]
                            :nameHeatmap="'heatmapWithLineage' + index"
                            :timeName="'timeDistributionWithLineage'+index"
                            :withLineages="true"
                            v-if="rowsTable[index].length > 0">
                          </TablesComponent>
                          <div v-else style="text-align: center; margin-top: 20px">
                            <h2 style="color: white"> Not enough data for this location </h2>
                          </div>
                        </v-expansion-panel-content>
                      </v-expansion-panel>
                   </v-expansion-panels>
                 </v-flex>
               </v-layout>
             </v-card-text>
          </v-card>
        </v-row>
    </v-card>

    <v-overlay :value="overlay">
      <v-progress-circular
        indeterminate
        size="64"
      ></v-progress-circular>
    </v-overlay>

  </div>
</template>

<script>

import {mapActions, mapGetters, mapMutations, mapState} from "vuex";
import axios from "axios";
import TablesComponent from "@/components/TablesComponent";

export default {
  name: "MainPageWithLineages",
  components: {TablesComponent},
  data() {
    return {
      menu: false,
      overlay: false,
      allGeo: null,
      selectedGeo: 'world',
      possibleGeo: ['world', 'continent', 'country', 'region'],
      selectedSpecificGeo: null,
      possibleSpecificGeo: [],
      selectedDate: null,
      selectedWeekNum: 4,
      possibleWeekNum: [2,3,4],

      selectedLineage: null,
      possibleLineage: [],

      expansionPanels: [],
      expansionPanelsSingleInfo: [],

      rowsTable: [],
    }
  },
  computed: {
    ...mapState(['background_card_color', 'menu_color', 'toolbar_color', 'all_geo', 'all_lineages']),
    ...mapGetters({}),
  },
  methods: {
    ...mapMutations([]),
    ...mapActions([]),
    getFieldText(item){
      let name;
      if (item === null){
        name = 'N/D'
      }
      else{
        name = item;
      }
      return name;
    },
    doAnalysis(){
      this.loadTables();
    },
    loadTables(){
      this.overlay = true;
      let countNumAnalysis = this.rowsTable.length;
      let url = `/analyse_mutations/getStatistics`;
      // let url = `/automatic_analysis/getStatistics`;
      let to_send = {
        'granularity': this.selectedGeo,
        'value': this.selectedSpecificGeo,
        'lineage': this.selectedLineage,
        'date': this.selectedDate,
        'numWeek': this.selectedWeekNum};

      axios.post(url, to_send)
        .then((res) => {
          return res.data;
        })
        .then((res) => {
          let that = this;

          this.expansionPanelsSingleInfo[countNumAnalysis] = {
            'weekNum': this.selectedWeekNum,
            'date': this.selectedDate,
            'granularity': this.selectedGeo,
            'location': this.selectedSpecificGeo,
            'lineage': this.selectedLineage
          };

          this.rowsTable[countNumAnalysis] = JSON.parse(JSON.stringify(res)).filter(function (el) {
            let granularity = that.selectedGeo;
            if (that.selectedGeo === 'continent'){
              granularity = 'geo_group';
            }
            return el['granularity'] ===  granularity
          });

          this.selectedWeekNum = 4;
          this.selectedDate = new Date().toISOString().slice(0, 10);
          this.selectedGeo = 'world';
          this.selectedSpecificGeo = null;
          this.selectedLineage = null;
          this.overlay = false;
          this.expansionPanels.push(countNumAnalysis);
        });
    },
  },
  mounted() {
      this.selectedDate = new Date().toISOString().slice(0, 10);
      this.selectedSpecificGeo = null;
      this.possibleSpecificGeo = [];
      this.allGeo = this.all_geo;
      this.possibleLineage = this.all_lineages;
  },
  watch: {
    all_geo(){
      this.allGeo = this.all_geo;
    },
    all_lineages(){
      this.possibleLineage = this.all_lineages;
    },
    selectedGeo(){
      this.possibleSpecificGeo = [];
      let i = 0;
      if (this.selectedGeo !== 'world') {
        while (i < this.allGeo[this.selectedGeo].length) {
          if (this.allGeo[this.selectedGeo][i] != null) {
            this.possibleSpecificGeo.push(this.allGeo[this.selectedGeo][i]);
          } else {
            this.possibleSpecificGeo.push('N/D');
          }
          i = i + 1;
        }
      }
      this.possibleSpecificGeo.sort();
    },
  },
}
</script>

<style scoped>

</style>