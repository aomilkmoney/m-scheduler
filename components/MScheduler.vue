<template>
  <v-container style="max-width: 1200px">
    <v-row no-gutters>
      <v-col cols="3">
        <v-btn
          text
          color="primary"
          class="mt-3 ml-5"
          @click="openBlackoutDialog"
        >
          Add Blackout Dates
        </v-btn>
      </v-col>
      <v-col cols="9">
        <v-row
          id="fixed-header-row"
          class="flex-nowrap"
          no-gutters
          style="overflow-x: scroll"
          @scroll="scrollLinkHorizontal"
        >
          <v-col cols="2">
            <v-card>
              <v-card-text> 04/05 - 04/11</v-card-text>
            </v-card>
          </v-col>
          <v-col cols="2">
            <v-card>
              <v-card-text> 04/12 - 04/18 </v-card-text>
            </v-card>
          </v-col>
          <v-col cols="2">
            <v-card>
              <v-card-text> 04/19 - 04/25</v-card-text>
            </v-card>
          </v-col>
          <v-col cols="2">
            <v-card>
              <v-card-text> 04/26 - 05/02 </v-card-text>
            </v-card>
          </v-col>
          <v-col cols="2">
            <v-card>
              <v-card-text> 05/03 - 05/09 </v-card-text>
            </v-card>
          </v-col>
          <v-col cols="2">
            <v-card>
              <v-card-text> 05/10 - 05/16 </v-card-text>
            </v-card>
          </v-col>
          <v-col cols="2">
            <v-card>
              <v-card-text> 05/17 - 05/23 </v-card-text>
            </v-card>
          </v-col>
          <v-col cols="2">
            <v-card>
              <v-card-text> 05/24 - 05/30 </v-card-text>
            </v-card>
          </v-col>
          <v-col cols="2">
            <v-card>
              <v-card-text> 05/31 - 06/06 </v-card-text>
            </v-card>
          </v-col>
          <v-col cols="2">
            <v-card>
              <v-card-text> 06/07 - 06/13 </v-card-text>
            </v-card>
          </v-col>
          <v-col cols="2">
            <v-card>
              <v-card-text> 06/14 - 06/20 </v-card-text>
            </v-card>
          </v-col>
          <v-col cols="2">
            <v-card>
              <v-card-text> 06/21 - 06/27 </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
    <v-row no-gutters>
      <v-col
        id="fixed-sidebar"
        cols="3"
        style="overflow-y: scroll; overflow-x: hidden; max-height: 722px"
        @scroll="scrollLinkVertical"
      >
        <v-row>
          <v-col>
            <v-list two-line class="py-0">
              <v-list-item
                v-for="(item, i) in items"
                :key="i"
                style="height: 83px"
              >
                <v-list-item-avatar>
                  <v-icon>mdi-image</v-icon>
                </v-list-item-avatar>
                <v-list-item-content>
                  <v-list-item-title
                    v-text="item.panelNumber"
                  ></v-list-item-title>
                  <v-list-item-subtitle
                    v-text="item.dma"
                  ></v-list-item-subtitle>
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-col>
        </v-row>
      </v-col>
      <v-col
        id="blackout-container"
        cols="9"
        style="max-height: 722px; overflow-x: scroll; overflow-y: scroll"
        @scroll="scrollLinkBothDirections"
      >
        <v-row no-gutters>
          <v-col
            v-for="item in items"
            :key="item.title"
            cols="12"
            style="height: 83px"
          >
            <div
              id="blackout.id"
              style="
                height: 83px;
                width: 1686px;
                position: relative;
                border-bottom: 1px dotted lightgrey;
              "
              @click="openBlackoutDialog"
            >
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
              <div
                v-for="n in 11"
                :key="n"
                class="vl"
                :style="'left: ' + n * 142.11 + 'px'"
              ></div>
            </div>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
    <m-dialog
      :value.sync="blackoutDialog"
      :title="'Blackout Dates For Item#' + blackoutDialogID"
      cancel-button-text="Cancel"
      submit-button-text="Submit"
      width="600"
      style="overflow: hidden"
      @submit="submitBlackout"
    >
      <v-row>
        <v-col cols="6">
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
              <v-btn text color="primary" @click="menu = false"> Cancel </v-btn>
              <v-btn text color="primary" @click="$refs.menu.save(startDate)">
                OK
              </v-btn>
            </v-date-picker>
          </v-menu>
        </v-col>
        <v-col cols="6">
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
              <v-btn text color="primary" @click="$refs.menu2.save(endDate)">
                OK
              </v-btn>
            </v-date-picker>
          </v-menu>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="6">
          <v-text-field label="Client Name"></v-text-field>
        </v-col>
        <v-col class="d-flex" cols="6">
          <v-select :items="statuses" label="Status"></v-select>
        </v-col>
      </v-row>
    </m-dialog>
  </v-container>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      blackoutDialogID: null,
      startDate: new Date().toISOString().substr(0, 10),
      endDate: new Date().toISOString().substr(0, 10),
      statuses: ['Booked', 'On Hold'],
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
          panelNumber: 'adfe2Q',
          dma: 'Los Angeles',
          blackouts: [
            {
              id: 'adsafa',
              clientName: 'Naz X',
              width: 'width: 136px',
              leftAbsolute: 'left: 4px;',
              panelNumber: 'adfe2Q',
              dma: 'Los Angeles',
            },
            {
              id: 'sadgat',
              clientName: 'Lil beeb',
              width: 'width: 217px',
              leftAbsolute: 'left: 150px;',
            },
          ],
        },
        {
          title: 'Pacific and Redfield',
          panelNumber: 'acbfeN4',
          dma: 'New York',
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
          title: '30th and Tweed',
          panelNumber: 'aadftQ',
          dma: 'Detroit',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Kanye',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
              panelNumber: 'adfe2Q',
              dma: 'Los Angeles',
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
          title: 'Chesterfield and Redwood',
          panelNumber: 'desaf2Q',
          dma: 'Los Angeles',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Harrison Ford',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
              panelNumber: 'adfe2Q',
              dma: 'Los Angeles',
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
          title: 'Gary and Cinese',
          panelNumber: 'adfe2Q',
          dma: 'Los Angeles',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Buford Tannen',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
              panelNumber: 'adfe2Q',
              dma: 'Los Angeles',
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
          title: 'Western and Fairfax',
          panelNumber: 'adfe2Q',
          dma: 'Los Angeles',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Dr. Emmet Brown',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
              panelNumber: 'adfe2Q',
              dma: 'Los Angeles',
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
          title: '6th and Vine',
          panelNumber: 'adfe2Q',
          dma: 'Los Angeles',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
              panelNumber: 'adfe2Q',
              dma: 'Los Angeles',
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
          title: 'Harold and Kumar',
          panelNumber: 'adfe2Q',
          dma: 'Los Angeles',
          blackouts: [
            {
              id: 'fgsdftr',
              clientName: 'Lamar',
              width: 'width: 217px',
              leftAbsolute: 'left: 232px;',
              panelNumber: 'adfe2Q',
              dma: 'Los Angeles',
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
          panelNumber: 'adfe2Q',
          dma: 'Los Angeles',
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
          panelNumber: 'adfe2Q',
          dma: 'Los Angeles',
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
          panelNumber: 'adfe2Q',
          dma: 'Los Angeles',
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
  // mounted() {
  //   const fixedHeader = document.getElementById('fixed-header-row')
  //   fixedHeader.onscroll = this.scrollLinkHorizontal
  //   const blackoutContainer = document.getElementById('blackout-container')
  //   blackoutContainer.onscroll = this.scrollLinkHorizontal
  // },
  methods: {
    allowedEndDates: (val) => new Date(val).getDay() === 6,
    allowedStartDates: (val) => new Date(val).getDay() === 0,
    openBlackoutDialog(event) {
      this.blackoutDialog = true
      this.blackoutDialogID = event.target.id
      console.log(event.target)
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
        }, 50)
      }
    },
    scrollLinkVertical(event) {
      console.log('scrollLinkVertical triggered')
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
        }, 50)
      }
    },
    scrollLinkBothDirections(event) {
      this.scrollLinkHorizontal(event)
      this.scrollLinkVertical(event)
    },
    submitBlackout() {
      console.log('Blackout submitted')
      this.blackoutDialog = false
    },
  },
}
</script>

<style>
.vl {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  top: 0;
  z-index: 0;
}
</style>
