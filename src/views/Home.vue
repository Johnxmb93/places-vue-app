<template>
  <div class="home">
    <h1 id="stuff2">{{ message }}</h1>
    <div v-for="place in places" v-bind:key="place.id">
      <h2 id="stuff">{{ place.name }}</h2>
      <h4 id="stuff2">{{ place.address }}</h4>
      <button v-on:click="showPlace(place)">More info!</button>
    </div>
    <div>
      Name:
      <input type="text" v-model="newPlacesParams.name" />
      Address:
      <input type="text" v-model="newPlacesParams.address" />
      <button v-on:click="showPlace(place)">More info!</button>
    </div>
    <button v-on:click="createPlace">Create place</button>
    <div>
      <dialog id="place-details">
        <form method="dialog">
          <h1>Place Info!</h1>
          <p>
            Name:
            <input type="text" v-model="currentPlace.name" />
          </p>
          <p>
            Address:
            <input type="text" v-model="currentPlace.address" />
          </p>
          <button v-on:click="updatePlace(currentPlace)">Update Place</button>
          <button>Close</button>
        </form>
      </dialog>
    </div>
  </div>
</template>
<style></style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "You Are Viewing Places In Chicago!!!",
      places: [],
      newPlacesParams: {},
      currentPlace: {},
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      axios.get("http://localhost:3000/places").then((response) => {
        this.places = response.data;
        console.log("All Places:", this.places);
      });
    },
    createPlace: function () {
      console.log("testing");
      axios
        .post("http://localhost:3000/places", this.newPlacesParams)
        .then((response) => {
          console.log("Success!", response.data);
          this.place.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
    showPlace: function (place) {
      console.log(place);
      this.currentPlace = place;
      document.querySelector("#place-details").showModal();
    },
    updatePlace: function (place) {
      var editPlaceParams = place;
      axios
        .patch("http://localhost:3000/places/" + place.id, editPlaceParams)
        .then((response) => {
          console.log("Success!", response.data);
        })
        .catch((error) => console.log(error.response));
    },
  },
};
</script>

<style>
#stuff {
  color: #ff0000;
}
#stuff2 {
  color: #00a708;
}
</style>
