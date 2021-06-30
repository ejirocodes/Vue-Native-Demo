<template>
  <view class="container">
    <text class="text-color-primary">My Awesome Vue Native App</text>
    <text class="text-color-primary">{{ name }}</text>
    <text-input
      :style="{
        height: 30,
        width: 250,
        borderColor: '#184d47',
        borderWidth: 1,
        paddingLeft: 10,
      }"
      v-model="user.country"
    />
    <text> {{ user.country }}</text>

    <!-- Network image -->
    <image
      :style="{ width: 200, height: 120 }"
      :source="{
        uri: 'https://images.unsplash.com/photo-1621570074981-ee6a0145c8b5?ixid=MnwxMjA3fDF8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=750&q=80',
      }"
    />
    <!-- Static image -->
    <image
      :style="{ width: 200, height: 120 }"
      :source="require('./assets/photo.jpg')"
    />
    <!-- local image -->
    <image
      :style="{ width: 66, height: 58 }"
      :source="{
        uri: 'data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADMAAAAzCAYAAAA6oTAqAAAAEXRFWHRTb2Z0d2FyZQBwbmdjcnVzaEB1SfMAAABQSURBVGje7dSxCQBACARB+2/ab8BEeQNhFi6WSYzYLYudDQYGBgYGBgYGBgYGBgYGBgZmcvDqYGBgmhivGQYGBgYGBgYGBgYGBgYGBgbmQw+P/eMrC5UTVAAAAABJRU5ErkJggg==',
      }"
    />
    <button
      :on-press="getLocation"
      title="Get Location"
      color="#184d47"
      accessibility-label="Get access to users' location"
    />
    <text>Location Details:</text>
    <text>{{ location }}</text>
    <text>Latitude: {{ latitude }}</text>
    <text>Longitude: {{ longitude }}</text>

    <text class="text-error">{{ errorMessage }}</text>
    <view>
      <text v-for="user in users" :key="user.id">
        Email: {{ user.email }}
      </text>
    </view>
  </view>
</template>


<script>
import store from "./store";
import * as Location from "expo-location";

export default {
  data() {
    return {
      location: "",
      latitude: "",
      longitude: "",
      errorMessage: "",
      text: "",
      user: {
        country: "",
      },
      users: [
        {
          id: 1,
          name: "Ejiro",
          email: "ejiroasiuhu10@gmailc.com",
        },
        {
          id: 2,
          name: "Sam",
          email: "samueljinx@gmail.com",
        },
      ],
    };
  },
  methods: {
    async getLocation() {
      try {
        let { status } = await Location.requestForegroundPermissionsAsync();
        if (status !== "granted") {
          this.errorMessage = "Permission to access location was denied";
          return;
        }
        let location = await Location.getCurrentPositionAsync({});
        this.location = location;
        this.latitude = location.coords.latitude;
        this.longitude = location.coords.longitude;
        this.errorMessage = "";
      } catch (error) {
        this.errorMessage = error;
      }
    },
  },
  computed: {
    name() {
      return store.state.name;
    },
  },
};
</script>
<style>
.container {
  background-color: white;
  align-items: center;
  justify-content: center;
  flex: 1;
}
.text-color-primary {
  color: #184d47;
}
</style>
