<template>
  <v-container
    style="
      padding: 20px;
      max-height: 800px;
      overflow-y: scroll;
      overflow-x: scroll;
      max-width: 1200px;
    "
  >
    <v-row no-gutters style="">
      <v-row
        no-gutters
        style="
          position: fixed;
          max-width: 1200px;
          overflow-x: scroll;
          height: 83px;
          z-index: 2;
        "
      >
        <v-col cols="4" style="max-width: 373.333">
          <v-card>
            <v-card-text style="height: 83px"> Inventory Item </v-card-text>
          </v-card>
        </v-col>
        <v-col cols="8">
          <v-row no-gutters>
            <v-col
              v-for="week in weeks"
              :key="week.throughDates"
              style="min-width: 76px"
              cols="1"
            >
              <v-card style="height: 83px">
                <v-card-text>3/22 - 3/28 </v-card-text>
              </v-card>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
      <v-col cols="4" style="padding-top: 86px">
        <v-list style="padding: 0px">
          <v-list-item v-for="(item, i) in items" :key="i" style="padding: 0px">
            <v-card style="width: 100%; height: 83px" class="justify-center">
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

      <v-col cols="8">
        <v-row no-gutters style="min-width: 900px; padding-top: 86px"> </v-row>
        <v-row v-for="item in items" :key="item.title" no-gutters>
          <v-col cols="">
            <div style="height: 83px; min-width: 900px; position: relative">
              <v-btn
                v-for="appointment in item.appointments"
                :key="appointment.id"
                class="mt-7"
                small
                rounded
                color="error"
                :style="appointment.leftAbsolute + appointment.width"
                style="z-index: 1"
                >{{ appointment.clientName }}
              </v-btn>
              <div v-for="n in 11" :key="n" :class="'vl' + n"></div>
            </div>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  components: {},
  data() {
    return {
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
          appointments: [
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
          appointments: [
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
          appointments: [
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
          appointments: [
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
          appointments: [
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
          appointments: [
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
          appointments: [
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
          appointments: [
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
          appointments: [
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
          appointments: [
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
          appointments: [
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
          appointments: [
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
          appointments: [
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
          appointments: [
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
  left: 74px;
  top: 0;
  z-index: 0;
}
.vl2 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 149px;
  top: 0;
  z-index: 0;
}
.vl3 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 224px;
  top: 0;
  z-index: 0;
}
.vl4 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 299px;
  top: 0;
  z-index: 0;
}
.vl5 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 374px;
  top: 0;
  z-index: 0;
}
.vl6 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 449px;
  top: 0;
  z-index: 0;
}
.vl7 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 524px;
  top: 0;
  z-index: 0;
}
.vl8 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 599px;
  top: 0;
  z-index: 0;
}
.vl9 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 674px;
  top: 0;
  z-index: 0;
}
.vl10 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 749px;
  top: 0;
  z-index: 0;
}
.vl11 {
  border-left: 1px dotted lightgrey;
  height: 81px;
  position: absolute;
  left: 824px;
  top: 0;
  z-index: 0;
}
</style>
