<template>
  <div id="app" :ref="refApp">
    <h1>Filter queries</h1>
    <b-input-group class="mb-2">
      <b-input-group-prepend is-text>
        <b-icon icon="search"></b-icon>
      </b-input-group-prepend>
      <b-form-input type="search" placeholder="Search query" v-model="filter"></b-form-input>
    </b-input-group>
    <b-table
      class="mb-5"
      tbody-tr-class="button" 
      head-variant="dark"
      striped 
      hover 
      :items="items" 
      :filter="filter" 
      :fields="fields"
      @row-clicked="onRowClicked">
    </b-table>

    <div v-for="item in items" v-bind:key="item.description">
      <h1 :ref="createRefName(item.description)">{{ item.description }}
        <b-icon icon="documents" class="border p-1 button" @click="copyQueryToClipboard(createRefName(item.description))"></b-icon>
        <b-icon icon="arrow-up" class="border p-1 button" @click="scrollMeTo(refApp)"></b-icon>
      </h1>
      <pre>{{ item.query }}</pre>
    </div>

  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      filter: '',
      fields: ['description'],
      items: [
          { description: 'All users', query: 'SELECT * FROM USERS;' },
          { description: 'Employee salary by id', query: 'SELECT salary FROM EMPLOYEE WHERE ID=?;' },
          { description: 'Actual date', query: 'SELECT sysdate FROM dual;' },
          { description: 'Most expensive employee', query: 'SELECT * FROM EMPLOYEE WHERE salary = (SELECT MAX(salary) FROM EMPLOYEE);' },
          { description: 'Employees in each department', query: 'SELECT department_id, sum(1) FROM EMPLOYEE GROUP BY department_id;' },
        ],
      refApp: 'refApp'
    }
  },
  methods: {
    copyQueryToClipboard(refName) {
      /* eslint-disable no-debugger */
      // debugger
      var element = this.$refs[refName][0].nextSibling
      element.classList.add('blink')
      this.$copyText(element.innerText)
      setTimeout(() => { 
        element.classList.remove('blink')
        }, 250)
    },
    createRefName(description) {
      return description.split(' ').join('_')
    },
    onRowClicked(item) {
      this.scrollMeTo(this.createRefName(item.description))
    },
    scrollMeTo(refName) {
      var element = this.$refs[refName]
      if (element.length)
        element = element[0]
      var top = element.offsetTop
      window.scrollTo(0, top)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.button {
  cursor: pointer;
}
.blink {
  background-color: gray;
  -webkit-transition: background-color 250ms linear;
  -ms-transition: background-color 250ms linear;
  transition: background-color 250ms linear;
}
</style>
