<template>
  <div>
    <header>
      <h1 class="title">Boost<span class="ed">ed</span>.</h1>
      <h2 class="ornot">or not</h2>
    </header>
    <main class="main flex items-top justify-center sm:items-center sm:pt-0">
      <div class="max-w-4xl mx-auto sm:px-6 lg:px-8">
        <div class="now">Now, see if u r boosted.</div>
        <input
          class="input"
          type="text"
          v-model="myInput"
          placeholder="Summoner's name"
        />
        <button class="send" type="submit" v-on:click="getData()">
          <svg
            width="21"
            height="21"
            viewBox="0 0 21 21"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M10.012 0.995C4.49401 0.995 0.0140076 5.475 0.0140076 10.993C0.0140076 16.511 4.49401 20.991 10.012 20.991C15.53 20.991 20.009 16.511 20.009 10.993C20.009 5.475 15.53 0.995 10.012 0.995ZM10.012 2.495C14.702 2.495 18.509 6.303 18.509 10.993C18.509 15.683 14.702 19.491 10.012 19.491C5.32201 19.491 1.51401 15.683 1.51401 10.993C1.51401 6.303 5.32201 2.495 10.012 2.495ZM11.54 7.21C11.54 7.21 13.042 8.715 14.795 10.469C14.941 10.616 15.014 10.808 15.014 11C15.014 11.192 14.941 11.383 14.795 11.53C13.042 13.284 11.541 14.788 11.541 14.788C11.396 14.933 11.205 15.005 11.014 15.005C10.823 15.004 10.631 14.931 10.484 14.784C10.191 14.491 10.189 14.018 10.48 13.727L12.458 11.75H5.76401C5.35001 11.75 5.01401 11.414 5.01401 11C5.01401 10.586 5.35001 10.25 5.76401 10.25H12.458L10.479 8.271C10.19 7.982 10.193 7.509 10.485 7.217C10.632 7.07 10.824 6.996 11.016 6.995C11.206 6.995 11.396 7.066 11.54 7.21Z"
              fill="#808080"
            />
          </svg>
        </button>
        <div>{{ username }} {{ level }} {{ winrate }}</div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from "axios";
import "@/styles/styles.css";
export default {
  name: "Search",
  data() {
    return {
      userId: "",
      username: "",
      level: "",
      wins: 0,
      losses: 0,
      winrate: "",
      apiKey: "RGAPI-d54def72-6c63-466c-aae6-25ab9ae01094",
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
            this.winrate = "";
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
          let datas = res.data;
          if (datas.length > 0) {
            this.wins = res.data[0].wins;
            this.losses = res.data[0].losses;
            this.getWinrate();
          } else {
            this.wins = 0;
            this.losses = 0;
            this.winrate = "winrate: 0%";
          }
        });
    },
    getWinrate() {
      return (this.winrate =
        "winrate: " +
        ((this.wins / (this.wins + this.losses)) * 100).toFixed(2) +
        "%");
    },
  },
};
</script>
