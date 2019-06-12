<template>
  <v-container>
    <v-layout>
      <v-flex sm6 offset-sm3>
        <div v-for="user in users" v-bind:key="user.id">
          <v-card>
            <v-card-title>
              <div>
                <div class="headline">Email: {{user.email}}</div>
                <span>{{user.last_name}}, {{user.first_name}}</span>
              </div>
            </v-card-title>
          </v-card>
          <br>
        </div>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      page: 0,
      users: [],
      allData: false
    };
  },
  created() {
    this.fetchApi();
    this.scroll();
  },
  methods: {
    fetchApi() {
      this.axios
        .get("https://reqres.in/api/users?&per_page=8&page=" + (this.page += 1))
        .then(res => {
          let data = res.data.data;
          console.log(data.length);
          if (data.length > 0) {
            for (var i = 0; i < data.length; i++) {
              this.users.push(data[i]);
            }
          } else {
            this.allData = true;
            this.page -= 1;
          }
        });
    },
    scroll() {
      window.onscroll = () => {
        let bottomOfWindow =
          Math.max(
            window.pageYOffset,
            document.documentElement.scrollTop,
            document.body.scrollTop
          ) +
            window.innerHeight ===
          document.documentElement.offsetHeight;
        if (bottomOfWindow) {
          this.fetchApi();
          console.log("reached the bottom");
        }
      };
    }
  }
};
</script>

