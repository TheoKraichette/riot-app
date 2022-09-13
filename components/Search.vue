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
     {{ username }} is {{ level }}
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
      level: "Which level ?",
    };
  },
  mounted() {
    axios
      .get(
        "https://euw1.api.riotgames.com/lol/summoner/v4/summoners/by-name/blqlol?api_key=RGAPI-5e966363-7318-48a2-ac75-17d9623cf4d3"
      )
      .then((res) => (
        this.username = res.data.name,
        this.level = ("level " + res.data.summonerLevel)));
  },
};
</script>
