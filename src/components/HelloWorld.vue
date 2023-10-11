<template>
  <div>
    <button @click="readNotice">{{ countNotice }}</button>
  </div>
</template>

<script>
import axios from 'axios';
import Echo from 'laravel-echo';


export default {
  name: 'HelloWorld',
  data() {
    return {
      countNotice: 0,
      loginToken: 'eyJraWQiOiJGY2hCY1B6U092REpCb0tFWTNSZjRGSkFkaElGSGNORGJYZ2tKZFh0azAwPSIsImFsZyI6IlJTMjU2In0.eyJzdWIiOiIxMzgxZDRjZi03ZTY2LTQ2NDktYTgxNC1hYjdhOTAwMTg3OTIiLCJpc3MiOiJodHRwczpcL1wvY29nbml0by1pZHAuYXAtbm9ydGhlYXN0LTEuYW1hem9uYXdzLmNvbVwvYXAtbm9ydGhlYXN0LTFfVzlucno3SkNoIiwiY2xpZW50X2lkIjoiM3M3ZjdvMDk4a2pzYTQ3NDhoNGhyZWw4MmIiLCJvcmlnaW5fanRpIjoiNTFjYjVmNzgtYzBhYS00NDY2LWFiM2QtYTQzNzFkYTYyYjQ1IiwiZXZlbnRfaWQiOiI2MjgyOTNkNi01YmE2LTRjMjQtYTNmMS0zODlhNWQzNzNjZDkiLCJ0b2tlbl91c2UiOiJhY2Nlc3MiLCJzY29wZSI6ImF3cy5jb2duaXRvLnNpZ25pbi51c2VyLmFkbWluIiwiYXV0aF90aW1lIjoxNjk3MDIyMjE2LCJleHAiOjE2OTcwMjU4MTYsImlhdCI6MTY5NzAyMjIxNiwianRpIjoiMDRjZDg2YzctZTNhOS00OWI1LWI3NmMtODI5MTU3YjExNmQyIiwidXNlcm5hbWUiOiIxMzgxZDRjZi03ZTY2LTQ2NDktYTgxNC1hYjdhOTAwMTg3OTIifQ.m7bn9GUFENojtcuZ4OCjITMEX3ty4kSN40A4_W1Ppf90_Ic3ns-tQiqb-nI_xAm9itFnlaVzbzfzOuSswVuDshMYxBVvanbX_ySK16GzgZxJ9sTD3gK5C9TN04iZI_6VvQr7Npwv29gYXBoVX6Uz66x2la1lZe7HOa2G2NLg2cMF9dEJNs2k9FbvAGogfu6Hr3XgGlAZsaTeB6kX2NEAujQRGIF6CY1dmQsVzTgif1kNVzpzPvFD-UKlHY5ZE6bf-3fSheTtbul6gkh9rJhxQhFL_EzZHE5gy613mpwTklnH5r2-2wzPvL5mWap0mZjZH8mMQgOc2Im1BU5DtVzkpA'
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
      host: 'http://localhost:6001'
    });
   
    echo.join('notice.1')
    .listen('NoticeEvent', (notice) => {
      if (notice.data.recciverUserId == 1) {
        this.countNotice = notice.data.countNotice;
      }
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
        console.log(response.data);
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
