<template style="background: white">
  <div style="background: white">
    <v-container
      style="
        max-width: 1200px;
        position: fixed;
        z-index: 3;
        background: white;
        padding: 20px;
      "
    >
      <v-row
        id="fixed-header-row"
        no-gutters
        style="overflow-x: scroll; min-width: 900px; height: 83px"
      >
        <v-col cols="3" style="min-width: 276px">
          <v-card>
            <v-card-text style="height: 83px"></v-card-text>
          </v-card>
        </v-col>
        <v-col cols="9">
          <v-row no-gutters class="flex-nowrap" style="min-width: 900px">
            <v-col v-for="(week, index) in weeks" :key="week.throughDates">
              <v-card style="min-width: 151px; max-width: 151px; height: 83px">
                <v-card-text>{{ 'week ' + (index + 1) }}</v-card-text>
              </v-card>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
    </v-container>
    <v-container
      id="blackout-container"
      style="
        padding: 20px;
        max-height: 800px;
        overflow-y: scroll;
        overflow-x: scroll;
        max-width: 1200px;
        background: white;
      "
    >
      <v-row no-gutters>
        <div
          id="fixed-sidebar"
          style="
            height: 750px;
            overflow-y: scroll;
            position: fixed;
            z-index: 2;
            background: white;
          "
        >
          <v-col cols="3" style="padding-top: 106px; min-width: 276px">
            <v-list style="padding: 0px">
              <v-list-item
                v-for="(item, i) in items"
                :key="i"
                style="padding: 0px"
              >
                <v-card
                  style="width: 100%; height: 83px"
                  class="justify-center"
                >
                  <v-card-text class="text-center">
                    <v-list-item-content>
                      <v-list-item-title
                        class="pb-2"
                        v-text="item.title"
                      ></v-list-item-title>
                    </v-list-item-content>
                  </v-card-text>
                </v-card>
              </v-list-item>
            </v-list>
          </v-col>
        </div>

        <v-col cols="9" style="margin-left: 296px">
          <v-row no-gutters style="min-width: 900px; padding-top: 106px">
          </v-row>
          <v-row v-for="item in items" :key="item.title" no-gutters>
            <v-col cols="">
              <div style="height: 83px; min-width: 900px; position: relative">
                <v-btn
                  v-for="blackout in item.blackouts"
                  :key="blackout.id"
                  class="mt-7"
                  small
                  rounded
                  color="error"
                  :style="blackout.leftAbsolute + blackout.width"
                  style="z-index: 1"
                  @click="openBlackoutDialog"
                  >{{ blackout.clientName }}
                </v-btn>
                <div v-for="n in 12" :key="n" :class="'vl' + n"></div>
              </div>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
      <v-dialog
        v-model="blackoutDialog"
        persistent
        max-width="600px"
        style="overflow: hidden"
      >
        <v-card>
          <v-container>
            <v-row>
              <v-col cols="8">
                <v-card-title> Create New Blackout </v-card-title>
              </v-col>
              <v-spacer></v-spacer>
              <v-col>
                <v-btn
                  class="ml-10 mt-2"
                  x-small
                  text
                  fab
                  @click="blackoutDialog = false"
                  >X</v-btn
                >
              </v-col>
            </v-row>
          </v-container>

          <v-card-text>
            <v-row>
              <v-col cols="12">
                Here you will create a new blackout set!
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12" sm="6" md="4">
                <v-menu
                  ref="menu"
                  v-model="menu"
                  :close-on-content-click="false"
                  :return-value.sync="startDate"
                  transition="scale-transition"
                  offset-y
                  min-width="auto"
                >
                  <template #activator="{ on, attrs }">
                    <v-text-field
                      v-model="startDate"
                      label="Select start date"
                      prepend-icon="mdi-calendar"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-date-picker
                    v-model="startDate"
                    :allowed-dates="allowedStartDates"
                    scrollable
                  >
                    <v-spacer></v-spacer>
                    <v-btn text color="primary" @click="menu = false">
                      Cancel
                    </v-btn>
                    <v-btn
                      text
                      color="primary"
                      @click="$refs.menu.save(startDate)"
                    >
                      OK
                    </v-btn>
                  </v-date-picker>
                </v-menu>
              </v-col>
              <v-col cols="12" sm="6" md="4">
                <v-menu
                  ref="menu2"
                  v-model="menu2"
                  :close-on-content-click="false"
                  :return-value.sync="endDate"
                  transition="scale-transition"
                  offset-y
                  min-width="auto"
                >
                  <template #activator="{ on, attrs }">
                    <v-text-field
                      v-model="endDate"
                      label="Select end date"
                      prepend-icon="mdi-calendar"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-date-picker
                    v-model="endDate"
                    :allowed-dates="allowedEndDates"
                    scrollable
                  >
                    <v-spacer></v-spacer>
                    <v-btn text color="primary" @click="menu2 = false">
                      Cancel
                    </v-btn>
                    <v-btn
                      text
                      color="primary"
                      @click="$refs.menu2.save(endDate)"
                    >
                      OK
                    </v-btn>
                  </v-date-picker>
                </v-menu>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12" sm="6" md="3">
                <v-text-field label="Client Name"></v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12" md="6">
                <v-textarea
                  name="input-7-1"
                  label="Notes"
                  value=""
                  hint="A few notes for you later."
                ></v-textarea>
              </v-col>
            </v-row>
            <v-row> </v-row>
          </v-card-text>
        </v-card>
      </v-dialog>
    </v-container>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      startDate: new Date().toISOString().substr(0, 10),
      endDate: new Date().toISOString().substr(0, 10),
      menu: false,
      menu2: false,
      blackoutDialog: false,
      horizontalDebounce: true,
      verticalDebounce: true,
      weeks: [
        { throughDates: '3/22-3/29' },
        { throughDates: '3/22-3/29' },
        { throughDates: '3/22-3/29' },
        { throughDates: '3/22-3/29' },
        { throughDates: '3/22-3/29' },
        { throughDates: '3/22-3/29' },
        { throughDates: '3/22-3/29' },
        { throughDates: '3/22-3/29' },
        { throughDates: '3/22-3/29' },
        { throughDates: '3/22-3/29' },
        { throughDates: '3/22-3/29' },
        { throughDates: '3/22-3/29' },
      ],
      items: [
        {
          title: '35th and Boston',
          blackouts: [
            {
              id: 'adsafa',
              clientName: 'Naz X',
              width: 'width: 142px',
              leftAbsolute: 'left: 4px;',
            },
            {
              id: 'sadgat',
              clientName: 'Lil beeb',
              width: 'width: 217px',
              leftAbsolute: 'left: 236px;',
            },
          ],
        },
        {
          title: 'Pacific and Redfield',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Rhianna',
              width: 'width: 217px',
              leftAbsolute: 'left: 79px;',
            },
            {
              id: '3452t',
              clientName: 'Jay-Z',
              width: 'width: 217px',
              leftAbsolute: 'left: 236px;',
            },
          ],
        },
        {
          title: 'Pacific and Redfield',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Kanye',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
            {
              id: '3452t',
              clientName: 'Kanye Again',
              width: 'width: 217px',
              leftAbsolute: 'left: 462px;',
            },
          ],
        },
        {
          title: 'Pacific and Redfield',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Harrison Ford',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
            {
              id: '3452t',
              clientName: 'Jethro Tull',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
          ],
        },
        {
          title: 'Pacific and Redfield',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Buford Tannen',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
            {
              id: '3452t',
              clientName: 'Amos McFly',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
          ],
        },
        {
          title: 'Pacific and Redfield',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Dr. Emmet Brown',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
            {
              id: '3452t',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
          ],
        },
        {
          title: 'Pacific and Redfield',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
            {
              id: '3452t',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
          ],
        },
        {
          title: 'Pacific and Redfield',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
            {
              id: '3452t',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
          ],
        },
        {
          title: 'Pacific and Redfield',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
            {
              id: '3452t',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
          ],
        },
        {
          title: 'Pacific and Redfield',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
            {
              id: '3452t',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
          ],
        },
        {
          title: 'Pacific and Redfield',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
            {
              id: '3452t',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
          ],
        },
        {
          title: 'Pacific and Redfield',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
            {
              id: '3452t',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
          ],
        },
        {
          title: 'Pacific and Redfield',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
            {
              id: '3452t',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
          ],
        },
        {
          title: 'Pacific and Redfield',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
            {
              id: '3452t',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
            },
          ],
        },
      ],
    }
  },
  mounted() {
    const fixedHeader = document.getElementById('fixed-header-row')
    fixedHeader.onscroll = this.scrollLinkHorizontal
    const blackoutContainer = document.getElementById('blackout-container')
    blackoutContainer.onscroll = this.scrollLinkHorizontal
  },
  methods: {
    allowedEndDates: (val) => new Date(val).getDay() === 6,
    allowedStartDates: (val) => new Date(val).getDay() === 0,
    openBlackoutDialog() {
      this.blackoutDialog = true
    },
    scrollLinkHorizontal(event) {
      if (this.horizontalDebounce) {
        this.horizontalDebounce = false
        if (event.target.id === 'fixed-header-row') {
          const linkedContainer = document.getElementById('blackout-container')
          linkedContainer.scrollLeft = event.target.scrollLeft
        } else {
          const linkedContainer = document.getElementById('fixed-header-row')
          linkedContainer.scrollLeft = event.target.scrollLeft
        }
        setTimeout(() => {
          this.horizontalDebounce = true
        }, 500)
      }
    },
    scrollLinkVertical(event) {
      if (this.verticalDebounce) {
        this.verticalDebounce = false
        if (event.target.id === 'fixed-sidebar') {
          const linkedContainer = document.getElementById('blackout-container')
          linkedContainer.scrollTop = event.target.scrollTop
        } else {
          const linkedContainer = document.getElementById('fixed-sidebar')
          linkedContainer.scrollTop = event.target.scrollTop
        }
        setTimeout(() => {
          this.verticalDebounce = true
        }, 500)
      }
    },
  },
}
</script>

<style>
.sticky-row {
  display: block;
  position: fixed;
  min-width: 800px;
  height: 76px;
}
.vl1 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 149px;
  top: 0;
  z-index: 0;
}
.vl2 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 298px;
  top: 0;
  z-index: 0;
}
.vl3 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 448px;
  top: 0;
  z-index: 0;
}
.vl4 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 598px;
  top: 0;
  z-index: 0;
}
.vl5 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 748px;
  top: 0;
  z-index: 0;
}
.vl6 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 898px;
  top: 0;
  z-index: 0;
}
.vl7 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 1048px;
  top: 0;
  z-index: 0;
}
.vl8 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 1198px;
  top: 0;
  z-index: 0;
}
.vl9 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 1348px;
  top: 0;
  z-index: 0;
}
.vl10 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 1498px;
  top: 0;
  z-index: 0;
}
.vl11 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 1648px;
  top: 0;
  z-index: 0;
}
.vl12 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 1795px;
  top: 0;
  z-index: 0;
}
</style>
