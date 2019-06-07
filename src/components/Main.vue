<template>
  <div>
    <br>
    <div class="container">
      <div class="row">
        <div class="col-md-12">
          <div v-for="user in users" v-bind:key="user.id">
            <div class="card">
              <div class="card-body">
                <h5>Email: {{user.email}}</h5>
                <p>{{user.last_name}}, {{user.first_name}}</p>
              </div>
            </div>
            <br>
          </div>
        </div>
      </div>
    </div>
  </div>
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
        .get(
          "https://reqres.in/api/users?&per_page=8&page=" +
            (this.page += 1)
        )
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

