<template>
  <div id="table-component">
    <div class="tableUser">
      <button v-on:click="statusModal = true">Добавить</button>
      <table class="table_sort">
        <thead>
        <tr>
          <th style="cursor:pointer;" @click="sortTable">Имя</th>
          <th style="cursor:pointer;" @click="sortTable">Номер</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="item in dataLocalStorage" v-bind:key="item.name" >
          <td>{{item.name}}
              <table>
                <tr v-for="items in item.employees" :key="items.name">
                  <td> + {{items.name}}</td>
                  <td>{{items.phone}}</td>
                </tr>
              </table>
          </td>
          <td>{{item.phone}}</td>
        </tr>
        </tbody>
      </table>
    </div>
    <div class="addUser">
      <add-user  v-on:close="statusModal = false" v-if="statusModal" v-on:add-user="loadDataAddUser" v-bind:dataLocalStorage="dataLocalStorage"/>
    </div>
  </div>
</template>

<script>
import AddUser from './AddUser.vue' 

export default {
  data () {
    return {
      dataLocalStorage: [],
      statusModal: false,
    }
  },
  mounted () {
    this.loadData()
    console.log(this.dataLocalStorage[0])
  },
  methods: {
    loadData () {
      for (let i = 0; i < localStorage.length - 1; i++) {
        this.dataLocalStorage.push(JSON.parse(localStorage.getItem(i)))
      }
    },
    loadDataAddUser (dataFormAddUser) {
      
      for (let i = 0; i < localStorage.length - 1; i++) {
        if (dataFormAddUser.boss == this.dataLocalStorage[i].name) {
          this.dataLocalStorage[i].employees.push(dataFormAddUser)
          localStorage.removeItem(i)
          localStorage.setItem(i, JSON.stringify(this.dataLocalStorage[i]))
        }
      }
      this.dataLocalStorage.push(dataFormAddUser)
    },
    sortTable () {
      const getSort = ({ target }) => {
        const order = (target.dataset.order = -(target.dataset.order || -1));
        const index = [...target.parentNode.cells].indexOf(target);
        const collator = new Intl.Collator(['en', 'ru'], { numeric: true });
        const comparator = (index, order) => (a, b) => order * collator.compare(
            a.children[index].innerHTML,
            b.children[index].innerHTML
        );
        
        for(const tBody of target.closest('table').tBodies)
            tBody.append(...[...tBody.rows].sort(comparator(index, order)));

        for(const cell of target.parentNode.cells)
            cell.classList.toggle('sorted', cell === target);
    };
    document.querySelectorAll('.table_sort').forEach(tableTH => tableTH.addEventListener('click', () => getSort(event)));
    }
  },
  components: {
    AddUser
  }
}
</script>

<style scoped>
  #table-component {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
  }
  .tableUser, .addUser {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    align-content: center;
    width: 100%;
  }
  table {
    
    border-collapse: collapse;
  }
  td {
    border: 1px solid black;
    padding: 10px;
  }
</style>