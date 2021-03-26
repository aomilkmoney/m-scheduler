<template>
  <v-container>
    <v-row no-gutters>
      <v-col cols="4">
        <v-card style="height: 76px">
          <v-card-text> Inventory Item </v-card-text>
        </v-card>
      </v-col>

      <v-col v-for="week in weeks" :key="week.throughDates" cols="1">
        <v-card>
          <v-card-text>3/22 - 3/28 </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-row no-gutters>
      <v-col cols="4">
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
        <v-row
          v-for="appointment in appointments"
          :key="appointment.id"
          no-gutters
        >
          <v-col cols="">
            <v-card style="height: 83px">
              <v-btn
                class="mt-7"
                small
                rounded
                color="primary"
                :style="appointment.marginLeft + appointment.width"
                >Lamar</v-btn
              >
            </v-card>
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
      ],
      items: [
        { title: '35th and Boston' },
        { title: 'Pacific and Redfield' },
        { title: 'Pacific and Redfield' },
        { title: 'Pacific and Redfield' },
        { title: 'Pacific and Redfield' },
        { title: 'Pacific and Redfield' },
        { title: 'Pacific and Redfield' },
        { title: 'Pacific and Redfield' },
        { title: 'Pacific and Redfield' },
        { title: 'Pacific and Redfield' },
        { title: 'Pacific and Redfield' },
        { title: 'Pacific and Redfield' },
      ],
      appointments: [
        {
          id: 'adsafa',
          vendorName: 'Lamar',
          width: 'width: 50%',
          marginLeft: 'margin-left: 100px;',
        },
        {
          id: 'sadgat',
          vendorName: 'Lamar',
          width: 'width: 20%',
          marginLeft: 'margin-left: 150px;',
        },
        {
          id: 'fgsdftr',
          vendorName: 'Lamar',
          width: 'width: 50%',
          marginLeft: 'margin-left: 270px;',
        },
        {
          id: '3452t',
          vendorName: 'Lamar',
          width: 'width: 20%',
          marginLeft: 'margin-left: 200px;',
        },
        {
          id: 'scvdsga',
          vendorName: 'Lamar',
          width: 'width: 50%',
          marginLeft: 'margin-left: 50px;',
        },
        {
          id: 'adsdrtafa',
          vendorName: 'Lamar',
          width: 'width: 70%',
          marginLeft: 'margin-left: 150px;',
        },
        {
          id: 'ads343afa',
          vendorName: 'Lamar',
          width: 'width: 50%',
          marginLeft: 'margin-left: 210px;',
        },
        {
          id: 'adsagrsedazfa',
          vendorName: 'Lamar',
          width: 'width: 20%',
          marginLeft: 'margin-left: 230px;',
        },
      ],
    }
  },
}
</script>
