<template>
  <div>
    <button @click="readNotice">{{ countNotice }}</button>
    <ul>
      <li v-for="item in list" :key="item.id">
        {{JSON.stringify(item)}}</li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';
import Echo from 'laravel-echo';


export default {
  name: 'HelloWorld',
  data() {
    return {
      list: [],
      countNotice: 0,
      loginToken: 'eyJraWQiOiJGY2hCY1B6U092REpCb0tFWTNSZjRGSkFkaElGSGNORGJYZ2tKZFh0azAwPSIsImFsZyI6IlJTMjU2In0.eyJzdWIiOiJiNDlhN2FjZC1kODQ5LTQyZTItOWI0Ny0zMDE1NGE3YjUzNTIiLCJpc3MiOiJodHRwczpcL1wvY29nbml0by1pZHAuYXAtbm9ydGhlYXN0LTEuYW1hem9uYXdzLmNvbVwvYXAtbm9ydGhlYXN0LTFfVzlucno3SkNoIiwiY2xpZW50X2lkIjoiM3M3ZjdvMDk4a2pzYTQ3NDhoNGhyZWw4MmIiLCJvcmlnaW5fanRpIjoiMDUzYzdhOWQtYjAwYS00ZjgwLTgxYTEtMWYzOWNjMGJhOGE2IiwiZXZlbnRfaWQiOiJlMjc2MTc1YS1mMTYzLTQxNDktOTEyZC04YjRkYmUwNjhiOGEiLCJ0b2tlbl91c2UiOiJhY2Nlc3MiLCJzY29wZSI6ImF3cy5jb2duaXRvLnNpZ25pbi51c2VyLmFkbWluIiwiYXV0aF90aW1lIjoxNjk3MDgwMjc1LCJleHAiOjE2OTcwODM4NzUsImlhdCI6MTY5NzA4MDI3NSwianRpIjoiOWVhMTIzZjYtYzdiNy00NjZlLTllZWMtOTk2ZjZmZjM3MTY5IiwidXNlcm5hbWUiOiJiNDlhN2FjZC1kODQ5LTQyZTItOWI0Ny0zMDE1NGE3YjUzNTIifQ.GELWsnDsCBuPXYjJxBNvZ2dmPemr41g-X9JYH-u0hrFmT6B6gW6vjthoH_TbQpwcEoebZSuvpDSlMInl2y9BUq8WnjjHqmXDpG22-l-g6VGqtL6iM1yzjQqGhA48Nq_q-6eVC0dCbjiSNqb_4mDGRy3_gXFPBva3xQ8VaygpnwNwMGjWhuaHZP5tOqk5OP53ZgzGPa9bhepqJ1NBy8RmT2N5YqOTHv_uZ7pd3jDYkr-_y_m5CmWU07YN2Wnkv4AxILfQ2_FxAiUsKNDlFgaPEo1PWxoreDAF-Uk5a7EuLIP3jCZCSKt-VsLYEfzYMpn1YyOkAikI9RI4tHU3IYfsrQ'
    };
  },
  props: {
    msg: String
  },
  created() {
    this.getCountNotice();
    this.getListNotice();
  },
  mounted() {
    const echo = new Echo({
      broadcaster: 'socket.io',
      host: 'http://localhost:8010',
      auth: {
        headers: {
          Authorization: `Bearer ${this.loginToken}`,
        },
      }
    });
   
    echo.join('notice.4')
    .listen('NoticeEvent', (notice) => {
        this.countNotice = notice.data.countNotice;
    })
  },
  methods: {
    getCountNotice() {
      axios.post('http://localhost:8010/api/v1/notice/count', [], {
        headers: {
          'Authorization': `Bearer ${this.loginToken}`
        }
      })
      .then(response => {
        console.log(response.data);
      })
      .catch(error => {
        console.error('Error:', error);
      });
    },
    readNotice() {
      axios.post('http://localhost:8010/api/v1/notice/read', [], {
        headers: {
          'Authorization': `Bearer ${this.loginToken}`
        }
      })
        .then(response => {
          console.log(response);
          this.getListNotice();
        })
        .catch(error => {
          console.error('API Error:', error);
        });
    },
    getListNotice() {
      axios.post('http://localhost:8010/api/v1/notice', [], {
        headers: {
          'Authorization': `Bearer ${this.loginToken}`
        }
      })
      .then(response => {
        this.list = response.data.data;
      })
      .catch(error => {
        console.error('Error:', error);
      });
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
