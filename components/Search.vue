<!-- Please remove this file from your project -->
<template>
  <div
    class="
      relative
      flex
      items-top
      justify-center
      min-h-screen
      bg-gray-100
      sm:items-center sm:pt-0
    "
  >
    <link
      href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css"
      rel="stylesheet"
    />
    <div class="max-w-4xl mx-auto sm:px-6 lg:px-8">
      <input type="text" v-model="myInput" />
      <input type="submit" v-on:click="getData()" />
      <div>{{ username }} {{ level }}</div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Search",
  data() {
    return {
      username: "Who?",
      level: "which level ?",
    };
  },
  methods: {
    getData() {
      console.log(this.myInput);
      if (this.myInput != null) {
        axios
          .get(
            "https://euw1.api.riotgames.com/lol/summoner/v4/summoners/by-name/" +
              this.myInput +
              "?api_key=RGAPI-a76af830-6531-4b55-81dd-26dc42f63949"
          )
          .then((res) => {
            if (res.data) {
              (this.username = res.data.name),
                (this.level = "is level " + res.data.summonerLevel);
            }
          })
          .catch((err) => {
            this.username = "User not found";
            this.level = "";
            console.log(err);
          });
      }
    },
  },
};
</script>
