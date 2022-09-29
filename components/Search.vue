<!-- Please remove this file from your project -->
<template>
  <div class="relative flex items-top justify-center min-h-screen bg-gray-100 sm:items-center sm:pt-0">
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css" rel="stylesheet" />
    <div class="max-w-4xl mx-auto sm:px-6 lg:px-8">
      <input type="text" v-model="myInput" />
      <input type="submit" v-on:click="getData()" />
      <div>{{ username }} {{ level }}</div>
      <div>Wins : {{ wins }}</div>
      <div>Looses : {{ looses }}</div>
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
      userPuuid: "",
      username: "Who?",
      level: "which level ?",
      listMatch: [],
      wins: 0,
      looses: 0,
      winrate: 0,
      apiKey: "RGAPI-78ab5886-ef8e-4ab6-9166-e242d6f2d593" ,
    };
  },
  methods: {
    getData() {
      if (this.myInput != null) {
        axios
          .get(
            "https://euw1.api.riotgames.com/lol/summoner/v4/summoners/by-name/" +
            this.myInput +
            "?api_key=" + this.apiKey
          )
          .then((res) => {
            if (res.data) {
              (this.userPuuid = res.data.puuid),
                (this.username = res.data.name),
                (this.level = "is level " + res.data.summonerLevel);
              this.getListMatch(this.userPuuid);
            }
          })
          .catch((err) => {
            this.username = "User not found";
            this.level = "";
            console.log(err);
          });
      }
    },
    getListMatch(userPuuid) {
      axios
        .get(
          "https://europe.api.riotgames.com/lol/match/v5/matches/by-puuid/" +
          userPuuid +
          "/ids?start=0&count=19&api_key=" + this.apiKey
        )
        .then((res) => {
          this.listMatch = res.data;
          this.getWinLoose(this.listMatch);
        });
    },
    getWinLoose(listMatch) {
      listMatch.forEach((match) => {
        axios
          .get(
            "https://europe.api.riotgames.com/lol/match/v5/matches/" +
            match +
            "?api_key=" + this.apiKey
          )
          .then((res, win) => {
            res.data.info.participants.forEach((user) => {
              if (user.puuid == this.userPuuid) {
                win = user.win;
                if (win) {
                  this.wins += 1;
                } else this.looses += 1;
              }
            });
            this.getWinrate();
          });
      });
    },
    getWinrate() {
      return (this.winrate =
        ((this.wins / (this.wins + this.looses)) * 100).toFixed(2) + "%");
    },
  },
};
</script>
