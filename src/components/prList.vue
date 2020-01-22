<template>
  <div>
    <li v-for="(listPr, index) in listPrs"
        v-bind:item="listPr "
        v-bind:index="index"
        v-bind:key="listPr.id">
      <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px', display: 'flex'}">
        <ul>
          Title PR: {{listPr.title}} <br/>
          Creator: <img :style="{ height: '15px' }" :src="listPr.user.avatar_url">
          {{listPr.user.login}} <br/>
          Created at: {{ listPr.created_at | formatDate }} <br/>
          <div v-for="(amountComment, index) in amountComments" :key=index>
            <div 
              v-bind:style="{ color: amountComment[0] > 3 && varningColor}"
              v-if="listPr.url === amountComment[1]">
             Have {{ amountComment[0] }} review comments
            </div>
          </div>
        </ul>
      </div>
    </li>
  </div>
</template>

<script>
import axios from 'axios'
var URL = 'https://api.github.com/orgs/evannetwork/repos?page=1&per_page=100'
export default {
  name: "prList",
  created: function () {
    this.fetchData()
  },
  data () {
    return {
      listPrs: [],
      amountComments: [],
      activeColor: 'blue',
      varningColor: 'red',
      fontSize: 20
    }
  },
  filters: {
    formatDate: function (v) {
      return v.replace(/T|Z/g, ' ')
    }
  },
  methods: {
    fetchData: function() {
      var xhr = new XMLHttpRequest()
      var self = this
      let repos = []
      xhr.open('GET', URL)
      xhr.onload = function () {
        repos = JSON.parse(xhr.responseText).filter(function (repo) {
          if (repo.open_issues > 0) return repo
        })

        for (let i = 0; i < repos.length; i++) {
          axios.get(repos[i].url + '/pulls')
          .then(function(response) {
            for (let j = 0; j < response.data.length; j++) {
                self.listPrs.push(response.data[j])
                axios.get(response.data[j].review_comments_url)
                .then(function(response) {
                    self.amountComments.push(response.data.length > 0 ? [response.data.length, response.data[0].pull_request_url] : [0, 0])
                })
            }
          })
          .catch((error) => {
            console.log(error)
          })
        }
      }
      xhr.send()
    }
  }
};
</script>



