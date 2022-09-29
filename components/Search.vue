<!-- Please remove this file from your project -->
<template>
  <div
    class="relative flex items-top justify-center min-h-screen bg-gray-100 sm:items-center sm:pt-0"
  >
    <link
      href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css"
      rel="stylesheet"
    />
    <div class="max-w-4xl mx-auto sm:px-6 lg:px-8">
      <input type="text" v-model="myInput" />
      <input type="submit" v-on:click="getData()" />
      <div>{{ username }} {{ level }}</div>
      <div>Wins : {{ wins }}</div>
      <div>losses : {{ losses }}</div>
      <div>Winrate : {{ winrate }}</div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Search",
  data() {
    return {
      userId: "",
      username: "Who?",
      level: "which level ?",
      wins: 0,
      losses: 0,
      winrate: 0,
      apiKey: "RGAPI-78ab5886-ef8e-4ab6-9166-e242d6f2d593",
    };
  },
  methods: {
    getData() {
      if (this.myInput != null) {
        axios
          .get(
            "https://euw1.api.riotgames.com/lol/summoner/v4/summoners/by-name/" +
              this.myInput +
              "?api_key=" +
              this.apiKey
          )
          .then((res) => {
            if (res.data) {
              (this.userId = res.data.id),
                (this.username = res.data.name),
                (this.level = "is level " + res.data.summonerLevel);
              this.getWinLoose(this.userId);
            }
          })
          .catch((err) => {
            this.username = "User not found";
            this.level = "";
            console.log(err);
          });
      }
    },
    getWinLoose(userId) {
      axios
        .get(
          "https://euw1.api.riotgames.com/lol/league/v4/entries/by-summoner/" +
            userId +
            "?api_key=" +
            this.apiKey
        )
        .then((res) => {
          console.log(res);
          this.wins = res.data[0].wins;
          this.losses = res.data[0].losses;
          this.getWinrate();
        });
    },
    getWinrate() {
      return (this.winrate =
        ((this.wins / (this.wins + this.losses)) * 100).toFixed(2) + "%");
    },
  },
};
</script>
